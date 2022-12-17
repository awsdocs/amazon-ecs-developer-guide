# Scheduling Amazon ECS tasks<a name="scheduling_tasks"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a shared state, optimistic concurrency system that provides flexible scheduling capabilities for your tasks and containers\. The Amazon ECS schedulers use the same cluster state information as the Amazon ECS API to make appropriate placement decisions\.

Each task that uses the Fargate launch type has its own isolation boundary and doesn't share underlying resources with any other tasks\. These resources include the underlying kernel, CPU resources, memory resources, and elastic network interface\.

Amazon ECS provides a service scheduler for long\-running tasks and applications\. It also provides the ability to run tasks manually for batch jobs or single run tasks\. Amazon ECS provides one whenever it places tasks on your cluster\. You can specify the task placement strategies and constraints for running tasks that best meet your needs\. For example, you can specify whether tasks run across multiple Availability Zones or within a single Availability Zone\. And, optionally, you can integrate tasks with your own custom or third\-party schedulers\.

**Service scheduler**

The service scheduler is suitable for long running stateless services and applications\. The service scheduler ensures that the scheduling strategy that you specify is followed and reschedules tasks when a task fails\. For example, if the underlying infrastructure fails, the service scheduler can reschedule tasks\.

There are two service scheduler strategies available:
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](ecs_services.md#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. The service scheduler evaluates the task placement constraints for running tasks and will stop tasks that do not meet the placement constraints\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](ecs_services.md#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.

The service scheduler optionally also makes sure that tasks are registered against an Elastic Load Balancing load balancer\. You can update your services that are maintained by the service scheduler\. This might include deploying a new task definition or changing the number of desired tasks that are running\. By default, the service scheduler spreads tasks across multiple Availability Zones\. However, you can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Amazon ECS services](ecs_services.md)\.

**Manually running tasks**

The `RunTask` action is suitable for processes such as batch jobs that perform work and then stop\. For example, you can have a process call `RunTask` when work comes into a queue\. The task pulls work from the queue, performs the work, and then exits\. Using `RunTask`, you can allow the default task placement strategy to distribute tasks randomly across your cluster\. This minimizes the chances that a single instance gets a disproportionate number of tasks\. Alternatively, you can use `RunTask` to customize how the scheduler places tasks using task placement strategies and constraints\. For more information, see [Run a standalone task in the classic Amazon ECS console](ecs_run_task.md) and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) in the *Amazon Elastic Container Service API Reference*\.

**Running tasks on a `cron`\-like schedule**

If you have tasks to run at set intervals in your cluster, you can use the Amazon ECS console to create an EventBridge event\. You can run tasks for a backup operation or a log scan\. The EventBridge event that you create can run one or more tasks in your cluster at specified times\. Your scheduled event can be set to a specific interval \(run every *N* minutes, hours, or days\)\. Otherwise, for more complicated scheduling, you can use a `cron` expression\. For more information, see [Scheduled tasks](scheduled_tasks.md)\.

**Custom schedulers**

With Amazon ECS, you can create your own schedulers or use third\-party schedulers\. [Blox](https://blox.github.io/) is an open\-source project that gives you more control over how your containerized applications run on Amazon ECS\. You can use it to build schedulers and integrate third\-party schedulers with Amazon ECS\. At the same time, you can also use Amazon ECS to manage and scale your clusters\. Custom schedulers use the [StartTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_StartTask.html) API operation to place tasks on specific container instances within your cluster\. 

**Note**  
Custom schedulers are only compatible with tasks hosted on EC2 instances\. If you use Amazon ECS on Fargate, the StartTask API doesn't work\.

**Task placement**

Use `RunTask` and `CreateService` actions to specify task placement constraints and task placement strategies\. These customize how Amazon ECS places and runs your tasks\. For more information, see [Amazon ECS task placement](task-placement.md)\.

**Topics**
+ [Running a standalone task using the new Amazon ECS console](ecs_run_task-v2.md)
+ [Stopping tasks using the new console](stop-task-console-v2.md)
+ [Run a standalone task in the classic Amazon ECS console](ecs_run_task.md)
+ [Amazon ECS task placement](task-placement.md)
+ [Scheduled tasks](scheduled_tasks.md)
+ [Task lifecycle](task-lifecycle.md)
+ [Creating a scheduled task using the AWS CLI](scheduled_tasks_cli_tutorial.md)