# Getting Started with Amazon ECS using Fargate<a name="ECS_GetStarted"></a>

Get started with Amazon Elastic Container Service \(Amazon ECS\) by creating a task definition that uses the Fargate launch type, scheduling tasks, and configuring a cluster in the Amazon ECS console\. 

In the Regions that support AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with Amazon ECS using Fargate\. For more information, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

**Important**  
For more information about the Amazon ECS first\-run wizard for EC2 tasks, see [Getting Started with Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted_EC2.html)\.

Complete the following tasks to get started with Amazon ECS using Fargate:
+ [Prerequisites](#first-run-prereqs)
+ [Step 1: Create a Task Definition](#first-run-task-def)
+ [Step 2: Configure the Service](#first-run-service)
+ [Step 3: Configure the Cluster](#first-run-cluster)
+ [Step 4: Review](#first-run-review)
+ [Step 5: \(Optional\) View your Service](#first-run-view)

## Prerequisites<a name="first-run-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or [Amazon ECS First Run Wizard](IAMPolicyExamples.md#first-run-permissions) IAM policy example\.

The first\-run wizard attempts to automatically create the task execution IAM role, which is required for Fargate tasks\. To ensure that the first\-run experience is able to create this IAM role, one of the following must be true:
+ Your user has administrator access\. For more information, see [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created the task execution role so that it is available on the account to be used\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\. 

## Step 1: Create a Task Definition<a name="first-run-task-def"></a>

A task definition is like a blueprint for your application\. Each time you launch a task in Amazon ECS, you specify a task definition\. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container\.

1. Open the Amazon ECS console first\-run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. From the navigation bar, select the **US East \(N\. Virginia\)** Region\.
**Note**  
You can complete this first\-run wizard using these steps for any Region that supports Amazon ECS using Fargate\. For more information, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\.

1. Configure your container definition parameters\. 

   For **Container definition**, the first\-run wizard comes preloaded with the `sample-app`, `nginx`, and `tomcat-webserver` container definitions in the console\. You can optionally rename the container or review and edit the resources used by the container \(such as CPU units and memory limits\) by choosing **Edit** and editing the values shown\. For more information, see [Container Definitions](task_definition_parameters.md#container_definitions)\.
**Note**  
If you are using an Amazon ECR image in your container definition, be sure to use the full `registry/repository:tag` naming for your Amazon ECR images\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.

1. For **Task definition**, the first\-run wizard defines a task definition to use with the preloaded container definitions\. You can optionally rename the task definition and edit the resources used by the task \(such as the **Task memory** and **Task CPU** values\) by choosing **Edit** and editing the values shown\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

   Task definitions created in the first\-run wizard are limited to a single container for simplicity\. You can create multi\-container task definitions later in the Amazon ECS console\.

1. Choose **Next**\.

## Step 2: Configure the Service<a name="first-run-service"></a>

In this section of the wizard, select how to configure the Amazon ECS service that is created from your task definition\. A service launches and maintains a specified number of copies of the task definition in your cluster\. The **Amazon ECS sample** application is a web\-based Hello World–style application that is meant to run indefinitely\. By running it as a service, it restarts if the task becomes unhealthy or unexpectedly stops\.

The first\-run wizard comes preloaded with a service definition, and you can see the `sample-app-service` service defined in the console\. You can optionally rename the service or review and edit the details by choosing **Edit** and doing the following:

1. In the **Service name** field, select a name for your service\.

1. In the **Number of desired tasks** field, enter the number of tasks to launch with your specified task definition\.

1. In the **Security group** field, specify a range of IPv4 addresses to allow inbound traffic from, in CIDR block notation\. For example, `203.0.113.0/24`\.

1. \(Optional\) You can choose to use an Application Load Balancer with your service\. When a task is launched from a service that is configured to use a load balancer, the task is registered with the load balancer\. Traffic from the load balancer is distributed across the instances in the load balancer\. For more information, see [Introduction to Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)\.
**Important**  
Application Load Balancers do incur cost while they exist in your AWS resources\. For more information, see [Application Load Balancer Pricing](http://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)\.

   Complete the following steps to use a load balancer with your service\.

   1. In the **Container to load balance** section, choose the **Load balancer listener port**\. The default value here is set up for the sample application, but you can configure different listener options for the load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\.

1. Review your service settings and click **Save**, **Next**\.

## Step 3: Configure the Cluster<a name="first-run-cluster"></a>

In this section of the wizard, you name your cluster, and then Amazon ECS takes care of the networking and IAM configuration for you\.

1. In the **Cluster name** field, choose a name for your cluster\.

1. Click **Next** to proceed\.

## Step 4: Review<a name="first-run-review"></a>

1. Review your task definition, task configuration, and cluster configuration and click **Create** to finish\. You are directed to a **Launch Status** page that shows the status of your launch\. It describes each step of the process \(this can take a few minutes to complete while your Auto Scaling group is created and populated\)\.

1. After the launch is complete, choose **View service**\.

## Step 5: \(Optional\) View your Service<a name="first-run-view"></a>

If your service is a web\-based application, such as the **Amazon ECS sample** application, you can view its containers with a web browser\.

1. On the **Service: *service\-name*** page, choose the **Tasks** tab\.

1. Choose a task from the list of tasks in your service\.

1. In the **Network** section, choose the **ENI Id** for your task\. This takes you to the Amazon EC2 console where you can view the details of the network interface associated with your task, including the **IPv4 Public IP** address\.

1. Enter the **IPv4 Public IP** address in your web browser and you should see a webpage that displays the **Amazon ECS sample** application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Sample_Application.png)
