# Getting Started with Windows Containers<a name="ECS_Windows_getting_started"></a>

This tutorial walks you through manually getting Windows containers running on Amazon ECS\. You create a cluster for your Windows container instances, launch one or more container instances into your cluster, register a task definition that uses a Windows container image, create a service that uses that task definition, and then view the sample web page that the container runs\.


+ [Step 1: Create a Windows Cluster](#create_windows_cluster)
+ [Step 2: Launching a Windows Container Instance into your Cluster](#launch_windows_container_instance)
+ [Step 3: Register a Windows Task Definition](#register_windows_task_def)
+ [Step 4: Create a Service with Your Task Definition](#create_windows_service)
+ [Step 5: View Your Service](#view_windows_service)

## Step 1: Create a Windows Cluster<a name="create_windows_cluster"></a>

You should create a new cluster for your Windows containers\. Linux container instances cannot run Windows containers, and vice versa, so proper task placement is best accomplished by running Windows and Linux container instances in separate clusters\. In this tutorial, you create a cluster called `windows` for your Windows containers\.

**To create a cluster with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. Choose **EC2 Windows \+ Networking** and choose **Next step**\.

1. For **Cluster name** enter a name for your cluster \(in this example, `windows` is the name of the cluster\)\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. Choose **Create an empty cluster**, **Create**\.

**To create a cluster with the AWS CLI**

+ You can create a cluster using the AWS CLI with the following command:

  ```
  aws ecs create-cluster --cluster-name windows
  ```

## Step 2: Launching a Windows Container Instance into your Cluster<a name="launch_windows_container_instance"></a>

You can launch Windows container instance using the AWS Management Console, as described in this topic\. Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\. After you've launched your instance, you can use it to run tasks\.

**To launch a Windows container instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the region to use\.

1. From the console dashboard, choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, choose **Community AMIs**\.

1. Type **ECS\_Optimized** in the **Search community AMIs** field and press the **Enter** key\. Choose **Select** next to the **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2018\.02\.21** AMI\. 

   The current Amazon ECS\-optimized Windows AMI IDs by region are listed below for reference\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_Windows_getting_started.html)

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. The `t2.micro` instance type is selected by default\. The instance type that you select determines the resources available for your tasks to run on\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, set the **Auto\-assign Public IP** check box depending on whether to make your instance accessible from the public Internet\. If your instance should be accessible from the Internet, verify that the **Auto\-assign Public IP** field is set to **Enable**\. If your instance should not be accessible from the Internet, choose **Disable**\.
**Note**  
Container instances need external network access to communicate with the Amazon ECS service endpoint, so if your container instances do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) in the *Amazon VPC User Guide* and [HTTP Proxy Configuration](http_proxy_config.md) in this guide\. For help creating a VPC, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)

1. On the **Configure Instance Details** page, select the `ecsInstanceRole` **IAM role** value that you created for your container instances in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
**Important**  
If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent will not connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. <a name="windows-instance-launch-user-data-step"></a>Expand the **Advanced Details** section and paste the provided user data PowerShell script into the **User data** field\. By default, this script registers your container instance into the `windows` cluster that you created earlier\. To launch into another cluster instead of `windows`, replace the red text in the script below with the name of your cluster\.
**Note**  
The `-EnableTaskIAMRole` option is required to enable IAM roles for tasks\. For more information, see [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)\.

   ```
   <powershell>
   Import-Module ECSTools
   Initialize-ECSAgent -Cluster 'windows' -EnableTaskIAMRole
   </powershell>
   ```

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, configure the storage for your container instance\. The Windows OS and container images are quite large \(approximately 9 GiB for the Windows server core base layers\), and just a few images and containers quickly fill up the default 50 GiB volume size for the Amazon ECS\-optimized Windows AMI\. A larger root volume size \(for example, 200 GiB\) allows for more containers and images on your instance\.

   You can optionally increase or decrease the volume size for your instance to meet your application needs\.

1. Choose **Review and Launch**\.

1. On the **Review Instance Launch** page, under **Security Groups**, you'll see that the wizard created and selected a security group for you\. By default, you should have port 3389 for RDP connectivity\. If you want your containers to receive inbound traffic from the Internet, you need to open those ports as well\.

   1. Choose **Edit security groups**\.

   1. On the **Configure Security Group** page, ensure that the **Create a new security group** option is selected\.

   1. Add rules for any other ports that your containers may need \(the sample task definition later in this walk through uses port 8080, so you should open that to **Anywhere**\), and choose **Review and Launch**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, then select the key pair that you created when getting set up\. 

   When you are ready, select the acknowledgment field, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running`, and it receives a public DNS name\. \(If the **Public DNS** column is hidden, choose the **Show/Hide** icon and choose **Public DNS**\.\)

1. After your instance has launched, you can view your cluster in the Amazon ECS console to see that your container instance has registered with it\.
**Note**  
It can take up to 15 minutes for your Windows container instance to register with your cluster\.

## Step 3: Register a Windows Task Definition<a name="register_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple web page on port 8080 of a container instance with the `microsoft/iis` container image\.

**To register the sample task definition with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. Scroll to the bottom of the page and choose **Configure via JSON**\.

1. Paste the sample task definition JSON below into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.

   ```
   {
     "family": "windows-simple-iis",
     "containerDefinitions": [
       {
         "name": "windows_sample_app",
         "image": "microsoft/iis",
         "cpu": 100,
         "entryPoint":["powershell", "-Command"],
         "command":["New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"],
         "portMappings": [
           {
             "protocol": "tcp",
             "containerPort": 80,
             "hostPort": 8080
           }
         ],
         "memory": 500,
         "essential": true
       }
     ]
   }
   ```

1. Verify your information and choose **Create**\.

**To register the sample task definition with the AWS CLI**

1. Create a file called `windows-simple-iis.json`\.

1. Open the file with your favorite text editor and add the sample JSON above to the file and save it\.

1. Using the AWS CLI, run the following command to register the task definition with Amazon ECS\.
**Note**  
Make sure that your AWS CLI is configured to use the same region that your Windows cluster exists in, or add the `--region your_cluster_region` option to your command\.

   ```
   aws ecs register-task-definition --cli-input-json file://windows-simple-iis.json
   ```

## Step 4: Create a Service with Your Task Definition<a name="create_windows_service"></a>

After you have registered your task definition, you can place tasks in your cluster with it\. The following procedure creates a service with your task definition and places one task on your cluster\.

**To create a service from your task definition with the AWS Management Console**

1. On the **Task Definition: windows\-simple\-iis** registration confirmation page, choose **Actions**, **Create Service**\.

1. On the **Create Service** page, enter the following information and then choose **Create service**\.

   + **Cluster:** windows 

   + **Number of tasks:** 1

   + **Service name:** windows\-simple\-iis

**To create a service from your task definition with the AWS CLI**

+ Using the AWS CLI, run the following command to create your service\.

  ```
  aws ecs create-service --cluster windows --task-definition windows-simple-iis --desired-count 1 --service-name windows-simple-iis
  ```

## Step 5: View Your Service<a name="view_windows_service"></a>

After your service has launched a task into your cluster, you can view the service and open the IIS test page in a browser to verify that the container is running\.

**Note**  
It can take up to 15 minutes for your container instance to download and extract the Windows container base layers\.

**To view your service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, choose the **windows** cluster\.

1. In the **Services** tab, choose the **windows\-simple\-iis** service\.

1. On the **Service: windows\-simple\-iis** page, choose the task ID for the task in your service\.

1. On the **Task** page, expand the `iis` container to view its information\.

1. In the **Network bindings** of the container, you should see an **External Link** IP address and port combination link\. Choose that link to open the IIS test page in your browser\.  
![\[Windows simple IIS test page\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/windows-simple-iis.png)