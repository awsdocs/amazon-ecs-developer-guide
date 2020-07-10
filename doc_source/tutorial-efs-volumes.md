# Tutorial: Using Amazon EFS file systems with Amazon ECS<a name="tutorial-efs-volumes"></a>

Amazon Elastic File System \(Amazon EFS\) provides simple, scalable file storage for use with your Amazon ECS tasks\. With Amazon EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files\. Your applications can have the storage they need, when they need it\.

You can use Amazon EFS file systems with Amazon ECS to access file system data across your fleet of Amazon ECS tasks\. That way, your tasks have access to the same persistent storage, no matter the infrastructure or container instance on which they land\. When you reference your Amazon EFS file system and container mount point in your Amazon ECS task definition, Amazon ECS takes care of mounting the file system in your container\. The following sections help you get started using Amazon EFS with Amazon ECS\.

This feature is supported by tasks that use both the EC2 and Fargate launch types, however this tutorial will use an Amazon ECS task that uses the EC2 launch type\. This tutorial is also meant to be followed step by step, however if you already have some of these resources created on your account then you may be able to skip some steps\.

**Note**  
Amazon EFS may not be available in all Regions\. For more information about which Regions support Amazon EFS, see [Amazon Elastic File System Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticfilesystem.html) in the *AWS General Reference*\.

## Step 1: Create an Amazon ECS cluster<a name="efs-create-cluster"></a>

Use the following steps to create an Amazon ECS cluster\. When you use the AWS Management Console to create a non\-empty cluster, Amazon ECS creates an AWS CloudFormation stack along with Auto Scaling resources\.

**To create an Amazon ECS cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. For **Select cluster compatibility**, choose **EC2 Linux \+ Networking** and then choose **Next step**\.

1. For **Cluster name**, enter `EFS-tutorial` for the cluster name\.

1. For **Provisioning model**, choose **On\-Demand Instance**\.

1. For **EC2 instance type**, choose `t2.micro`\.

1. For **Number of instances**, enter `1`\.

1. For **EC2 AMI Id**, choose the Amazon Linux 2 Amazon ECS\-optimized AMI\.

1. For **EBS storage \(GiB\)**, leave the default setting\.

1. For **Key pair**, choose an Amazon EC2 key pair to use with your container instances for SSH access\. This is required as you will connect to the instance later\.

1. In the **Networking** section, configure the VPC to launch your container instances into\. By default, the cluster creation wizard creates a new VPC with two subnets in different Availability Zones, and a security group open to the internet on port 80\. This is a basic setup that works well for an HTTP service\. However, you can modify these settings by following the steps below\.
**Important**  
Record the VPC and security group IDs you use for your cluster as you will need to create the Amazon EFS file system in the same VPC\.

   1. For **VPC**, create a new VPC, or select an existing VPC\.

   1. \(Optional\) If you choose to create a new VPC, for **CIDR Block**, select a CIDR block for your VPC\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

   1. For **Subnets**, select the subnets to use for your VPC\. If you choose to create a new VPC, you can keep the default settings or you can modify them to meet your needs\. If you choose to use an existing VPC, select one or more subnets in that VPC to use for your cluster\.

   1. For **Security group**, select the security group to attach to the container instances in your cluster\. If you choose to create a new security group, you can specify a CIDR block to allow inbound traffic from\. The default port 0\.0\.0\.0/0 is open to the internet\. You can also select a single port or a range of contiguous ports to open on the container instance\. For more complicated security group rules, you can choose an existing security group that you have already created\.
