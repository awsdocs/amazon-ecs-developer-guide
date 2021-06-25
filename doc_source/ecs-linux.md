# Linux instances<a name="ecs-linux"></a>

An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster\. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances\.

**Note**  
Tasks using the Fargate launch type are deployed onto infrastructure managed by AWS, so this topic does not apply\.

**Topics**
+ [Amazon ECS\-optimized AMI](ecs-optimized_AMI.md)
+ [Launching an Amazon ECS Linux container instance](launch_container_instance.md)
+ [Bootstrapping container instances with Amazon EC2 user data](bootstrap_container_instance.md)
+ [Starting a task at container instance launch time](start_task_at_launch.md)
+ [Elastic network interface trunking](container-instance-eni.md)
+ [Container Instance Memory Management](memory-management.md)
+ [Connect to your container instance](instance-connect.md)
+ [Manage container instances remotely using AWS Systems Manager](ec2-run-command.md)