# Tutorial: Using FSx for Windows File Server file systems with Amazon ECS<a name="tutorial-wfsx-volumes"></a>

FSx for Windows File Server provides fully managed Microsoft Windows file servers, that are backed by a fully native Windows file system\. When using FSx for Windows File Server together with ECS, you can provision your Windows tasks with persistent, distributed, shared, static file storage\. For more information, see [What Is FSx for Windows File Server?](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) in the *FSx for Windows File Server User Guide*\.

You can use FSx for Windows File Server to deploy Windows workloads that require access to shared external storage, highly available regional storage, or high\-throughput storage\. You can mount one or more FSx for Windows File Server file system volumes to an ECS container running on an ECS Windows instance\. You can share FSx for Windows File Server file system volumes among multiple ECS containers within a single ECS task\. 

**Note**  
FSx for Windows File Server might not be available in all Regions\. For more information about which Regions support FSx for Windows File Server, see [Amazon FSx Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/fsxn.html) in the *AWS General Reference*\.

In this tutorial, you launch an ECS Optimized Windows instance that hosts an FSx for Windows File Server file system and containers that can access the file system\. To do this, you first create an AWS Directory Service AWS Managed Microsoft Active Directory\. Then, you create an Amazon FSx for Windows File Server file system and an ECS cluster with an ECS instance and an ECS task definition\. You configure the task definition for your containers to use the FSx for Windows File Server file system\. Finally, you test the file system\.

It takes 20 to 45 minutes each time you launch or delete either the Active Directory or the FSx for Windows File Server file system\. Be prepared to reserve at least 90 minutes to complete the tutorial or complete the tutorial over a few sessions\.

## Prerequisites for the tutorial<a name="wfsx-prerequisites"></a>
+ An IAM Account with administrator access\. See [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ \(Optional\) A pem key pair for connecting to your EC2 Windows instance through RDP access\. For information about how to create key pairs, see [Amazon EC2 key pairs and Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html) in the *User Guide for Windows Instances*\.
+ A VPC with at least one public and one private subnet, and one security group\. You can use your default VPC\. You don't need a NAT gateway or device\. AWS Directory Service doesn't support Network Address Translation \(NAT\) with Active Directory\. For this to work, the Active Directory, FSx for Windows File Server file system, ECS Cluster, and ECS instance must be located within your VPC\. For more information regarding VPCs and Active Directories, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html) and [AWS Managed Microsoft AD Prerequisites](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started_prereqs.html)\.
+ The IAM ecsInstanceRole and ecsTaskExecutionRole permissions are associated with your account\. These service\-linked roles allow services to make API calls and access containers, secrets, directories and file servers on your behalf\.

## Step 1: Create IAM access roles<a name="iam-roles"></a>

**Create a cluster with the AWS Management Console\.**

1. See [Amazon ECS container instance IAM role](instance_IAM_role.md) to check whether you have an ecsInstanceRole and to see how you can create one if you don't have one\.

1. We recommend that role policies are customized for minimum permissions in an actual production environment\. For the purpose of working through this tutorial, verify that the following AWS managed policy is attached to your ecsInstanceRole\. Attach the policy if it is not already attached\.
   + AmazonEC2ContainerServiceforEC2Role

   To attach AWS managed policies\.

   1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

   1. In the navigation pane, choose **Roles\.**

   1. Choose an *`AWS managed role`*\.

   1. Choose **Permissions, Attach policies\.**\.

   1. To narrow the available policies to attach, use **Filter**\.

   1. Select the appropriate policy and choose **Attach policy**\.

1. See [Amazon ECS task execution IAM role](task_execution_IAM_role.md) to check whether you have an ecsTaskExecutionRole and to see how you can create one if you don't have one\.

   We recommend that role policies are customized for minimum permissions in an actual production environment\. For the purpose of working through this tutorial, verify that the following AWS managed policies are attached to your ecsTaskExecutionRole\. Attach the policies if they are not already attached\. Use the procedure given in the preceding section to attach the AWS managed policies\.
   + SecretsManagerReadWrite
   + AmazonFSxReadOnlyAccess
   + AmazonSSMReadOnlyAccess
   + AmazonECSTaskExecutionRolePolicy

