# Getting started with the new console using Windows containers on AWS Fargate<a name="Windows_fargate-getting_started"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage your containers\. You can host your containers on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks on AWS Fargate\. For a broad overview on Amazon ECS on Fargate, see [What is Amazon Elastic Container Service?](Welcome.md)\.

Get started with Amazon ECS on AWS Fargate by using the Fargate launch type for your tasks in the Regions where Amazon ECS supports AWS Fargate\.

Complete the following steps to get started with Amazon ECS on AWS Fargate\.

## Prerequisites<a name="first-run-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.

The first\-run wizard attempts to automatically create the task execution IAM role, which is required for Fargate tasks\. To ensure that the first\-run experience is able to create this IAM role, one of the following must be true:
+ Your user has administrator access\. For more information, see [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created the task execution role so that it is available on the account to be used\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\. 

## Step 1: Create a cluster<a name="create_fargate_windows_cluster-v2"></a>

You can create a new cluster called `windows` that uses the default VPC\.

**To create a cluster with the AWS Management Console**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter **windows**\.

1. \(Optional\) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

1. \(Optional\) To help identify your cluster, expand **Tags**, and then configure your tags\.

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Create**\.

## Step 2: Register a Windows task definition<a name="register_fargate_windows_task_def"></a>

Before you can run Windows containers in your Amazon ECS cluster, you must register a task definition\. The following task definition example displays a simple webpage on port 8080 of a container instance with the `mcr.microsoft.com/windows/servercore/iis` container image\.

**To register the sample task definition with the AWS Management Console**

1. In the navigation pane, choose **Task definitions**\.

1. Choose **Create new task definition**, **Create new task definition with JSON**\.

1. Copy and paste the following example task definition into the box and then choose **Save**\.

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

## Step 3: Create a service with your task definition<a name="create_fargate_windows_service"></a>

After you have registered your task definition, you can place tasks in your cluster with it\. The following procedure creates a service with your task definition and places one task on your cluster\.

**To create a service from your task definition with the console**

1. In the navigation pane, choose **Clusters**, and then select the cluster you created in [Step 1: Create a cluster](#create_fargate_windows_cluster-v2)\.

1. From the **Services** tab, choose **Deploy**\.

1. Under **Deployment configuration**, specify how your application is deployed\.

   1. For **Task definition**, choose the task definition you created in [Step 2: Create a task definition](getting-started-fargate.md#get-started-fargate-task-def)\.

   1. For **Service name**, enter a name for your service\.

   1. For **Desired tasks**, enter **1**\.

1. Choose **Deploy**\.

## Step 4: View your service<a name="view_windows_fargate_service"></a>

After your service has launched a task into your cluster, you can view the service and open the IIS test page in a browser to verify that the container is running\.

**Note**  
It can take up to 15 minutes for your container instance to download and extract the Windows container base layers\.

**To view your service**

1. On the **Clusters** page, choose the **windows** cluster\.

1. In the **Services** tab, choose the **windows\_fargate\_sample\_app** service\.

1. On the **Service: windows\_fargate\_sample\_app** page, choose the task ID for the task in your service\.

1. On the **Task** page, in the **Configuration** section, under **Public IP**, choose **open address**\.  
![\[Screenshot of the Amazon ECS sample application. The output indicates that "Your application is now running on Amazon ECS".\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/windows-simple-iis.png)

## Step 5: Clean Up<a name="first-fargate-run-cleanup"></a>

When you are finished using an Amazon ECS cluster, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

Some Amazon ECS resources, such as tasks, services, clusters, and container instances, are cleaned up using the Amazon ECS console\. Other resources, such as Amazon EC2 instances, Elastic Load Balancing load balancers, and Auto Scaling groups, must be cleaned up manually in the Amazon EC2 console or by deleting the AWS CloudFormation stack that created them\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.

1. Choose **Delete Cluster**\. At the confirmation prompt, enter **delete windows**, and then choose **Delete**\. Deleting the cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.