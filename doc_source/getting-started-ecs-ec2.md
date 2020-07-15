# Getting started with Amazon ECS using Amazon EC2<a name="getting-started-ecs-ec2"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage your containers\. You can host your containers on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks using the Fargate launch type\. For more control, you can host your tasks on a cluster of Amazon EC2 instances that you manage by using the EC2 launch type\. For a broad overview on Amazon ECS, see [What is Amazon Elastic Container Service?](Welcome.md)\.

Get started with Amazon ECS using the EC2 launch type by registering a task definition, creating a cluster, and creating a service in the Amazon ECS console\.

**Important**  
For information about getting started with Amazon ECS using the Fargate launch type, see [Getting started with Amazon ECS using Fargate](getting-started-fargate.md)\.

Complete the following steps to get started with Amazon ECS using the EC2 launch type\.

## Prerequisites<a name="getting-started-ec2-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.

## Step 1: Register a task definition<a name="getting-started-ec2-task-def"></a>

A task definition is like a blueprint for your application\. Each time that you launch a task in Amazon ECS, you specify a task definition\. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container\. For more information about task definitions, see [Amazon ECS Task definitions](task_definitions.md)\.

The following steps walk you through creating a task definition that will deploy a simple web application\.

**To register a task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region you want to use\.

1. In the navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. On the **Select launch type compatibility** page, select **EC2** and choose **Next step**\.

1. On the **Configure task and container definitions** page, scroll down and choose **Configure via JSON**\.

1. Copy and paste the following example task definition into the box and then choose **Save**\.

   ```
   {
       "containerDefinitions": [
           {
               "entryPoint": [
                   "sh",
                   "-c"
               ],
               "portMappings": [
                   {
                       "hostPort": 80,
                       "protocol": "tcp",
                       "containerPort": 80
                   }
               ],
               "command": [
                   "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
               ],
               "cpu": 10,
               "memory": 300,
               "image": "httpd:2.4",
               "name": "simple-app"
           }
       ],
       "family": "console-sample-app-static"
   }
   ```

1. Choose **Create**\.

## Step 2: Create a cluster<a name="getting-started-ec2-cluster"></a>

An Amazon ECS cluster is a logical grouping of tasks, services, and container instances\. When creating a cluster using the console, Amazon ECS creates a AWS CloudFormation stack that takes care of the Amazon EC2 instance creation, networking and IAM configuration for you\. For more information about clusters, see [Amazon ECS clusters](clusters.md)\.

The following steps walk you through creating a cluster with one Amazon EC2 instance registered to it which will enable us to run a task on it\. If a specific field is not mentioned, leave the default value the console uses\.

**To create a cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the same Region you used in the previous step\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. On the **Select cluster template** page, choose **EC2 Linux \+ Networking**\.

1. For **Cluster name**, choose a name for your cluster\.

1. In the **Instance configuration** section, do the following:

   1. For **EC2 instance type**, choose either the **t2\.micro** or **t3\.micro** instance type to use for the container instance\. Instance types with more CPU and memory resources can handle more tasks, but that is unnecessary for this getting started guide\. For more information about the different instance types, see [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/)\.

   1. For **Number of instances**, type **1**\. Amazon EC2 instances incur costs while they exist in your AWS resources\. For more information, see [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing/)\.

   1. For **EC2 Ami Id**, use the default value which is the Amazon Linux 2 Amazon ECS\-optimized AMI\. For more information about the Amazon ECS\-optimized AMI, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

1. In the **Networking** section, for **VPC** choose either **Create a new VPC** to have Amazon ECS create a new VPC for the cluster to use, or choose an existing VPC to use\. For more information on creating your own VPC, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

1. In the **Container instance IAM role** section, choose **Create new role** to have Amazon ECS create a new IAM role for your container instances, or choose an existing Amazon ECS container instance \(`ecsInstanceRole`\) role that you have already created\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Choose **Create**\.

## Step 3: Create a Service<a name="getting-started-ec2-service"></a>

An Amazon ECS service enables you to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster\. If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it in order to maintain the desired number of tasks in the service\. For more information on services, see [Amazon ECS services](ecs_services.md)\.

**To create a service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the same Region you used in the previous step\.

1. In the navigation pane, choose **Clusters**\.

1. Select the cluster you created in the previous step\.

1. On the **Services** tab, choose **Create**\.

1. In the **Configure service** section, do the following:

   1. For **Launch type**, select **EC2**

   1. For **Task definition**, select the **console\-sample\-app\-static** task definition you created in step 1\.

   1. For **Cluster**, select the cluster you created in step 2\.

   1. For **Service name**, select a name for your service\.

   1. For **Number of tasks**, enter `1`\.

1. Use the default values for the rest of the fields and choose **Next step**\.

1. In the **Configure network** section, leave the default values and choose **Next step**\.

1. In the **Set Auto Scaling** section, leave the default value and choose **Next step**\.

1. Review the options and choose **Create service**\.

1. Choose **View service** to review your service\.

## Step 4: View your Service<a name="getting-started-ec2-view"></a>

The service is a web\-based application so you can view its containers with a web browser\.

**To view the service details**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the same Region you used in the previous step\.

1. In the navigation pane, choose **Clusters**\.

1. Select the cluster you created step 2\.

1. On the **Services** tab, choose the service you created in step 3\.

1. On the **Service: *service\-name*** page, choose the **Tasks** tab\.

1. Confirm that the task is in a **RUNNING** state\. If it is, select the task to view the task details\. If it is not in a **RUNNING** status, refresh the service details screen until it is\.

1. In the **Containers** section, expand the container details\. In the **Network bindings** section, for **External Link** you will see the **IPv4 Public IP** address to use to access the web application\.

1. Enter the **IPv4 Public IP** address in your web browser and you should see a webpage that displays the **Amazon ECS sample** application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Sample_Application.png)

## Step 5: Clean Up<a name="getting-started-ec2-cleanup"></a>

When you are finished using an Amazon ECS cluster, you can clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

The Amazon ECS resources created in this getting started guide, such as the cluster and service can be cleaned up using the Amazon ECS console\.

**To clean up the resources**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. Select the cluster you created step 2\.

1. On the **Services** tab, select the service you created in step 3 and choose **Delete**\. At the confirmation prompt, enter **delete me** and then choose **Delete**\.

1. On the cluster details page, choose **Delete cluster**\. At the confirmation prompt, enter **delete me** and then choose **Delete**\. Deleting the cluster cleans up the associated resources that were created with the cluster, including the VPC and Amazon EC2 instances\.