**Note**  
You can also choose to create a new security group and then modify the rules after the cluster is created\. For more information, see [Amazon EC2 Security Groups for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

   1. In the **Container instance IAM role** section, select the IAM role to use with your container instances\. If your account has the **ecsInstanceRole** that is created for you in the console first\-run wizard, it is selected by default\. If you do not have this role in your account, you can choose to create the role, or you can choose another IAM role to use with your container instances\.
**Important**  
The IAM role you use must have the `AmazonEC2ContainerServiceforEC2Role` managed policy attached to it, otherwise you will receive an error during cluster creation\. If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent does not connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. For **CloudWatch Container Insights**, deselect **Enable Container Insights** as this feature won't be needed for this tutorial\.

   1. Choose **Create**\.

## Step 2: Create a security group for the Amazon EFS file system<a name="efs-security-group"></a>

In this step, you create a security group for your Amazon EFS file system that allows inbound access from your container instances\. This security group will contain an inbound rule that references the security group you created, or referenced, for your cluster in the previous step\.

**To create a security group for an Amazon EFS file system**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation pane, choose **Security Groups**, **Create security group**\.

1. For **Security group name**, enter a unique name for your security group\. For example, `EFS-access-for-sg-dc025fa2`\.

1. For **Description**, enter a description for your security group\.

1. For **VPC**, choose the VPC that you identified earlier for your cluster\.

1. For **Inbound rules**, choose **Add rule**\.

1. For **Type**, choose **NFS**\.

1. For **Source**, choose **Custom** and then enter the security group ID that you identified earlier for your cluster\.

1. Choose **Create security group**\.

## Step 3: Create an Amazon EFS file system<a name="efs-create-filesystem"></a>

In this step, you create an Amazon EFS file system\.

**To create an Amazon EFS file system for Amazon ECS tasks\.**

1. Open the Amazon Elastic File System console at [https://console\.aws\.amazon\.com/efs/](https://console.aws.amazon.com/efs/)\.

1. Choose **Create file system**\.

1. On the **Configure network access** page, choose the VPC that your container instances are hosted in\. By default, each subnet in the specified VPC receives a mount target that uses the default security group for that VPC\.
**Important**  
Your Amazon EFS file system, your Amazon ECS cluster, container instances and tasks must be in the same VPC\.

1. Under **Create mount targets**, for **Security groups**, add the security group that you created in step 2\. Choose **Next Step**\.

1. On the **Configure file system settings** page, configure optional settings and then choose **Next Step** to proceed\.

   1. \(Optional\) Add tags for your file system\. For example, you could specify a unique name for the file system by entering that name in the **Value** column next to the **Name** key\.

   1. \(Optional\) Enable lifecycle management to save money on infrequently accessed storage\. For more information, see [EFS Lifecycle Management](https://docs.aws.amazon.com/efs/latest/ug/lifecycle-management-efs.html) in the *Amazon Elastic File System User Guide*\.

   1. Choose a throughput mode for your file system\. The **Bursting** mode is the default, and it is recommended for most file systems\.

   1. Choose a performance mode for your file system\. The **General Purpose** mode is the default, and it is recommended for most file systems\.

   1. \(Optional\) Enable encryption\. Select the check box to enable encryption of your Amazon EFS file system at rest\.

1. On the **Configure client access** page, choose **Next Step**\.

1. Review your file system options and choose **Create File System** to complete the process\.

1. From the file systems details screen, record the **File system ID**\. In the next step, you will reference this value in your Amazon ECS task definition\.

## Step 4: Add content to the Amazon EFS file system<a name="efs-add-content"></a>

In this step, you mount the Amazon EFS file system to an Amazon EC2 instance and add content to it\. This is for testing purposes in this tutorial, to illustrate the persistent nature of the data\. When using this feature you would normally have your application or another method of writing data to your Amazon EFS file system\.

**To create an Amazon EC2 instance and mount the Amazon EFS file system**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image** page, select the latest **Amazon Linux 2 AMI \(HVM\)** AMI\.

1. On the **Choose an Instance Type** page, keep the default instance type, `t2.micro` and choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, do the following:

   1. For **Network**, select the VPC that you specified for your Amazon EFS file system and Amazon ECS cluster\.

   1. For **Auto\-assign Public IP**, choose **Enable**\. Otherwise, your instances do not get public IP addresses or public DNS names\.

   1. For **File systems**, select your Amazon EFS file system\. You can optionally change the mount location or leave the default value\.

   1. Under **Advanced Details**, ensure that the user data script is populated automatically with the Amazon EFS file system mounting steps\.

1. Advance to step 5 of the instance wizard by choosing **Next: Add Storage**, **Next: Add Tags**, and **Next: Configure Security Group**\.

1. On the **Configure Security Group** page, choose **Select an existing security group** and select the security group that you created in step 1, and then choose **Review and Launch**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. On the **Select an existing key pair or create a new key pair** dialog box, select **Choose an existing key pair** and choose your key pair\. Select the acknowledgment check box, and choose **Launch Instances**\.

1. On the **Launch Status** page, choose **View Instances** to see the status of your instances\. Initially, their status is `pending`\. After the status changes to `running`, your instances are ready for use\.

Now, you connect to the Amazon EC2 instance and add content to the Amazon EFS file system\.

**To connect to the Amazon EC2 instance and add content to the Amazon EFS file system**

1. SSH to the Amazon EC2 instance you created\. For more information, see [Connect to Your Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. From the terminal window for each instance, run the df \-T command to verify that the Amazon EFS file system is mounted\. In the following output, we have highlighted the Amazon EFS file system mount\.

   ```
   $ df -T
   Filesystem     Type            1K-blocks    Used        Available Use% Mounted on
   devtmpfs       devtmpfs           485468       0           485468   0% /dev
   tmpfs          tmpfs              503480       0           503480   0% /dev/shm
   tmpfs          tmpfs              503480     424           503056   1% /run
   tmpfs          tmpfs              503480       0           503480   0% /sys/fs/cgroup
   /dev/xvda1     xfs               8376300 1310952          7065348  16% /
   127.0.0.1:/    nfs4     9007199254739968       0 9007199254739968   0% /mnt/efs/fs1
   tmpfs          tmpfs              100700       0           100700   0% /run/user/1000
   ```

1. Navigate to the directory that the Amazon EFS file system is mounted at\. In the example above, that is `/mnt/efs/fs1`\.

1. Create a file named `index.html` with the following content:

   ```
   <html>
       <body>
           <h1>It Works!</h1>
           <p>You are using an Amazon EFS file system for persistent container storage.</p>
       </body>
   </html>
   ```

## Step 5: Create a task definition<a name="efs-task-def"></a>

The following task definition creates a data volume named `efs-html`\. The `nginx` container mounts the host data volume at the NGINX root, `/usr/share/nginx/html`\.

**To create a new task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. On the **Select compatibilities** page, choose **EC2**, **Next step**\.

1. Choose **Configure via JSON**, copy and paste the following JSON text, replacing the `fileSystemId` with the ID of your Amazon EFS file system\.

   ```
   {
       "containerDefinitions": [
           {
               "memory": 128,
               "portMappings": [
                   {
                       "hostPort": 80,
                       "containerPort": 80,
                       "protocol": "tcp"
                   }
               ],
               "essential": true,
               "mountPoints": [
                   {
                       "containerPath": "/usr/share/nginx/html",
                       "sourceVolume": "efs-html"
                   }
               ],
               "name": "nginx",
               "image": "nginx"
           }
       ],
       "volumes": [
           {
               "name": "efs-html",
               "efsVolumeConfiguration": {
                   "fileSystemId": "fs-1324abcd",
                   "transitEncryption": "ENABLED"
               }
           }
       ],
       "family": "efs-tutorial"
   }
   ```

1. Choose **Save**, **Create**\.

## Step 6: Run a task and view the results<a name="efs-run-task"></a>

Now that your Amazon EFS file system is created and there is web content for the NGINX container to serve, you can run a task using the task definition that you created\. The NGINX web server serves your simple HTML page\. If you update the content in your Amazon EFS file system, those changes are propagated to any containers that have also mounted that file system\.

**To run a task and view the results**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster that you created in step 1 earlier\.

1. Choose **Tasks**, **Run new task**\.

1. For **Task Definition**, choose the `nginx-efs` task definition that you created earlier and choose **Run Task**\. For more information on the other options in the run task workflow, see [Running tasks](ecs_run_task.md)\.

1. Below the **Tasks** tab, choose the task that you just ran\.

1. Expand the container name at the bottom of the page, and choose the IP address that is associated with the container\. Your browser should open a new tab with the following message:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/it_works.png)
**Note**  
If you do not see the message, make sure that the security group for your container instance allows inbound network traffic on port 80\.