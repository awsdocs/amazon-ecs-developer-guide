# Windows instances<a name="ecs-windows"></a>

An Amazon ECS container instance is an Amazon EC2 instance that is running the Amazon ECS container agent and has been registered into an Amazon ECS cluster\. When you run tasks with Amazon ECS using the EC2 launch type or an Auto Scaling group capacity provider, your tasks are placed on your active container instances\.

**Note**  
Tasks using the Fargate launch type are deployed onto infrastructure managed by AWS, so this topic does not apply\.

**Topics**
+ [Amazon ECS\-optimized AMI](ecs-optimized_windows_AMI.md)
+ [Launching an Amazon ECS Windows container instance](launch_window-container_instance.md)
+ [Bootstrapping Windows container instances with Amazon EC2 user data](bootstrap_windows_container_instance.md)
+ [Connect to your container Windows instance](instance-windows-connect.md)
+ [Deregister an Amazon EC2 backed container instance](deregister_container_instance.md)