# Linux platform versions<a name="platform-linux-fargate"></a>

## Platform version considerations<a name="platform-version-considerations"></a>

Consider the following when specifying a platform version:
+ When specifying a platform version, you can use either a specific version number, for example `1.4.0`, or `LATEST`\.

  When the **LATEST** platform version is selected, `1.4.0` platform version is used\.
+ In the China \(Beijing\) and China \(Ningxia\) Regions, the only supported platform versions are `1.4.0` and `1.3.0`\. The AWS Management Console displays older platform versions but an error will be returned if they are chosen\. The `LATEST` platform version is supported because it uses the `1.4.0` platform version\.
+ If you have a service with running tasks and want to update their platform version, you can update your service, specify a new platform version, and choose **Force new deployment**\. Your tasks are redeployed with the latest platform version\. 
+ If your service is scaled up without updating the platform version, those tasks receive the platform version that was specified on the service's current deployment\.

The following are the available Linux platform versions\. For information about platform version deprecation, see [AWS Fargate platform version deprecation](platform-versions-retired.md)\.

## 1\.4\.0<a name="platform-version-1-4"></a>

The following is the changelog for platform version `1.4.0`\.
+ Beginning on November 5, 2020, any new Amazon ECS task launched on Fargate using platform version `1.4.0` will be able to use the following features:
  + When using Secrets Manager to store sensitive data, you can inject a specific JSON key or a specific version of a secret as an environment variable or in a log configuration\. For more information, see [Using Secrets Manager to secure sensitive data](specifying-sensitive-data-secrets.md)\.
  + Specify environment variables in bulk using the `environmentFiles` container definition parameter\. For more information, see [Passing environment variables to a container](taskdef-envfiles.md)\.
  + Tasks run in a VPC and subnet enabled for IPv6 will be assigned both a private IPv4 address and an IPv6 address\. For more information, see [Fargate task networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
  + The task metadata endpoint version 4 provides additional metadata about your task and container including the task launch type, the Amazon Resource Name \(ARN\) of the container, and the log driver and log driver options used\. When querying the `/stats` endpoint you also receive network rate stats for your containers\. For more information, see [Task metadata endpoint version 4](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task-metadata-endpoint-v4-fargate.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ Beginning on July 30, 2020, any new Amazon ECS task launched on Fargate using platform version `1.4.0` will be able to route UDP traffic using a Network Load Balancer to their Amazon ECS on Fargate tasks\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ Beginning on May 28, 2020, any new Amazon ECS task launched on Fargate using platform version `1.4.0` will have its ephemeral storage encrypted with an AES\-256 encryption algorithm using an AWS owned encryption key\. For more information, see [Fargate task storage](fargate-task-storage.md)\.
+ Added support for using Amazon EFS file system volumes for persistent task storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.
+ The ephemeral task storage has been increased to a minimum of 20 GB for each task\. For more information, see [Fargate task storage](fargate-task-storage.md)\.
+ The network traffic behavior to and from tasks has been updated\. Starting with platform version 1\.4\.0, all Fargate tasks receive a single elastic network interface \(referred to as the task ENI\) and all network traffic flows through that ENI within your VPC and will be visible to you through your VPC flow logs\. For more information, see [Fargate Task Networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ Task ENIs add support for jumbo frames\. Network interfaces are configured with a maximum transmission unit \(MTU\), which is the size of the largest payload that fits within a single frame\. The larger the MTU, the more application payload can fit within a single frame, which reduces per\-frame overhead and increases efficiency\. Supporting jumbo frames will reduce overhead when the network path between your task and the destination supports jumbo frames, such as all traffic that remains within your VPC\.
+ CloudWatch Container Insights will include network performance metrics for Fargate tasks\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Added support for the task metadata endpoint version 4 which provides additional information for your Fargate tasks, including network stats for the task and which Availability Zone the task is running in\. For more information, see [Task metadata endpoint version 4](task-metadata-endpoint-v4.md)\.
+ Added support for the `SYS_PTRACE` Linux parameter in container definitions\. For more information, see [Linux parameters](task_definition_parameters.md#container_definition_linuxparameters)\.
+ The Fargate container agent replaces the use of the Amazon ECS container agent for all Fargate tasks\. Usually, this change does not have an effect on how your tasks run\.
+ The container runtime is now using Containerd instead of Docker\. Most likely, this change does not have an effect on how your tasks run\. You will notice that some error messages that originate with the container runtime changes from mentioning Docker to more general errors\. For more information, see [Stopped tasks error codes](https://docs.aws.amazon.com/AmazonECS/latest/userguide/stopped-task-error-codes.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ Based on Amazon Linux 2\.

## 1\.3\.0<a name="platform-version-1-3"></a>

The following is the changelog for platform version `1.3.0`\.
+ Beginning on Sept 30, 2019, any new Fargate task that is launched supports the `awsfirelens` log driver\. FireLens for Amazon ECS enables you to use task definition parameters to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics\. For more information, see [Custom log routing](using_firelens.md)\.
+ Added task recycling for Fargate tasks, which is the process of refreshing tasks that are a part of an Amazon ECS service\. For more information, [Task maintenance](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task-maintenance.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ Beginning on March 27, 2019, any new Fargate task that is launched can use additional task definition parameters that you use to define a proxy configuration, dependencies for container startup and shutdown as well as a per\-container start and stop timeout value\. For more information, see [Proxy configuration](task_definition_parameters.md#proxyConfiguration), [Container dependency](task_definition_parameters.md#container_definition_dependson), and [Container timeouts](task_definition_parameters.md#container_definition_timeout)\.
+ Beginning on April 2, 2019, any new Fargate task that is launched supports injecting sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.
+ Beginning on May 1, 2019, any new Fargate task that is launched supports referencing sensitive data in the log configuration of a container using the `secretOptions` container definition parameter\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.
+ Beginning on May 1, 2019, any new Fargate task that is launched supports the `splunk` log driver in addition to the `awslogs` log driver\. For more information, see [Storage and logging](task_definition_parameters.md#container_definition_storage)\.
+ Beginning on July 9, 2019, any new Fargate tasks that is launched supports CloudWatch Container Insights\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Beginning on December 3, 2019, the Fargate Spot capacity provider is supported\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.
+ Based on Amazon Linux 2\.

## 1\.2\.0<a name="platform-version-1-2"></a>

The following is the changelog for platform version `1.2.0`\.

**Note**  
Platform version `1.2.0` is deprecated\. We recommend migrating to the latest platform version\. For information about platform version deprecation, see [AWS Fargate platform version deprecation](platform-versions-retired.md)\.
+ Added support for private registry authentication using AWS Secrets Manager\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

## 1\.1\.0<a name="platform-version-1-1"></a>

The following is the changelog for platform version `1.1.0`\.

**Note**  
Platform version `1.1.0` is deprecated\. We recommend migrating to the latest platform version\. For information about platform version deprecation, see [AWS Fargate platform version deprecation](platform-versions-retired.md)\.
+ Added support for the Amazon ECS task metadata endpoint\. For more information, see [Amazon ECS task metadata endpoint](task-metadata-endpoint.md)\.
+ Added support for Docker health checks in container definitions\. For more information, see [Health check](task_definition_parameters.md#container_definition_healthcheck)\.
+ Added support for Amazon ECS service discovery\. For more information, see [Service Discovery](service-discovery.md)\.

## 1\.0\.0<a name="platform-version-1-0"></a>

The following is the changelog for platform version `1.0.0`\.

**Note**  
Platform version `1.0.0` is deprecated\. We recommend migrating to the latest platform version\. For information about platform version deprecation, see [AWS Fargate platform version deprecation](platform-versions-retired.md)\.
+ Based on Amazon Linux 2017\.09\.
+ Initial release\.

## Migrating to platform version 1\.4\.0<a name="platform-version-migration"></a>

Consider the following when migrating your Amazon ECS on Fargate tasks from platform version `1.0.0`, `1.1.0`, `1.2.0`, or `1.3.0` to platform version `1.4.0`\. It is considered best practice to confirm your task works properly on platform version `1.4.0` prior to migrating your tasks\.
+ The network traffic behavior to and from tasks has been updated\. Starting with platform version 1\.4\.0, all Amazon ECS on Fargate tasks receive a single elastic network interface \(referred to as the task ENI\) and all network traffic flows through that ENI within your VPC and will be visible to you through your VPC flow logs\. For more information, see [Fargate Task Networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ If you are using interface VPC endpoints, consider the following\.
  + When using container images hosted with Amazon ECR, both the **com\.amazonaws\.region\.ecr\.dkr** and **com\.amazonaws\.region\.ecr\.api** Amazon ECR VPC endpoints as well as the Amazon S3 gateway endpoint are required\. For more information, see [Amazon ECR interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html) in the *Amazon Elastic Container Registry User Guide*\.
  + When using a task definition that references Secrets Manager secrets to retrieve sensitive data for your containers, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
  + When using a task definition that references Systems Manager Parameter Store parameters to retrieve sensitive data for your containers, you must create the interface VPC endpoints for Systems Manager\. For more information, see [Using Systems Manager with VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) in the *AWS Systems Manager User Guide*\.
  + Ensure that the security group in the Elastic Network Interface \(ENI\) associated with your task has the security group rules created to allow traffic between the task and the VPC endpoints you are using\.