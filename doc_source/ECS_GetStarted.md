# Getting Started with Amazon ECS<a name="ECS_GetStarted"></a>

Get started with Amazon Elastic Container Service \(Amazon ECS\) by creating the Amazon ECS resources necessary to launch your first task\. The Amazon ECS console provides a first\-run experience that makes this easy\.

In the Regions that support AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with Amazon ECS using Fargate\. For more information, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

In the Regions that don't support AWS Fargate, the Amazon ECS first\-run wizard guides you through the process of getting started with tasks that use the EC2 launch type\. The wizard gives you the option of creating a cluster and launching a sample web application\. If you already have a Docker image to launch in Amazon ECS, you can create a task definition with that image and use that for your cluster instead\.

**Topics**
+ [Getting Started with Amazon ECS using Fargate](ECS_GetStarted_Fargate.md)
+ [Getting Started with Amazon ECS](ECS_GetStarted_EC2.md)
+ [Cleaning Up your Amazon ECS Resources](ECS_CleaningUp.md)