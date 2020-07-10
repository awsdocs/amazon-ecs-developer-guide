# Scheduling Amazon ECS tasks<a name="scheduling_tasks"></a>

Amazon Elastic Container Service \(Amazon ECS\) is a shared state, optimistic concurrency system that provides flexible scheduling capabilities for your tasks and containers\. The Amazon ECS schedulers leverage the same cluster state information provided by the Amazon ECS API to make appropriate placement decisions\.

Each task that uses the Fargate launch type has its own isolation boundary and does not share the underlying kernel, CPU resources, memory resources, or elastic network interface with another task\.

Amazon ECS provides a service scheduler \(for long\-running tasks and applications\), the ability to run tasks manually \(for batch jobs or single run tasks\), with Amazon ECS placing tasks on your cluster for you\. You can specify task placement strategies and constraints that allow you to run tasks in the configuration you choose, such as spread out across Availability Zones\. It is also possible to integrate with custom or third\-party schedulers\.

**Service Scheduler**

The service scheduler is ideally suited for long running stateless services and applications\. The service scheduler ensures that the scheduling strategy you specify is followed and reschedules tasks when a task fails \(for example, if the underlying infrastructure fails for some reason\)\.

There are two service scheduler strategies available:
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](ecs_services.md#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. The service scheduler evaluates the task placement constraints for running tasks and will stop tasks that do not meet the placement constraints\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](ecs_services.md#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.

The service scheduler optionally also makes sure that tasks are registered against an Elastic Load Balancing load balancer\. You can update your services that are maintained by the service scheduler, such as deploying a new task definition, or changing the running number of desired tasks\. By default, the service scheduler spreads tasks across Availability Zones, but you can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Amazon ECS services](ecs_services.md)\.

**Manually Running Tasks**

The `RunTask` action is ideally suited for processes such as batch jobs that perform work and then stop\. For example, you could have a process call `RunTask` when work comes into a queue\. The task pulls work from the queue, performs the work, and then exits\. Using `RunTask`, you can allow the default task placement strategy to distribute tasks randomly across your cluster, which minimizes the chances that a single instance gets a disproportionate number of tasks\. Alternatively, you can use `RunTask` to customize how the scheduler places tasks using task placement strategies and constraints\. For more information, see [Running tasks](ecs_run_task.md) and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) in the *Amazon Elastic Container Service API Reference*\.

**Running Tasks on a `cron`\-like Schedule**

If you have tasks to run at set intervals in your cluster, such as a backup operation or a log scan, you can use the Amazon ECS console to create a CloudWatch Events rule that runs one or more tasks in your cluster at specified times\. Your scheduled event rule can be set to either a specific interval \(run every *N* minutes, hours, or days\), or for more complicated scheduling, you can use a `cron` expression\. For more information, see [Scheduled tasks \(`cron`\)](scheduled_tasks.md)\.

**Custom Schedulers**

Amazon ECS allows you to create your own schedulers that meet the needs of your business, or to leverage third party schedulers\. [Blox](https://blox.github.io/) is an open\-source project that gives you more control over how your containerized applications run on Amazon ECS\. It enables you to build schedulers and integrate third\-party schedulers with Amazon ECS while leveraging Amazon ECS to fully manage and scale your clusters\. Custom schedulers use the [StartTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_StartTask.html) API operation to place tasks on specific container instances within your cluster\. 

**Note**  
Custom schedulers are only compatible with tasks using the EC2 launch type\. If you are using the Fargate launch type for your tasks, the StartTask API does not work\.

**Task placement**

The `RunTask` and `CreateService` actions enable you to specify task placement constraints and task placement strategies to customize how Amazon ECS places your tasks\. For more information, see [Amazon ECS task placement](task-placement.md)\.

**Topics**
+ [Running tasks](ecs_run_task.md)
+ [Amazon ECS task placement](task-placement.md)
+ [Scheduled tasks \(`cron`\)](scheduled_tasks.md)
+ [Task lifecycle](task-lifecycle.md)
+ [Task retirement](task-retirement.md)
+ [Fargate task recycling](task-recycle.md)
+ [Creating a scheduled task using the AWS CLI](scheduled_tasks_cli_tutorial.md)