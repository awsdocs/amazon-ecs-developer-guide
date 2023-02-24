# Amazon ECS task definitions<a name="task_definitions"></a>

A task definition is required to run Docker containers in Amazon ECS\. The following are some of the parameters that you can specify in a task definition:
+ The Docker image to use with each container in your task
+ How much CPU and memory to use with each task or each container within a task
+ The launch type to use, which determines the infrastructure that your tasks are hosted on
+ The Docker networking mode to use for the containers in your task
+ The logging configuration to use for your tasks
+ Whether the task continues to run if the container finishes or fails
+ The command that the container runs when it's started
+ Any data volumes that are used with the containers in the task
+ The IAM role that your tasks use

You can define multiple containers in a task definition\. The parameters that you use depend on the launch type that you choose for the task\. Not all parameters are valid\. For a list of available parameters and information about which launch types that they're valid for in a task definition, see [Task definition parameters](task_definition_parameters.md)\.

Your entire application stack doesn't need to be on a single task definition, and in most cases it isn't on a single task definition\. Your application can span multiple task definitions\. You can do this by combining related containers into their own task definitions, each representing a single component\. For more information, see [Application architecture](application_architecture.md)\.

After you create a task definition, you can run the task definition as a task or a service\.
+ A *task* is the instantiation of a task definition within a cluster\. After you create a task definition for your application within Amazon ECS, you can specify the number of tasks to run on your cluster\. 
+ An Amazon ECS *service* runs and maintains your desired number of tasks simultaneously in an Amazon ECS cluster\. How it works is that, if any of your tasks fail or stop for any reason, the Amazon ECS service scheduler launches another instance based on your task definition\. It does this to replace it and thereby maintain your desired number of tasks in the service\.

**Topics**
+ [Task definition states](task-definition-state.md)
+ [Amazon EC2 Windows task definition considerations](windows_task_definitions.md)
+ [Application architecture](application_architecture.md)
+ [Task definition management in the Amazon ECS console](available-task-definition-actions.md)
+ [Task definition template](task-definition-template.md)
+ [Task definition parameters](task_definition_parameters.md)
+ [Amazon ECS launch types](launch_types.md)
+ [Working with GPUs on Amazon ECS](ecs-gpu.md)
+ [Using video transcoding on Amazon ECS](ecs-vt1.md)
+ [Using machine learning on Amazon ECS](ecs-machine-learning.md)
+ [Working with 64\-bit ARM workloads on Amazon ECS](ecs-arm64.md)
+ [Using data volumes in tasks](using_data_volumes.md)
+ [Managing container swap space](container-swap.md)
+ [Amazon ECS task networking](task-networking.md)
+ [Using the awslogs log driver](using_awslogs.md)
+ [Custom log routing](using_firelens.md)
+ [Private registry authentication for tasks](private-auth.md)
+ [Passing environment variables to a container](taskdef-envfiles.md)
+ [Passing sensitive data to a container](specifying-sensitive-data.md)
+ [Example task definitions](example_task_definitions.md)
+ [Task definition management using the AWS CLI](task-def-management-cli.md)