# Amazon ECS Task Definitions<a name="task_definitions"></a>

A task definition is required to run Docker containers in Amazon ECS\. Some of the parameters you can specify in a task definition include:
+ The Docker images to use with the containers in your task
+ How much CPU and memory to use with each container
+ The launch type to use, which determines the infrastructure on which your tasks are hosted
+ Whether containers are linked together in a task
+ The Docker networking mode to use for the containers in your task
+ \(Optional\) The ports from the container to map to the host container instance
+ Whether the task should continue to run if the container finishes or fails
+ The command the container should run when it is started
+ \(Optional\) The environment variables that should be passed to the container when it starts
+ Any data volumes that should be used with the containers in the task
+ \(Optional\) The IAM role that your tasks should use for permissions

You can define multiple containers in a task definition\. The parameters that you use depend on the launch type you choose for the task\. Not all parameters are valid\. For more information about the parameters available and which launch types they are valid for in a task definition, see [Task Definition Parameters](task_definition_parameters.md)\.

Your entire application stack does not need to exist on a single task definition, and in most cases it should not\. Your application can span multiple task definitions by combining related containers into their own task definitions, each representing a single component\. For more information, see [Application Architecture](application_architecture.md)\.

**Topics**
+ [Application Architecture](application_architecture.md)
+ [Creating a Task Definition](create-task-definition.md)
+ [Task Definition Parameters](task_definition_parameters.md)
+ [Amazon ECS Launch Types](launch_types.md)
+ [Using Data Volumes in Tasks](using_data_volumes.md)
+ [Task Networking with the `awsvpc` Network Mode](task-networking.md)
+ [Using the awslogs Log Driver](using_awslogs.md)
+ [Private Registry Authentication for Tasks](private-auth.md)
+ [Example Task Definitions](example_task_definitions.md)
+ [Updating a Task Definition](update-task-definition.md)
+ [Deregistering Task Definitions](deregister-task-definition.md)