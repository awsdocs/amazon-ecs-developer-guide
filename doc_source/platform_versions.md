# AWS Fargate platform versions<a name="platform_versions"></a>

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure\. It is a combination of the kernel and container runtime versions\. 

New platform versions are released as the runtime environment evolves, for example, if there are kernel or operating system updates, new features, bug fixes, or security updates\. Security updates and patches are deployed automatically for your Fargate tasks\. If a security issue is found that affects a platform version, AWS patches the platform version\. In some cases, you may be notified that your Fargate tasks have been scheduled for retirement\. For more information, see [Task retirement](task-retirement.md)\.

**Topics**
+ [Platform version considerations](#platform-version-considerations)
+ [Available AWS Fargate platform versions](#available_pv)
+ [Migrating to platform version 1\.4\.0](#platform-version-migration)
+ [AWS Fargate platform versions scheduled for deprecation](platform-versions-retired.md)

## Platform version considerations<a name="platform-version-considerations"></a>

The following should be considered when specifying a platform version:
+ When specifying a platform version, you can use either a specific version number, for example `1.4.0`, or `LATEST` \(which uses the `1.3.0` platform version\)\.
+ To use a specific platform version, specify the version number when creating or updating your service\. If you specify `LATEST`, your tasks use platform version `1.3.0`\.
+ In the China \(Beijing\) and China \(Ningxia\) Regions, the only supported platform versions are `1.4.0` and `1.3.0`\. The AWS Management Console displays older platform versions but an error will be returned if they are chosen\. The `LATEST` platform version is supported because it uses the `1.3.0` platform version\.
+ If you have a service with running tasks and want to update their platform version, you can update your service, specify a new platform version, and choose **Force new deployment**\. Your tasks are redeployed with the latest platform version\. For more information, see [Updating a service](update-service.md)\.
+ If your service is scaled up without updating the platform version, those tasks receive the platform version that was specified on the service's current deployment\.

## Available AWS Fargate platform versions<a name="available_pv"></a>

The following is a list of the platform versions currently available:

Fargate platform version‐1\.4\.0  
+ Beginning on July 30, 2020, any new Fargate task that is launched using platform version 1\.4\.0 will be able to route UDP traffic using a Network Load Balancer to their Amazon ECS on Fargate tasks\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ Beginning on May 28, 2020, any new Fargate task that is launched using platform version 1\.4\.0 will have its ephemeral storage encrypted with an AES\-256 encryption algorithm using an AWS Fargate\-managed encryption key\. For more information, see [Fargate Task Storage](fargate-task-storage.md)\.
+ Added support for using Amazon EFS file system volumes for persistent task storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.
+ The ephemeral task storage has been increased to a minimum of 20 GB for each task\. For more information, see [Fargate Task Storage](fargate-task-storage.md)\.
+ The network traffic behavior to and from tasks has been updated\. Starting with platform version 1\.4\.0, all Fargate tasks receive a single elastic network interface \(referred to as the task ENI\) and all network traffic flows through that ENI within your VPC and will be visible to you through your VPC flow logs\. For more information, see [Fargate Task Networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ Task ENIs add support for jumbo frames\. Network interfaces are configured with a maximum transmission unit \(MTU\), which is the size of the largest payload that fits within a single frame\. The larger the MTU, the more application payload can fit within a single frame, which reduces per\-frame overhead and increases efficiency\. Supporting jumbo frames will reduce overhead when the network path between your task and the destination supports jumbo frames, such as all traffic that remains within your VPC\.
+ CloudWatch Container Insights will include network performance metrics for Fargate tasks\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Added support for the task metadata endpoint version 4 which provides additional information for your Fargate tasks, including network stats for the task and which Availability Zone the task is running in\. For more information, see [Task metadata endpoint version 4](task-metadata-endpoint-v4.md)\.
+ Added support for the `SYS_PTRACE` Linux parameter in container definitions\. For more information, see [Linux Parameters](task_definition_parameters.md#container_definition_linuxparameters)\.
+ The Fargate container agent replaces the use of the Amazon ECS container agent for all Fargate tasks\. This change should not have an effect on how your tasks run\.
+ The container runtime is now using Containerd instead of Docker\. This change should not have an effect on how your tasks run\. You will notice that some error messages that originate with the container runtime will change from mentioning Docker to more general errors\. For more information, see [Stopped tasks error codes](https://docs.aws.amazon.com/AmazonECS/latest/userguide/stopped-task-error-codes.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

Fargate platform version‐1\.3\.0  
+ Beginning on Sept 30, 2019, any new Fargate task that is launched supports the `awsfirelens` log driver\. FireLens for Amazon ECS enables you to use task definition parameters to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics\. For more information, see [Custom log routing](using_firelens.md)\.
+ Added task recycling for Fargate tasks, which is the process of refreshing tasks that are a part of an Amazon ECS service\. For more information, see [Fargate task recycling](task-recycle.md)\.
+ Beginning on March 27, 2019, any new Fargate task that is launched can use additional task definition parameters that enable you to define a proxy configuration, dependencies for container startup and shutdown as well as a per\-container start and stop timeout value\. For more information, see [Proxy configuration](task_definition_parameters.md#proxyConfiguration), [Container Dependency](task_definition_parameters.md#container_definition_dependson), and [Container Timeouts](task_definition_parameters.md#container_definition_timeout)\.
+ Beginning on April 2, 2019, any new Fargate task that is launched supports injecting sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.
+ Beginning on May 1, 2019, any new Fargate task that is launched supports referencing sensitive data in the log configuration of a container using the `secretOptions` container definition parameter\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.
+ Beginning on May 1, 2019, any new Fargate task that is launched supports the `splunk` log driver in addition to the `awslogs` log driver\. For more information, see [Storage and Logging](task_definition_parameters.md#container_definition_storage)\.
+ Beginning on July 9, 2019, any new Fargate tasks that is launched supports CloudWatch Container Insights\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Beginning on December 3, 2019, the Fargate Spot capacity provider is supported\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.

Fargate Platform Version‐1\.2\.0  
+ Added support for private registry authentication using AWS Secrets Manager\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

Fargate Platform Version‐1\.1\.0  
+ Added support for the Amazon ECS task metadata endpoint\. For more information, see [Amazon ECS Task metadata endpoint](task-metadata-endpoint.md)\.
+ Added support for Docker health checks in container definitions\. For more information, see [Health Check](task_definition_parameters.md#container_definition_healthcheck)\.
+ Added support for Amazon ECS service discovery\. For more information, see [Service Discovery](service-discovery.md)\.

Fargate Platform Version‐1\.0\.0  
+ Based on Amazon Linux 2017\.09\.
+ Initial release\.

## Migrating to platform version 1\.4\.0<a name="platform-version-migration"></a>

The following should be considered when migrating your Amazon ECS on Fargate tasks from platform version `1.0.0`, `1.1.0`, `1.2.0`, or `1.3.0` to platform version `1.4.0`\. It is considered best practice to confirm your task works properly on platform version `1.4.0` prior to migrating your tasks\.
+ The network traffic behavior to and from tasks has been updated\. Starting with platform version 1\.4\.0, all Amazon ECS on Fargate tasks receive a single elastic network interface \(referred to as the task ENI\) and all network traffic flows through that ENI within your VPC and will be visible to you through your VPC flow logs\. For more information, see [Fargate Task Networking](https://docs.aws.amazon.com/AmazonECS/latest/userguide/fargate-task-networking.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.
+ If you are using interface VPC endpoints, the following should be considered\.
  + When using container images hosted with Amazon ECR, both the **com\.amazonaws\.region\.ecr\.dkr** and **com\.amazonaws\.region\.ecr\.api** Amazon ECR VPC endpoints as well as the Amazon S3 gateway endpoint are required\. For more information, see [Amazon ECR interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html) in the *Amazon Elastic Container Registry User Guide*\.
  + When using a task definition that references Secrets Manager secrets to retrieve sensitive data for your containers, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
  + When using a task definition that references Systems Manager Parameter Store parameters to retrieve sensitive data for your containers, you must create the interface VPC endpoints for Systems Manager\. For more information, see [Using Systems Manager with VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html) in the *AWS Systems Manager User Guide*\.
  + Ensure that the security group in the Elastic Network Interface \(ENI\) associated with your task has the security group rules created to allow traffic between the task and the VPC endpoints you are using\.