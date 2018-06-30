# Windows Containers<a name="ECS_Windows"></a>

Amazon ECS now supports Windows containers on container instances that are launched with the Amazon ECS\-optimized Windows AMI\. 

Windows container instances use their own version of the Amazon ECS container agent\. On the Amazon ECS\-optimized Windows AMI, the Amazon ECS container agent runs as a service on the host\. Unlike the Linux platform, the agent does not run inside a container because it uses the host's registry and the named pipe at `\\.\pipe\docker_engine` to communicate with the Docker daemon\.

The source code for the Amazon ECS container agent is [available on GitHub](https://github.com/aws/amazon-ecs-agent)\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\. You can view open issues for Amazon ECS and Windows on our [GitHub issues page](https://github.com/aws/amazon-ecs-agent/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20label%3Aos/windows)\.

**Topics**
+ [Windows Container Caveats](#windows_caveats)
+ [Getting Started with Windows Containers](ECS_Windows_getting_started.md)
+ [Windows Task Definitions](windows_task_definitions.md)
+ [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)
+ [Pushing Windows Images to Amazon ECR](windows_ecr.md)

## Windows Container Caveats<a name="windows_caveats"></a>

Here are some things you should know about Windows containers and Amazon ECS\.
+ Windows containers cannot run on Linux container instances and vice versa\. To ensure proper task placement for Windows and Linux tasks, you should keep Windows and Linux container instances in separate clusters, and only place Windows tasks on Windows clusters\. You can ensure that Windows task definitions are only placed on Windows instances by setting the following placement constraint: `memberOf(ecs.os-type=='windows')`\.
+ Windows containers are only supported for tasks that use the EC2 launch type\. The Fargate launch type is not currently supported for Windows containers\. For more information about launch types, see [Amazon ECS Launch Types](launch_types.md)\.
+ Windows containers and container instances cannot support all the task definition parameters that are available for Linux containers and container instances\. For some parameters, they are not supported at all, and others behave differently on Windows than they do on Linux\. For more information, see [Windows Task Definitions](windows_task_definitions.md)\.
+ The IAM roles for tasks feature requires that you configure your Windows container instances to allow the feature at launch, and your containers must run some provided PowerShell code when they use the feature\. For more information, see [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)\.
+ The IAM roles for tasks feature uses a credential proxy to provide credentials to the containers\. This credential proxy occupies port 80 on the container instance, so if you use IAM roles for tasks, port 80 is not available for tasks\. For web service containers, you can use an Application Load Balancer and dynamic port mapping to provide standard HTTP port 80 connections to your containers\. For more information, see [Service Load Balancing](service-load-balancing.md)\.
+ The Docker on Windows container instance use default NAT network mode for containers which allocate Dynamic hostPort any available port above 1024. The Windows OS higher than 2008 using the Dynamic port range 49153 to 65535 similar to Linux platforms but still Docker on windows choose Dynamic port anything available above 1024.
+ The Windows server Docker images are large \(9 GiB\), so your container instances require more storage space than Linux container instances, which typically have smaller image sizes\.
