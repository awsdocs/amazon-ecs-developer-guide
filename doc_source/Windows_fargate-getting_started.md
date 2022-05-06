# Getting started with the classic Amazon ECS console using Windows containers on AWS Fargate<a name="Windows_fargate-getting_started"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage your containers\. You can host your containers on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks on AWS Fargate\. For a broad overview on Amazon ECS on Fargate, see [What is Amazon Elastic Container Service?](Welcome.md)\.

Get started with Amazon ECS on AWS Fargate by using the Fargate launch type for your tasks\. In the Regions where Amazon ECS supports AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with Amazon ECS using the Fargate launch type\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

## Prerequisites<a name="first-run-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.

The first\-run wizard attempts to automatically create the task execution IAM role, which is required for Fargate tasks\. To ensure that the first\-run experience is able to create this IAM role, one of the following must be true:
+ Your user has administrator access\. For more information, see [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created the task execution role so that it is available on the account to be used\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\. 

## Step 1: Create a cluster<a name="create_fargate_windows_cluster"></a>

You can create a new cluster called `windows`\.

**To create a cluster with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. Choose **Networking only** and choose **Next step**\.

1. For **Cluster name** enter a name for your cluster \(in this example, `windows` is the name of the cluster\)\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. In the **Networking** section, configure the VPC to launch your container instances into\. By default, the cluster creation wizard creates a new VPC with two subnets in different Availability Zones, and a security group open to the internet on port 80\. This is a basic setup that works well for an HTTP service\. However, you can modify these settings by following the substeps below\.

   1. To create a new VPC, select **CreateVPC**\.

   1. \(Optional\) If you chose to create a new VPC, for **CIDR Block**, enter a CIDR block for your VPC\. For more information, see [Your VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

   1. For **Subnet 1** and **Subnet 2**, enter the CIDR range for each subnet\.

1. In the **Tags** section, specify the key and value for each tag to associate with the cluster\. For more information, see [Tagging Your Amazon ECS Resources](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-using-tags.html)\.

1. In the **CloudWatch Container Insights** section, choose whether to enable Container Insights for the cluster\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.

1. Choose **Create**\.
**Note**  
It can take up to 15 minutes for your Windows container instances to register with your cluster\.

## Step 2: Register a Windows task definition<a name="register_fargate_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple webpage on port 8080 of a container instance with the `mcr.microsoft.com/windows/servercore/iis` container image\.

**To register the sample task definition with the AWS Management Console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **Task Definitions** page, choose **Create new Task Definition**\.

1. On the **Select launch type compatibilities** page, choose **Fargate**, **Next step**\.

1. Scroll to the bottom of the page and choose **Configure via JSON**\.

1. Paste the sample task definition JSON below into the text area \(replacing the pre\-populated JSON there\) and choose **Save**\.

   Use one of the following for `operatingSystemFamily`:
   + `WINDOWS_SERVER_2019_FULL`
   + `WINDOWS_SERVER_2019_CORE`

   ```
   {
       "containerDefinitions": [
           {
               "command": [
                   "New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"
               ],
               "entryPoint": [
                   "powershell",
                   "-Command"
               ],
               "essential": true,
               "cpu": 2048,
               "memory": 4096,      
               "image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
               "logConfiguration": {
                   "logDriver": "awslogs",
                   "options": {
                       "awslogs-group": "/ecs/fargate-windows-task-definition",
                       "awslogs-region": "us-east-1",
                       "awslogs-stream-prefix": "ecs"
                   }
               },
               "name": "sample_windows_app",
               "portMappings": [
                   {
                       "hostPort": 80,
                       "containerPort": 80,
                       "protocol": "tcp"
                   }
               ]
           }
       ],
       "memory": "4096",
       "cpu": "2048",
       "networkMode": "awsvpc",
       "family": "windows-simple-iis-2019-core",
       "executionRoleArn": "arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
       "runtimePlatform": {
           "operatingSystemFamily": "WINDOWS_SERVER_2019_CORE"
       },
       "requiresCompatibilities": [
           "FARGATE"
       ]
   }
   ```

1. Verify your information and choose **Create**\.

**To register the sample task definition with the AWS CLI**

1. Create a file called `windows_fargate_sample_app.json`\.

1. Open the file with your favorite text editor and add the sample JSON above to the file and save it\.

1. Using the AWS CLI, run the following command to register the task definition with Amazon ECS\.
**Note**  
Make sure that your AWS CLI is configured to use the same region that your Windows cluster exists in, or add the `--region your_cluster_region` option to your command\.

   ```
   aws ecs register-task-definition --cli-input-json file://windows_fargate_sample_app.json
   ```

## Step 3: Create a service with your task definition<a name="create_fargate_windows_service"></a>

After you have registered your task definition, you can place tasks in your cluster with it\. The following procedure creates a service with your task definition and places one task on your cluster\.

**To create a service from your task definition with the console**

1. On the **Task Definition: windows\_fargate\_sample\_app** registration confirmation page, choose **Actions**, **Create Service**\.

1. On the **Create Service** page, enter the following information and then choose **Create service**\.
   + **Launch type:** `Fargate`
   + **Platform operating system**: `WINDOWS_SERVER_2019_FULL` or `WINDOWS_SERVER_2019_CORE`
   + **Cluster:** windows
   + **Service name:** windows\_fargate\_sample\_app
   + **Service type:** `REPLICA`
   + **Number of tasks:** 1
   + **Deployment type:** Rolling update

**To create a service from your task definition with the AWS CLI**
+ Using the AWS CLI, run the following command to create your service\.

  ```
  aws ecs create-service --cluster windows --task-definition windows-simple-iis --desired-count 1 --service-name windows_fargate_sample_app
  ```

## Step 4: View your service<a name="view_windows_fargate_service"></a>

After your service has launched a task into your cluster, you can view the service and open the IIS test page in a browser to verify that the container is running\.

**Note**  
It can take up to 15 minutes for your container instance to download and extract the Windows container base layers\.

**To view your service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, choose the **windows** cluster\.

1. In the **Services** tab, choose the **windows\_fargate\_sample\_app** service\.

1. On the **Service: windows\_fargate\_sample\_app** page, choose the task ID for the task in your service\.

1. On the **Task** page, expand the `windows_fargate` container to view its information\.

1. In the **Network bindings** of the container, you should see an **External Link** IP address and port combination link\. Choose that link to open the IIS test page in your browser\.  
![\[Windows Fargate test page\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/windows-simple-iis.png)

## Step 5: Clean Up<a name="first-fargate-run-cleanup"></a>

When you are finished using an Amazon ECS cluster, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

Some Amazon ECS resources, such as tasks, services, clusters, and container instances, are cleaned up using the Amazon ECS console\. Other resources, such as Amazon EC2 instances, Elastic Load Balancing load balancers, and Auto Scaling groups, must be cleaned up manually in the Amazon EC2 console or by deleting the AWS CloudFormation stack that created them\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.

1. Choose **Delete Cluster**\. At the confirmation prompt, enter **delete me** and then choose **Delete**\. Deleting the cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.