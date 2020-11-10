# Windows containers<a name="ECS_Windows"></a>

Amazon ECS now supports Windows containers on container instances that are launched with the Amazon ECS\-optimized Windows Server AMI\. 

Windows container instances use their own version of the Amazon ECS container agent\. On the Amazon ECS\-optimized Windows Server AMI, the Amazon ECS container agent runs as a service on the host\. Unlike the Linux platform, the agent doesn't run inside a container because it uses the host's registry and the named pipe at `\\.\pipe\docker_engine` to communicate with the Docker daemon\.

The source code for the Amazon ECS container agent is [available on GitHub](https://github.com/aws/amazon-ecs-agent)\. We encourage you to submit pull requests for changes that you would like to have included\. However, we do not currently provide support for running modified copies of this software\. You can view open issues for Amazon ECS and Windows on our [GitHub issues page](https://github.com/aws/amazon-ecs-agent/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aopen%20label%3Aos/windows)\.

Amazon ECS vends AMIs that are optimized for Windows containers in the following variants\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.
+ **Amazon ECS\-optimized Windows Server 2019 Full AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 2019 Core AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 1909 Core AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\.
+ **Amazon ECS\-optimized Windows Server 2016 Full AMI** – Available for launching your Amazon ECS container instances on the Windows operating system\.

Windows Server 2019 and Windows Server 2016 are Long\-Term Servicing Channel \(LTSC\) releases\. Windows Server 2004 and Windows Servier 1909 are Semi\-Annual Channel \(SAC\) releases\. For more information, see [Windows Server release information](https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info)\.

**Topics**
+ [Windows container caveats](#windows_caveats)
+ [Getting started with Windows containers](#windows-getting-started)
+ [Windows task definitions](windows_task_definitions.md)
+ [Windows IAM roles for tasks](windows_task_IAM_roles.md)
+ [Pushing Windows images to Amazon ECR](windows_ecr.md)
+ [Using gMSAs for Windows Containers](windows-gmsa.md)

## Windows container caveats<a name="windows_caveats"></a>

Here are some things you should know about Windows containers and Amazon ECS\.
+ Windows containers can't run on Linux container instances, and the opposite is also the case\. For better task placement for Windows and Linux tasks, keep Windows and Linux container instances in separate clusters and only place Windows tasks on Windows clusters\. You can ensure that Windows task definitions are only placed on Windows instances by setting the following placement constraint: `memberOf(ecs.os-type=='windows')`\.
+ Windows containers are only supported for tasks that use the EC2 launch type\. The Fargate launch type isn't currently supported for Windows containers\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.
+ Windows containers and container instances can't support all the task definition parameters that are available for Linux containers and container instances\. For some parameters, they aren't supported at all, and others behave differently on Windows than they do on Linux\. For more information, see [Windows task definitions](windows_task_definitions.md)\.
+ For the IAM roles for tasks feature, you need to configure your Windows container instances to allow the feature at launch\. Your containers must run some provided PowerShell code when they use the feature\. For more information, see [Windows IAM roles for tasks](windows_task_IAM_roles.md)\.
+ The IAM roles for tasks feature uses a credential proxy to provide credentials to the containers\. This credential proxy occupies port 80 on the container instance, so if you use IAM roles for tasks, port 80 is not available for tasks\. For web service containers, you can use an Application Load Balancer and dynamic port mapping to provide standard HTTP port 80 connections to your containers\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ The Windows server Docker images are large \(9 GiB\)\. As such, your Windows container instances require more storage space than Linux container instances\.

## Getting started with Windows containers<a name="windows-getting-started"></a>

Work through a tutorial that guides you through getting Windows containers running on Amazon ECS with the Amazon ECS\-optimized Windows Server AMI in the AWS Management Console at [Getting started with Windows containers](ECS_Windows_getting_started.md)\.