# Linux instances<a name="ecs-linux"></a>

An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster\. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances\.

**Note**  
Tasks using the Fargate launch type are deployed onto infrastructure managed by AWS, so this topic does not apply\.

The following Linux container instance operating systems are available:
+ Amazon Linux: This is a general purpose operating system\.
+ Bottlerocket: This is an operating system that is optimized for container workloads and that has a focus on security\. It does not include a package manager and is immutable by default\. For information about the security features and guidance, see [Security Features](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_FEATURES.md) and [Security Guidance](https://github.com/bottlerocket-os/bottlerocket/blob/develop/SECURITY_GUIDANCE.md) on the GitHub website\.

**Topics**
+ [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)
+ [Using Bottlerocket with Amazon ECS](ecs-bottlerocket.md)
+ [Launching an Amazon ECS Linux container instance](launch_container_instance.md)
+ [Bootstrapping container instances with Amazon EC2 user data](bootstrap_container_instance.md)
+ [Starting a task at container instance launch time](start_task_at_launch.md)
+ [Elastic network interface trunking](container-instance-eni.md)
+ [Container Instance Memory Management](memory-management.md)
+ [Connect to your container instance](instance-connect.md)
+ [Manage container instances remotely using AWS Systems Manager](ec2-run-command.md)