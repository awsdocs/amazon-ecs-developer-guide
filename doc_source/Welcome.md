# What is Amazon Elastic Container Service?<a name="Welcome"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable and fast container management service\. You can use it to run, stop, and manage containers on a cluster\. With Amazon ECS, your containers are defined in a task definition that you use to run an individual task or task within a service\. In this context, a service is a configuration that you can use to run and maintain a specified number of tasks simultaneously in a cluster\. You can run your tasks and services on a serverless infrastructure that's managed by AWS Fargate\. Alternatively, for more control over your infrastructure, you can run your tasks and services on a cluster of Amazon EC2 instances that you manage\.

Amazon ECS provides the following features:
+ A serverless option with AWS Fargate\. With AWS Fargate, you don't need to manage servers, handle capacity planning, or isolate container workloads for security\. Fargate handles the infrastructure management aspects of your workload for you\. You can schedule the placement of your containers across your cluster based on your resource needs, isolation policies, and availability requirements\.
+ Integration with AWS Identity and Access Management \(IAM\)\. You can assign granular permissions for each of your containers\. This allows for a high level of isolation when building your applications\. In other words, you can launch your containers with the security and compliance levels that you've come to expect from AWS\.
+ AWS managed container orchestration\. As a fully managed service, Amazon ECS comes with AWS configuration and operational best practices built\-in\. This also means that you don't need to manage control plane, nodes, or add\-ons\. It's integrated with both Alexa Web Information Service and third\-party tools, such as Amazon Elastic Container Registry and Docker\. This integration makes it easier for teams to focus on building the applications, not the environment\.
+ Continuous integration and continuous deployment \(CI/CD\)\. This is a common process for microservice architectures that are based on Docker containers\. You can create a CI/CD pipeline that takes the following actions: 
  + Monitors changes to a source code repository
  + Builds a new Docker image from that source
  + Pushes the image to an image repository such as Amazon ECR or Docker Hub
  + Updates your Amazon ECS services to use the new image in your application
+ Support for service discovery\. This is a key component of most distributed systems and service\-oriented architectures\. With service discovery, your microservice components are automatically discovered as they're created and terminated on a given infrastructure\. 
+ Support for sending your container instance log information to CloudWatch Logs\. After you send this information to Amazon CloudWatch, you can view the logs from your container instances in one convenient location\. This prevents your container logs from taking up disk space on your container instances\.

The AWS container services team maintains a public roadmap on GitHub\. The roadmap contains information about what the teams are working on and enables AWS customers to provide direct feedback\. For more information, see [AWS Containers Roadmap](https://github.com/aws/containers-roadmap) on the GitHub website\.

## Launch types<a name="launch-types"></a>

There are two models that you can use to run your containers:
+ Fargate launch type \- This is a serverless pay\-as\-you\-go option\. You can run containers without needing to manage your infrastructure\.
+ EC2 launch type \- Configure and deploy EC2 instances in your cluster to run your containers\.

The Fargate launch type is suitable for the following workloads:
+ Large workloads that need to be optimized for low overhead 
+ Small workloads that have occasional burst
+ Tiny workloads
+ Batch workloads

The EC2 launch type is suitable for the following workloads:
+  Workloads that require consistently high CPU core and memory usage
+ Large workloads that need to be optimized for price
+ Your applications need to access persistent storage
+ You must directly manage your infrastructure

## Access Amazon ECS<a name="welcome-interfaces"></a>

You can create, access, and manage your Amazon ECS resources using any of the following interfaces:
+ **AWS Management Console** — Provides a web interface that you can use to access your Amazon ECS resources\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Amazon ECS\. It's supported on Windows, Mac, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS SDKs** — Provides language\-specific APIs and takes care of many of the connection details\. These include calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.
+ **AWS Copilot** — Provides an open\-source tool for developers to build, release, and operate production ready containerized applications on Amazon ECS\. For more information, see [AWS Copilot](https://github.com/aws/copilot-cli) on the GitHub website\.
+ **Amazon ECS CLI** — Provides a command line interface for you to run your applications on Amazon ECS and AWS Fargate using the Docker Compose file format\. You can quickly provision resources, push and pull images using Amazon Elastic Container Registry, and monitor running applications on Amazon ECS or Fargate\. You can also test containers that are running locally along with containers in the Cloud within the CLI\. For more information, see [Amazon ECS CLI](https://github.com/aws/amazon-ecs-cli) on the GitHub website\.
+ **AWS CDK** — Provides an open\-source software development framework that you can use to model and provision your cloud application resources using familiar programming languages\. The AWS CDK provisions your resources in a safe, repeatable manner through AWS CloudFormation\. For more information, see [Getting started with Amazon ECS using the AWS CDK](tutorial-ecs-web-server-cdk.md)\.

## Pricing<a name="welcome-pricing"></a>

Amazon ECS pricing is dependent on whether you use AWS Fargate or Amazon EC2 infrastructure to host your containerized workloads\. When using Amazon ECS on AWS Outposts, the pricing follows the same model that's used when you use Amazon EC2 directly\. For more information, see [Amazon ECS Pricing](https://aws.amazon.com/ecs/pricing)\.

Amazon ECS and Fargate also offer Savings Plans that provide significant savings based on your AWS usage\. For more information, see the *[Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)*\.

To view your bill, go to the **Billing and Cost Management Dashboard** in the [AWS Billing and Cost Management console](https://console.aws.amazon.com/billing/)\. Your bill contains links to usage reports that provide additional details about your bill\. To learn more about AWS account billing, see [AWS Account Billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

If you have questions concerning AWS billing, accounts, and events, [contact AWS Support](https://aws.amazon.com/contact-us/)\.

Trusted Advisor is a service that you can use to help optimize the costs, security, and performance of your AWS environment\. For more information about Trusted Advisor, see [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)\.