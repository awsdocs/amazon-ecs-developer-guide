# Amazon ECS task placement<a name="task-placement"></a>

When a task that uses the EC2 launch type is launched, Amazon ECS must determine where to place the task based on the requirements specified in the task definition, such as CPU and memory\. Similarly, when you scale down the task count, Amazon ECS must determine which tasks to terminate\. You can apply task placement strategies and constraints to customize how Amazon ECS places and terminates tasks\. Task placement strategies and constraints are not supported for tasks using the Fargate launch type\. By default, Fargate tasks are spread across Availability Zones\. With all other tasks, default task placement strategies depend on whether you are running tasks manually or within a service\. For more information, see [Scheduling Amazon ECS tasks](scheduling_tasks.md)\.

A *task placement strategy* is an algorithm for selecting instances for task placement or tasks for termination\. For example, Amazon ECS can select instances at random, or it can select instances such that tasks are distributed evenly across a group of instances\.

A *task placement constraint* is a rule that is considered during task placement\. For example, you can use constraints to place tasks based on Availability Zone or instance type\. You can also associate *attributes*, which are name/value pairs, with your container instances and then use a constraint to place tasks based on attribute\.

**Note**  
Task placement strategies are a best effort\. Amazon ECS still attempts to place tasks even when the most optimal placement option is unavailable\. However, task placement constraints are binding, and they can prevent task placement\. 

You can use task placement strategies and constraints together\. For example, you can use a task placement strategy and a task placement constraint to distribute tasks across Availability Zones and bin pack tasks based on memory within each Availability Zone, but only for G2 instances\.

When Amazon ECS places tasks, it uses the following process to select container instances:

1. Identify the instances that satisfy the CPU, memory, and port requirements in the task definition\.

1. Identify the instances that satisfy the task placement constraints\.

1. Identify the instances that satisfy the task placement strategies\.

1. Select the instances for task placement\.

**Topics**
+ [Amazon ECS task placement strategies](task-placement-strategies.md)
+ [Amazon ECS task placement constraints](task-placement-constraints.md)
+ [Cluster query language](cluster-query-language.md)