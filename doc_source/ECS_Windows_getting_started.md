# Getting started with Windows containers<a name="ECS_Windows_getting_started"></a>

This tutorial walks you through getting Windows containers running on Amazon ECS with the Amazon ECS\-optimized Windows Server AMI in the AWS Management Console\. You create a cluster for your Windows container instances, launch one or more container instances into your cluster, register a task definition that uses a Windows container image, create a service that uses that task definition, and then view the sample webpage that the container runs\.

**Topics**
+ [Step 1: Create a Windows cluster](#create_windows_cluster)
+ [Step 2: Register a Windows task definition](#register_windows_task_def)
+ [Step 3: Create a service with your task definition](#create_windows_service)
+ [Step 4: View your service](#view_windows_service)

## Step 1: Create a Windows cluster<a name="create_windows_cluster"></a>

You can create a new cluster for your Windows containers\. Amazon EC2 instances using the Linux Amazon ECS\-optimized AMIs cannot run Windows containers, and vice versa, so proper task placement is best accomplished by running Windows and Linux container instances in separate clusters\. In this tutorial, you create a cluster called `windows` and register one or more Amazon EC2 instances into the cluster for your Windows containers\.

**To create a cluster with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. Choose **EC2 Windows \+ Networking** and choose **Next step**\.

1. For **Cluster name** enter a name for your cluster \(in this example, `windows` is the name of the cluster\)\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. In the **Instance configuation** section, complete the following steps\.

   1. For **Provisioning model**, choose one of the following instance types:
      + **On\-Demand Instance**– With On\-Demand Instances, you pay for compute capacity by the hour with no long\-term commitments or upfront payments\.
      + **Spot**– Spot Instances allow you to bid on spare Amazon EC2 computing capacity for up to 90% off the On\-Demand price\. For more information, see [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)\.
**Note**  
Spot Instances are subject to possible interruptions\. We recommend that you avoid Spot Instances for applications that can't be interrupted\. For more information, see [Spot Instance Interruptions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html)\.

   1. For Spot Instances, do the following; otherwise, skip to the next step\.

      1. For **Spot Instance allocation strategy**, choose the strategy that meets your needs\. For more information, see [Spot Fleet Allocation Strategy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html#spot-fleet-allocation-strategy)\.

      1. For **Maximum bid price \(per instance/hour\)**, specify a bid price\. If your bid price is lower than the Spot price for the instance types that you selected, your Spot Instances are not launched\.

   1. For **EC2 instance type** page, choose the hardware configuration of your instance\. The instance type that you select determines the resources available for your tasks to run on\.

   1. For **Number of instances**, choose the number of Amazon EC2 instances to launch into your cluster\.

   1. For **EC2 AMI Id**, choose the Amazon ECS\-optimized AMI to use for your container instances\. The available AMIs will be determined by the Region and instance type you chose\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

   1. For **EBS storage \(GiB\)**, choose the size of the Amazon EBS volume to use for data storage on your container instances\. You can increase the size of the data volume to allow for greater image and container storage\.

   1. For **Key pair**, choose an Amazon EC2 key pair to use with your container instances for RDP access\. If you do not specify a key pair, you cannot connect to your container instances with RDP\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

1. In the **Networking** section, configure the VPC to launch your container instances into\. By default, the cluster creation wizard creates a new VPC with two subnets in different Availability Zones, and a security group open to the internet on port 80\. This is a basic setup that works well for an HTTP service\. However, you can modify these settings by following the substeps below\.

   1. For **VPC**, create a new VPC, or select an existing VPC\.

   1. \(Optional\) If you chose to create a new VPC, for **CIDR Block**, select a CIDR block for your VPC\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

   1. For **Subnets**, select the subnets to use for your VPC\. If you chose to create a new VPC, you can keep the default settings or you can modify them to meet your needs\. If you chose to use an existing VPC, select one or more subnets in that VPC to use for your cluster\.

   1. For **Security group**, select the security group to attach to the container instances in your cluster\. If you choose to create a new security group, you can specify a CIDR block to allow inbound traffic from\. The default port 0\.0\.0\.0/0 is open to the internet\. You can also select a single port or a range of contiguous ports to open on the container instance\. For more complicated security group rules, you can choose an existing security group that you have already created\.
**Note**  
You can also choose to create a new security group and then modify the rules after the cluster is created\. For more information, see [Amazon EC2 security groups for Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-security-groups.html) in the *Amazon EC2 User Guide for Windows Instances*\.

   1. In the **Container instance IAM role** section, select the IAM role to use with your container instances\. If your account has the **ecsInstanceRole** that is created for you in the console first\-run wizard, it is selected by default\. If you do not have this role in your account, you can choose to create the role, or you can choose another IAM role to use with your container instances\.
**Important**  
The IAM role you use must have the `AmazonEC2ContainerServiceforEC2Role` managed policy attached to it, otherwise you will receive an error during cluster creation\. If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent does not connect to your cluster\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. If you chose the Spot Instance type earlier, the **Spot Fleet Role IAM role** section indicates that an IAM role `ecsSpotFleetRole` is created\.

1. In the **Tags** section, specify the key and value for each tag to associate with the cluster\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. In the **CloudWatch Container Insights** section, choose whether to enable Container Insights for the cluster\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.

1. Choose **Create**\.
**Note**  
It can take up to 15 minutes for your Windows container instances to register with your cluster\.

## Step 2: Register a Windows task definition<a name="register_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple webpage on port 8080 of a container instance with the `microsoft/iis` container image\.

**To register the sample task definition with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. On the **Select launch type compatibilities** page, choose **EC2**, **Next step**\.
**Note**  
The Fargate launch type is not compatible with Windows containers\.

1. Scroll to the bottom of the page and choose **Configure via JSON**\.

1. Paste the sample task definition JSON below into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.

   ```
   {
     "family": "windows-simple-iis",
     "containerDefinitions": [
       {
         "name": "windows_sample_app",
         "image": "microsoft/iis",
         "cpu": 512,
         "entryPoint":["powershell", "-Command"],
         "command":["New-Item -Path C:\\inetpub\\wwwroot\\index.html -ItemType file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>' -Force ; C:\\ServiceMonitor.exe w3svc"],
         "portMappings": [
           {
             "protocol": "tcp",
             "containerPort": 80,
             "hostPort": 8080
           }
         ],
         "memory": 768,
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

## Step 3: Create a service with your task definition<a name="create_windows_service"></a>

After you have registered your task definition, you can place tasks in your cluster with it\. The following procedure creates a service with your task definition and places one task on your cluster\.

**To create a service from your task definition with the console**

1. On the **Task Definition: windows\-simple\-iis** registration confirmation page, choose **Actions**, **Create Service**\.

1. On the **Create Service** page, enter the following information and then choose **Create service**\.
   + **Launch type:** `EC2`
   + **Cluster:** windows
   + **Service name:** windows\-simple\-iis
   + **Service type:** `REPLICA`
   + **Number of tasks:** 1
   + **Deployment type:** Rolling update

**To create a service from your task definition with the AWS CLI**
+ Using the AWS CLI, run the following command to create your service\.

  ```
  aws ecs create-service --cluster windows --task-definition windows-simple-iis --desired-count 1 --service-name windows-simple-iis
  ```

## Step 4: View your service<a name="view_windows_service"></a>

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