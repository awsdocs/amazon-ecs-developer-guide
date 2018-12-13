# Running Tasks<a name="ecs_run_task"></a>

Running tasks manually is ideal in certain situations\. For example, suppose that you are developing a task but you are not ready to deploy this task with the service scheduler\. Perhaps your task is a one\-time or periodic batch job that does not make sense to keep running or restart when it finishes\.

To keep a specified number of tasks running or to place your tasks behind a load balancer, use the Amazon ECS service scheduler instead\. For more information, see [Services](ecs_services.md)\.

**Topics**
+ [Running a Task Using the Fargate Launch Type](ecs_run_task_fargate.md)
+ [Running a Task Using the EC2 Launch Type](ecs_run_task_ec2.md)