# Getting started with the new console using Linux containers on AWS Fargate<a name="getting-started-fargate"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage your containers\. You can host your containers on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks on AWS Fargate\. For a broad overview on Amazon ECS on Fargate, see [What is Amazon Elastic Container Service?](Welcome.md)\.

Get started with Amazon ECS on AWS Fargate by using the Fargate launch type for your tasks in the Regions where Amazon ECS supports AWS Fargate\.

Complete the following steps to get started with Amazon ECS on AWS Fargate\.

## Prerequisites<a name="first-run-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has administrator access\. For more information, see [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created the task execution role so that it is available on the account to be used\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\. 

## Step 1: Create the cluster<a name="get-started-windows-fargate-cluster"></a>

Create a cluster that uses the default VPC\.

Before you begin, assign the appropriate IAM permission\. For more information, see [Cluster examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies)\.

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter a unique name\.

   The name can contain up to 255 letters \(uppercase and lowercase\), numbers, and hyphens\.

1. \(Optional\) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

1. \(Optional\) To help identify your cluster, expand **Tags**, and then configure your tags\.

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Create**\.

## Step 2: Create a task definition<a name="get-started-fargate-task-def"></a>

A task definition is like a blueprint for your application\. Each time you launch a task in Amazon ECS, you specify a task definition\. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container\.

1. In the navigation pane, choose **Task Definitions**\.

1. Choose **Create new Task Definition**, **Create new revision with JSON**\.

1. Copy and paste the following example task definition into the box and then choose **Save**\.

   ```
   {
       "family": "sample-fargate", 
       "networkMode": "awsvpc", 
       "containerDefinitions": [
           {
               "name": "fargate-app", 
               "image": "public.ecr.aws/docker/library/httpd:latest", 
               "portMappings": [
                   {
                       "containerPort": 80, 
                       "hostPort": 80, 
                       "protocol": "tcp"
                   }
               ], 
               "essential": true, 
               "entryPoint": [
                   "sh",
   		"-c"
               ], 
               "command": [
                   "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
               ]
           }
       ], 
       "requiresCompatibilities": [
           "FARGATE"
       ], 
       "cpu": "256", 
       "memory": "512"
   }
   ```

1. Choose **Create**\.

## Step 3: Create the service<a name="create-windows-service"></a>

Create a service using the task definition\.

1. In the navigation pane, choose **Clusters**, and then select the cluster you created in [Step 1: Create the cluster](#get-started-windows-fargate-cluster)\.

1. From the **Services** tab, choose **Deploy**\.

1. Under **Deployment configuration**, specify how your application is deployed\.

   1. For **Task definition**, choose the task definition you created in [Step 2: Create a task definition](#get-started-fargate-task-def)\.

   1. For **Service name**, enter a name for your service\.

   1. For **Desired tasks**, enter **1**\.

1. Choose **Deploy**\.

## Step 4: View your service<a name="view-fargate-windows"></a>

1. On the **Service: *service\-name*** page, choose the **Tasks** tab\.

1. In the **Services** tab, choose the service you created in [Step 3: Create the service](#create-windows-service)\.

1. On the **Service: *service\-name*** page, choose the task ID for the task in your service\.

1. In the **Configuration** section, under **Public IP**, choose **open address**\.

1. You should see a webpage that displays the **Amazon ECS sample** application\.  
![\[Screenshot of the Amazon ECS sample application. The output indicates that "Your application is now running on Amazon ECS".\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Sample_Application.png)

## Step 5: Clean up<a name="windows-fargate-cleanup"></a>

When you are finished using an Amazon ECS cluster, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

Some Amazon ECS resources, such as tasks, services, clusters, and container instances, are cleaned up using the Amazon ECS console\. Other resources, such as Amazon EC2 instances, Elastic Load Balancing load balancers, and Auto Scaling groups, must be cleaned up manually in the Amazon EC2 console or by deleting the AWS CloudFormation stack that created them\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.

1. Choose **Delete Cluster**\. At the confirmation prompt, enter **delete *cluster\-name***, and then choose **Delete**\. Deleting the cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.