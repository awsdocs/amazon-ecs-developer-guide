# Amazon ECS Task Placement<a name="task-placement"></a>

When you launch a task that uses the EC2 launch type, Amazon ECS must determine where to place the task based on the requirements specified in the task definition, such as CPU and memory\. Similarly, when you scale down the task count, Amazon ECS must determine which tasks to terminate\. You can apply task placement strategies and constraints to customize how Amazon ECS places and terminates tasks\.

A *task placement strategy* is an algorithm for selecting instances for task placement or tasks for termination\. For example, Amazon ECS can select instances at random or it can select instances such that tasks are distributed evenly across a group of instances\. A *task placement constraint* is a rule that is considered during task placement\. For example, you can use constraints to place tasks based on Availability Zone or instance type\. You can associate *attributes*, which are name/value pairs, with your container instances and then use a constraint to place tasks based on attribute\.

**Note**  
Task placement strategies are best effort\. Amazon ECS still attempts to place tasks even when the most optimal placement option is unavailable\. However, task placement constraints are binding, and they can prevent task placement\. 

You can use strategies and constraints together\. For example, you can distribute tasks across Availability Zones and bin pack tasks based on memory within each Availability Zone, but only for G2 instances\.

When Amazon ECS places tasks, it uses the following process to select container instances:

1. Identify the instances that satisfy the CPU, memory, and port requirements in the task definition\.

1. Identify the instances that satisfy the task placement constraints\.

1. Identify the instances that satisfy the task placement strategies\.

1. Select the instances for task placement\.

**Note**  
Task placement contraints are not supported for Fargate tasks\. By default Fargate tasks are spread across availability zones\.

**Topics**
+ [Amazon ECS Task Placement Strategies](task-placement-strategies.md)
+ [Amazon ECS Task Placement Constraints](task-placement-constraints.md)
+ [Cluster Query Language](cluster-query-language.md)