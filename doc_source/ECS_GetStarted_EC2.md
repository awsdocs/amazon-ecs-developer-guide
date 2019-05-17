# Getting Started with Amazon ECS<a name="ECS_GetStarted_EC2"></a>

Get started with Amazon Elastic Container Service \(Amazon ECS\) by creating a task definition that uses the EC2 launch type, scheduling tasks, and configuring a cluster in the Amazon ECS console\. For more information, see [Amazon ECS Launch Types](launch_types.md)\.

In the Regions that don't support AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with tasks that use the EC2 launch type\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

**Important**  
For information about the Amazon ECS first\-run wizard for Fargate tasks, see [Getting Started with Amazon ECS](ECS_GetStarted.md)\.

You can optionally create an Amazon Elastic Container Registry \(Amazon ECR\) image repository and push an image to it\. For more information, see the *[Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)*\.

Complete the following tasks to get started with Amazon ECS:

**Topics**
+ [Prerequisites](#first-run-ec2-prereqs)
+ [Step 1: Choose Your Configuration Options](#first-run-ec2-options)
+ [Step 2: Create a Task Definition](#first-run-ec2-task-def)
+ [Step 3: Configure the Service](#first-run-ec2-service)
+ [Step 4: Configure the Cluster](#first-run-ec2-cluster)
+ [Step 5: Review](#first-run-ec2-review)
+ [Step 6: \(Optional\) View your Service](#first-run-ec2-view)

## Prerequisites<a name="first-run-ec2-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) and that your AWS user has either the permissions specified in the `AdministratorAccess` or [Amazon ECS First Run Wizard](IAMPolicyExamples.md#first-run-permissions) IAM policy example\.

The first\-run wizard attempts to automatically create the Amazon ECS service IAM and container instance IAM role\. To ensure that the first\-run experience is able to create these IAM roles, one of the following must be true:
+ Your user has administrator access\. For more information, see [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.
+ Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
+ A user with administrator access has manually created these IAM roles so that they are available on the account to be used\. For more information, see [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md) and [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\. 

## Step 1: Choose Your Configuration Options<a name="first-run-ec2-options"></a>

1. Open the Amazon ECS console first\-run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. Select your Amazon ECS first\-run options\.  
![\[Choose your configuration options\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/choose-first-run-config.png)

   To create an Amazon ECS cluster and deploy a container application to it, check the top option\. To create an Amazon ECR repository and push an image to it, which you can use in your Amazon ECS task definitions, check the bottom option\. Choose **Continue**\.
**Important**  
If you are scoped to a Region that supports AWS Fargate then you won't see these options\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted_Fargate.md)\.

1. If you've chosen to create an Amazon ECR repository, then complete the next two sections of the first\-run wizard, **Configure repository ** and **Build, tag, and push Docker image **\. If you are not creating an Amazon ECR repository, skip ahead to [Step 2: Create a Task Definition](#first-run-ec2-task-def)\.

**Configure repository**

A repository is where you store Docker images in Amazon ECR\. Every time you push or pull an image from Amazon ECR, you specify the registry and repository location to tell Docker where to push the image to or where to pull it from\.

1. Choose **Get Started**\.

1. For **Repository configuration**, enter a unique name for your repository and choose **Create repository**\.

**Build, tag, and push Docker image**

In this section of the wizard, you use the Docker CLI to tag an existing local image \(that you have built from a Dockerfile or pulled from another registry, such as Docker Hub\) and then push the tagged image to your Amazon ECR registry\.

1. Retrieve the docker login command that you can use to authenticate your Docker client to your registry by pasting the aws ecr get\-login command from the console into a terminal window\. 
**Note**  
The get\-login command is available in the AWS CLI starting with version 1\.9\.15; however, we recommend version 1\.11\.91 or later for recent versions of Docker \(17\.06 or later\)\. You can check your AWS CLI version with the aws \-\-version command\. If you are using Docker version 17\.06 or later, include the `--no-include-email` option after `get-login`\. If you receive an `Unknown options: --no-include-email` error, install the latest version of the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.

1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way\. They could use the credentials to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

1. \(Optional\) If you have a Dockerfile for the image to push, build the image and tag it for your new repository\. Pasting the docker build command from the console into a terminal window\. Make sure that you are in the same directory as your Dockerfile\.

1. Tag the image for your ECR registry and your new repository by pasting the docker tag command from the console into a terminal window\. The console command assumes that your image was built from a Dockerfile in the previous step\. If you did not build your image from a Dockerfile, replace the first instance of `repository:latest` with the image ID or image name of your local image to push\.

1. Push the newly tagged image to your ECR repository by pasting the docker push command into a terminal window\.

1. Choose **Close**\.

## Step 2: Create a Task Definition<a name="first-run-ec2-task-def"></a>

A task definition is like a blueprint for your application\. Each time that you launch a task in Amazon ECS, you specify a task definition\. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container\.

1. Open the Amazon ECS console first\-run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. From the navigation bar, select the **US West \(N\. California\)** Region\.

1. Configure your task definition parameters\.

   The first\-run wizard comes preloaded with a task definition, and you can see the `simple-app` container defined in the console\. You can optionally rename the task definition or review and edit the resources used by the container \(such as CPU units and memory limits\)\. Choose the container name and editing the values shown \(CPU units are under the **Advanced options** menu\)\. Task definitions created in the first\-run wizard are limited to a single container for simplicity\. You can create multi\-container task definitions later in the Amazon ECS console\.

   For more information about what each of these task definition parameters does, see [Task Definition Parameters](task_definition_parameters.md)\.
**Note**  
If you are using an Amazon ECR image in your container definition, be sure to use the full `registry/repository:tag` naming for your Amazon ECR images\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.

1. Choose **Next**\.

## Step 3: Configure the Service<a name="first-run-ec2-service"></a>

In this section of the wizard, you select how you would like to configure the Amazon ECS service that is created from your task definition\. A service launches and maintains a specified number of copies of the task definition in your cluster\. The **Amazon ECS sample** application is a web\-based Hello World–style application that is meant to run indefinitely\. By running it as a service, it restarts if the task becomes unhealthy or unexpectedly stops\.

The first\-run wizard comes preloaded with a service definition, and you can see the `sample-app-service` service defined in the console\. You can optionally rename the service or review and edit the details by choosing **Edit** and doing the following:

1. For **Service name**, select a name for your service\.

1. For **Desired number of tasks**, enter the number of tasks to launch with your specified task definition\.

1. \(Optional\) You can choose to use an Application Load Balancer with your service\. When a task is launched from a service that is configured to use a load balancer, the task is registered with the load balancer\. Traffic from the load balancer is distributed across the instances in the load balancer\. For more information, see [Introduction to Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)\.
**Important**  
Application Load Balancers do incur cost while they exist in your AWS resources\. For more information, see [Application Load Balancer Pricing](http://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)\.

   1. Choose the **Application Load Balancer listener port**\. The default value here is set up for the sample application, but you can configure different listener options for the load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\.

   1. In the **Application Load Balancer target group name** field, specify a name for the target group\.

   1. In the **Service IAM Role** section, choose either an existing Amazon ECS service \(`ecsServiceRole`\) role that you have already created, or choose **Create new role** to create the required IAM role for your service\. For more information, see [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md)\.

1. Review your service settings and choose **Next step**\.

## Step 4: Configure the Cluster<a name="first-run-ec2-cluster"></a>

In this section of the wizard, you name your cluster\. Then, Amazon ECS takes care of the networking and IAM configuration for you\.

1. For **Cluster name**, choose a name for your cluster\.

1. For **EC2 instance type**, choose the instance type to use for your container instances\. Instance types with more CPU and memory resources can handle more tasks\. For more information about the different instance types, see [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/)\.

1. For **Number of instances**, type the number of Amazon EC2 instances to launch into your cluster for tasksplacement\. The more instances you have in your cluster, the more tasks you can place on them\. Amazon EC2 instances incur costs while they exist in your AWS resources\. For more information, see [Amazon EC2 Pricing](http://aws.amazon.com/ec2/pricing/)\.
**Note**  
If you created a service with more than one desired task in it that exposes container ports on to container instance ports, such as the **Amazon ECS sample** application, you must specify at least that many instances here\.

1. Select a key pair name to use with your container instances\. This is required for you to log into your instances with SSH\. If you do not specify a key pair here, you cannot connect to your container instances with SSH\. If you do not have a key pair, you can create one in the Amazon EC2 console\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\.

1. \(Optional\) In the **Security Group** section, you can choose a CIDR block that restricts access to your instances\. The default value \(**Anywhere**\) allows access from the entire internet\.

1. In the **Container instance IAM role** section, choose an existing Amazon ECS container instance \(`ecsInstanceRole`\) role that you have already created, or choose **Create new role** to create the required IAM role for your container instances\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Choose **Review & Launch**\.

## Step 5: Review<a name="first-run-ec2-review"></a>

1. Review your task definition, task configuration, and cluster configurations and click **Create** to finish\. You are directed to a **Launch Status** page that shows the status of your launch\. It describes each step of the process \(this can take a few minutes to complete while your Auto Scaling group is created and populated\)\.

1. After the launch is complete, choose **View service**\.

## Step 6: \(Optional\) View your Service<a name="first-run-ec2-view"></a>

If your service is a web\-based application, such as the **Amazon ECS sample** application, you can view its containers with a web browser\.

1. On the **Service: *service\-name*** page, choose **Tasks**\.

1. Choose a task from the list of tasks in your service\.

1. In the **Network** section, choose the **ENI Id** value for your task\. This takes you to the Amazon EC2 console where you can view the details of the network interface associated with your task, including the**IPv4 Public IP** address\.

1. Enter the **IPv4 Public IP** address in your web browser and you should see a webpage that displays the **Amazon ECS sample** application\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Sample_Application.png)