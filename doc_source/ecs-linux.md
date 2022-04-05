# Linux instances<a name="ecs-linux"></a>

An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster\. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances\.

**Note**  
Tasks using the Fargate launch type are deployed onto infrastructure managed by AWS, so this topic does not apply\.

The following Linux container instance operating systems are available:
+ Amazon Linux: This is a general purpose operating system\.
+ Bottlerocket: This is an operating system that is optimized for container workloads and that has a focus on security\. It does not include a package manager and is immutable by default\. For information about the security features and guidance, see [Security Features](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_FEATURES.md) and [Security Guidance](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_GUIDANCE.md) on the GitHub website\.

An Amazon ECS container instance specification consists of the following components:

**Required**
+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.
+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS container agent](ECS_agent.md)\.
+ A Docker daemon running at least version 1\.9\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS container agent version that you are using\.

**Recommended**
+ An initialization and nanny process to run and monitor the Amazon ECS container agent\. The Amazon ECS\-optimized AMIs use the `ecs-init` RPM to manage the agent\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

**Topics**
+ [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)
+ [Using Bottlerocket with Amazon ECS](ecs-bottlerocket.md)
+ [Launching an Amazon ECS Linux container instance](launch_container_instance.md)
+ [Bootstrapping container instances with Amazon EC2 user data](bootstrap_container_instance.md)
+ [Starting a task at container instance launch time](start_task_at_launch.md)
+ [Elastic network interface trunking](container-instance-eni.md)
+ [Container Instance Memory Management](memory-management.md)
+ [Connect to your container instance using the classic console](instance-connect.md)
+ [Manage container instances remotely using AWS Systems Manager](ec2-run-command.md)