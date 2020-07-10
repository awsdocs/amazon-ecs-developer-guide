# Windows containers<a name="ECS_Windows"></a>

Amazon ECS now supports Windows containers on container instances that are launched with the Amazon ECS\-optimized Windows Server AMI\. 

Windows container instances use their own version of the Amazon ECS container agent\. On the Amazon ECS\-optimized Windows Server AMI, the Amazon ECS container agent runs as a service on the host\. Unlike the Linux platform, the agent does not run inside a container because it uses the host's registry and the named pipe at `\\.\pipe\docker_engine` to communicate with the Docker daemon\.

The source code for the Amazon ECS container agent is [available on GitHub](https://github.com/aws/amazon-ecs-agent)\. We encourage you to submit pull requests for changes that you would like to have included\. However, we do not currently provide support for running modified copies of this software\. You can view open issues for Amazon ECS and Windows on our [GitHub issues page](https://github.com/aws/amazon-ecs-agent/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20label%3Aos/windows)\.

Amazon ECS vends AMIs that are optimized for Windows containers in the following variants\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.
+ **Amazon ECS\-optimized Windows Server 2019 Full AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 2019 Core AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 1909 Core AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 2016 Full AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\.

Windows Server 2019 and Windows Server 2016 are Long\-Term Servicing Channel \(LTSC\) releases\. Windows Server, version 1909, is a Semi\-Annual Channel \(SAC\) release\. For more information, see [Windows Server release information](https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info)\.

**Topics**
+ [Windows container caveats](#windows_caveats)
+ [Getting started with Windows containers](ECS_Windows_getting_started.md)
+ [Windows task definitions](windows_task_definitions.md)
+ [Windows IAM roles for tasks](windows_task_IAM_roles.md)
+ [Pushing Windows images to Amazon ECR](windows_ecr.md)
+ [Using gMSAs for Windows Containers](windows-gmsa.md)

## Windows container caveats<a name="windows_caveats"></a>

Here are some things you should know about Windows containers and Amazon ECS\.
+ Windows containers cannot run on Linux container instances and vice versa\. To ensure proper task placement for Windows and Linux tasks, you should keep Windows and Linux container instances in separate clusters, and only place Windows tasks on Windows clusters\. You can ensure that Windows task definitions are only placed on Windows instances by setting the following placement constraint: `memberOf(ecs.os-type=='windows')`\.
+ Windows containers are only supported for tasks that use the EC2 launch type\. The Fargate launch type is not currently supported for Windows containers\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.
+ Windows containers and container instances cannot support all the task definition parameters that are available for Linux containers and container instances\. For some parameters, they are not supported at all, and others behave differently on Windows than they do on Linux\. For more information, see [Windows task definitions](windows_task_definitions.md)\.
+ The IAM roles for tasks feature requires that you configure your Windows container instances to allow the feature at launch, and your containers must run some provided PowerShell code when they use the feature\. For more information, see [Windows IAM roles for tasks](windows_task_IAM_roles.md)\.
+ The IAM roles for tasks feature uses a credential proxy to provide credentials to the containers\. This credential proxy occupies port 80 on the container instance, so if you use IAM roles for tasks, port 80 is not available for tasks\. For web service containers, you can use an Application Load Balancer and dynamic port mapping to provide standard HTTP port 80 connections to your containers\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ The Windows server Docker images are large \(9 GiB\), so your container instances require more storage space than Linux container instances, which typically have smaller image sizes\.