## Step 2: Create Windows Active Directory \(AD\)<a name="wfsx-create-ads"></a>

1. Follow the steps described in [Create Your AWS Managed AD Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started_create_directory.html) in the AWS *Directory Service Administration Guide*\. Use the VPC you have designated for this tutorial\. On Step 3 of *Create Your AWS Managed AD Directory*, save the user name and password for use in a following step\. Also, note the fully qualified domain name for future steps\. You can go on to complete the following step while the Active Directory is being created\.

1. Create an AWS Secrets Manager secret to use in the following steps\. For more information, see [Getting Started with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/getting-started.html) in the AWS *Secrets Manager User Guide*\.

   1. Open the [Secrets Manager console](https://console.aws.amazon.com/secretsmanager/)\.

   1. Click **Store a new secret**\.

   1. Select **Other type of secrets**\.

   1. For **Secret key/value**, in the first row, create a key **username** with value **admin**\. Click on **\+ Add row**\.

   1. In the new row, create a key **password**\. For value, type in the password you entered in Step 3 of *Create Your AWS Managed AD Directory*\.

   1. Click on the **Next** button\.

   1. Provide a secret name and description\. Click **Next**\.

   1. Click **Next**\. Click **Store**\.

   1. From the list of **Secrets** page, click on the secret you have just created\.

   1. Save the ARN of the new secret for use in the following steps\.

   1. You can proceed to the next step while your Active Directory is being created\.

## Step 3: Verify and update your security group<a name="wfsx-sg"></a>

In this step, you verify and update the rules for the security group that you're using\. For this, you can use the default security group that was created for your VPC\.

**Verify and update security group\.**

You need to create or edit your security group to send data from and to the ports, which are described in [Amazon VPC Security Groups](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/limit-access-security-groups.html#fsx-vpc-security-groups) in the *FSx for Windows File Server User Guide*\. You can do this by creating the security group inbound rule shown in the first row of the following table of inbound rules\. This rule allows inbound traffic from network interfaces \(and their associated instances\) that are assigned to the security group\. All of the cloud resources you create are within the same VPC and attached to the same security group\. Therefore, this rule allows traffic to be sent to and from the FSx for Windows File Server file system, Active Directory, and ECS instance as required\. The other inbound rules allow traffic to serve the website and RDP access for connecting to your ECS instance\.

The following table shows which security group inbound rules are required for this tutorial\.


| Type | Protocol | Port range | Source | 
| --- | --- | --- | --- | 
|  All traffic  |  All  |  All  |  *sg\-securitygroup*  | 
|  Custom TCP  |  TCP  |  8080  |  0\.0\.0\.0/0  | 
|  RDP  |  TCP  |  3389  |  your EC2 instance public IP address  | 

The following table shows which security group outbound rules are required for this tutorial\.


| Type | Protocol | Port range | Destination | 
| --- | --- | --- | --- | 
|  All traffic  |  All  |  All  |  0\.0\.0\.0/0  | 

1. Open the [EC2 console](https://console.aws.amazon.com/ec2/) and select **Security Groups** from the left\-hand menu\.

1. From the list of security groups now displayed, select check the check\-box to the left of the security group that you are using for this tutorial\.

   Your security group details are displayed\.

1. Edit the inbound and outbound rules by selecting the **Inbound rules** or **Outbound rules** tabs and choosing the **Edit inbound rules** or **Edit outbound rules** buttons\. Edit the rules to match those displayed in the preceding tables\. After you create your EC2 instance later on in this tutorial, edit the inbound rule RDP source with the public IP address of your EC2 instance as described in [Connect to your Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) from the *Amazon EC2 User Guide for Windows Instances*\.

## Step 4: Create an FSx for Windows File Server file system<a name="wfsx-create-fsx"></a>

After your security group is verified and updated and your Active Directory is created and is in the active status, create the FSx for Windows File Server file system in the same VPC as your Active Directory\. Use the following steps to create an FSx for Windows File Server file system for your Windows tasks\.

**Create your first file system\.**

1. Open the [Amazon FSx console](https://console.aws.amazon.com/fsx/)\.

1. On the dashboard, choose **Create file system** to start the file system creation wizard\.

1. On the **Select file system type** page, choose **FSx for Windows File Server**, and then choose **Next**\. The **Create file system** page appears\.

1. In the **File system details** section, provide a name for your file system\. Naming your file systems makes it easier to find and manage your them\. You can use up to 256 Unicode characters\. Allowed characters are letters, numbers, spaces, and the special characters plus sign \(\+\)\. minus sign \(\-\), equal sign \(=\), period \(\.\), underscore \(\_\), colon \(:\), and forward slash \(/\)\.

1. For **Deployment type** choose **Single\-AZ** to deploy a file system that is deployed in a single Availability Zone\. *Single\-AZ 2* is the latest generation of single Availability Zone file systems, and it supports SSD and HDD storage\.

1. For **Storage type**, choose **HDD**\.

1. For **Storage capacity**, enter the minimum storage capacity\. 

1. Keep **Throughput capacity** at its default setting\.

1. In the **Network & security** section, choose the same Amazon VPC that you chose for your AWS Directory Service directory\.

1. For **VPC Security Groups**, choose the security group that you verified in *Step 3: Verify and update your security group*\.

1. For **Windows authentication**, choose **AWS Managed Microsoft Active Directory**, and then choose your AWS Directory Service directory from the list\.

1. For **Encryption**, keep the default **Encryption key** setting of **aws/fsx \(default\)**\.

1. Keep the default settings for **Maintenance preferences**\.

1. Click on the **Next** button\.

1. Review the file system configuration shown on the **Create file system** page\. For your reference, note which file system settings you can modify after file system is created\. Choose **Create file system**\. 

1. Note the file system ID\. You will need to use it in a later step\.

   You can go on to the next steps to create a cluster and EC2 instance while the FSx for Windows File Server file system is being created\.

## Step 5: Create an Amazon ECS cluster<a name="wfsx-create-cluster"></a>

**Create a cluster using the AWS Management Console\.**

1. Open the [Amazon ECS console](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. Choose **EC2 Windows \+ Networking** and choose **Next step**\.

1. For **Cluster name** enter `windows-fsx-cluster`\.

1. Click the check\-box under the name of the cluster to **create an empty cluster**\.

1. Click the **Create** button on the lower right corner\.

1. Click the **View Cluster** button when the cluster is successfully created\.

   You are now on a page where you can view the details of your cluster\.

## Step 6: Create an Amazon ECS instance<a name="wfsx-create-instance"></a>

**Launch an ECS Optimized Windows EC2 instance into the ECS cluster you just created using the AWS Management Console\.**

1. Go to [Amazon ECS\-optimized AMI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_windows_AMI.html) in the *Amazon ECS Developer Guide* to find the latest version of the Windows Server 2019 Full AMI in the same Region as your VPC\.

1. You can get the latest version using one of the following steps\.

   Scroll down to the Windows Server 2019 Full AMI table\.

   1. Find the latest version in the table for your Region\. Click **View AMI ID** link to a page where you'll find the AMI ID of the latest version\. Save a copy of the AMI ID for the next steps\.

   1. Run the given Systems Manager command using the AWS CLI and save a copy of the AMI ID that is returned\.

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\.

1. Click on the **Launch Instance** button and select **Launch Instance**\.

   You are now on a page that lists available EC2 instances\.

1. 

**Select an AMI for your EC2 instance\.**

   1. Under **Quick Start**, click on **Community AMIs**\.

   1. In the **search** field, enter the AMI ID that you saved from the previous step and press return\.

   1. Select the Windows Server 2019 Full AMI that matches the AMI ID that you saved in the previous step\.

      You are now on a page listing instance types\.

1. For **Instance type** page, choose t2\.medium or t2\.micro and click on **Next: Configure Instance Details**\.

1. 

**Configure instance details\.**

   1. On the **Configure Instance Details** page, enter 1 for **Number of Instances**\.

   1. For **Network** select your VPC\.

   1. For **Subnets** select a public subnet\.

   1. Select **Enable** for **Auto\-assign Public IP**\.

   1. For **Domain join directory**, select the ID of the Active Directory that you created\. This option domain joins your AD when the EC2 instance is launched\.

   1. For **IAM role**, select your **ecsInstanceRole** from the drop\-down menu\.

   1. Scroll to the bottom of the page and enter the following into the **User data** text field\.

      ```
      <powershell>
      Initialize-ECSAgent -Cluster windows-fsx-cluster -EnableTaskIAMRole
      </powershell>
      ```

   1. Click **Next: Add Storage** button\.

   1. Click **Next: Add Tags** button\.

   1. Click **Next: Configure Security Group** button\.

1. On the **Configure Security Group** page, select the security group that you verified and updated in *Step 3: Verify and update your security group*\. If it doesn't already exist, add an inbound RDP TCP rule to allow traffic from your EC2 instance IP address through port 3389 if you want to be able to RDP into your instance\.

1. Click on **Review and Launch** button\.

1. On the **Review Instance Launch** page, click the **Launch** button\.

1. For **Key pair**, choose an Amazon EC2 key pair to use with your container instances for RDP access\. If you don't specify a key pair, you can't connect to your container instances with RDP\. For more information, see [Prerequisites for the tutorial](#wfsx-prerequisites)\.

1. Click on **View instance** to see the new instance status among your list of instances\.

1. Open the [Amazon ECS console](https://console.aws.amazon.com/ecs/) and select **Clusters**\.

1. Select your **fsx\-windows\-cluster** cluster\.

1. Select the **ECS Instances** tab and verify that your ECS instance has been registered in the **fsx\-windows\-cluster** cluster\.

## Step 7: Register a Windows task definition<a name="register_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple web page on port 8080 of a container instance\. The task launches two containers that have access to the FSx file system\. The first container writes an HTML file to the file system\. The second container downloads the HTML file from the file system and serves the webpage\.

**Register the sample task definition with the AWS Management Console\.**

1. Open the [Amazon ECS console](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. On the **Select launch type compatibilities** page, choose **EC2** and then **Next step**\.
**Note**  
The Fargate launch type isn't compatible with Windows containers\.

1. On the **Create new Task Definition** page, scroll to the bottom of the page and choose **Configure via JSON**\.

1. Paste the following sample task definition JSON into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.

   ```
   {
     "containerDefinitions": [
       {
         "entryPoint": [
           "powershell",
           "-Command"
         ],
         "portMappings": [],
         "command": [
           "New-Item -Path C:\\fsx-windows-dir\\index.html -ItemType file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>It Works!</h2> <p>You are using Amazon FSx for Windows File Server file system for persistent container storage.</p>' -Force"
         ],
         "cpu": 512,
         "memory": 256,
         "image": "mcr.microsoft.com/windows/servercore/iis",
         "essential": false,
         "name": "container1"
       },
       {
         "entryPoint": [
           "powershell",
           "-Command"
         ],
         "portMappings": [
             {
               "hostPort": 8080,
               "protocol": "tcp",
               "containerPort": 80
             }
         ],
         "command": [
           "Remove-Item -Recurse C:\\inetpub\\wwwroot\\* -Force; Start-Sleep -Seconds 120; Move-Item -Path C:\\fsx-windows-dir\\index.html -Destination C:\\inetpub\\wwwroot\\index.html -Force; C:\\ServiceMonitor.exe w3svc"
         ],
         "cpu": 512,
         "memory": 256,
         "image": "mcr.microsoft.com/windows/servercore/iis",
         "essential": true,
         "name": "container2"
       }
     ],
     "family": "fsx-windows"
   }
   ```

1. Directly above the **Configure via JSON** button, click the **plus sign** to the left of **Add volume**\.

   1. For **Volume type**, select **FSx for Windows File Server**\.

   1. For **Name**, enter **fsx\-windows\-vol** and save it for following steps\.

   1. For **File system ID**, select the ID of the FSx for Windows File Server file system that you created in preceding steps\.

   1. For **Root Directory**, enter **share**\.

   1. For **Credentials parameter**, enter the ARN of the secret you created by using the Secrets Manager in the preceding steps\.

   1. For **Domain**, enter your Active Directory fully qualified domain name\.

1. Click on **container1** under **Container Definitions**\.

1. Scroll to **STORAGE AND LOGGING** and, for **Mount points*** Source volume*, select **fsx\-windows\-vol** from the drop\-down menu\.

1. For **Container path**, enter **C:\\fsx\-windows\-dir**\.

1. Click on **Update** button\.

1. Repeat the last four steps for **container2** under **Container Definitions**\.

1. For **Task execution role**, choose your **ecsTaskExecutionRole** from the drop\-down menu\.

1. Verify your information and click on **Create** button\.

## Step 8: Run a task and view the results<a name="wfsx-run-task"></a>

Before running the task, verify that the status of your FSx for Windows File Server file system is **Available**\. After it is available, you can run a task using the task definition that you created\. The task starts out by creating containers that shuffle an HTML file between them using the file system\. After the shuffle, a web server serves the simple HTML page\.

**Note**  
You might not be able to connect to the website from within a VPN\.

**Run a task and view the results\.**

1. Open the [Amazon ECS console](https://console.aws.amazon.com/ecs/)\.

1. Choose your **fsx\-windows\-cluster** cluster\.

1. Choose **Tasks** tab, and then **Run new task**\.

1. For **Launch Type**, select EC2\.

1. For **Task Definition**, choose the **fsx\-windows** task definition that you created, and then choose **Run Task**\.

1. Under the **Tasks** tab, choose the task that you just ran\. Your task appears in the list of tasks\.

1. When your task status is **RUNNING**, click on the task ID\.

1. Expand container2\.

1. Scroll down and click on the external IP address that is associated with the container\. Your browser will open and display the following message\.
**Note**  
If you don't see this message, check that you aren't running in a VPN and make sure that the security group for your container instance allows inbound network HTTP traffic on port 8080\.

## Step 9: Clean up<a name="wfsx-cleanup"></a>

**Note**  
It takes 20 to 45 minutes to delete the FSx for Windows File Server file system or the AD\. You must wait until the FSx for Windows File Server file system delete operations are complete before starting the AD delete operations\.

**Remove FSx for Windows File Server file system\.**

1. Open the [Amazon FSx console](https://console.aws.amazon.com/fsx/)

1. Click the radio button to the left of the FSx for Windows File Server file system that you just created\.

1. Click on **Actions**\.

1. Select **Delete file system**\.

**Remove AD\.**

1. Open the [AWS Directory Service console](https://console.aws.amazon.com/directoryservicev2/)\.

1. Click the radio button to the left of the AD you just created\.

1. Click on **Actions**\.

1. Select **Delete directory**\.

**Remove ECS cluster\.**

1. Open the [Amazon ECS console](https://console.aws.amazon.com/ecs/)\.

1. Select **Clusters**\.

1. Select the cluster you just created\.

1. Click the **Delete Cluster** button\.

**Remove ECS instance\.**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\.

1. From the left\-hand menu, select **Instances**\.

1. Check the check\-box to the left of the EC2 instance you created during this exercise\.

1. Click the **Instance state** and then **Terminate instance**\.

**Remove secret\.**

1. Open the [Secrets Manager console](https://console.aws.amazon.com/secretsmanager/)\.

1. Select the secret you created for this walk through\.

1. Click **Actions**\.

1. Select **Delete secret**\.