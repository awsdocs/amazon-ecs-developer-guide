# Task groups<a name="task-groups"></a>

You can identify a set of related tasks as a *task group*\. All tasks with the same task group name are considered as a set when using the `spread` task placement strategy\. For example, suppose that you're running different applications in one cluster, such as databases and web servers\. To ensure that your databases are balanced across Availability Zones, add them to a task group named `databases` and then use the `spread` task placement strategy\. For more information, see [Amazon ECS task placement strategies](task-placement-strategies.md)\.

Task groups can also be used as a task placement constraint\. When you specify a task group in the `memberOf` constraint, task are only sent to container instances that are in the specified task group\. For an example, see [Example constraints](task-placement-constraints.md#constraint-examples)\.

By default, standalone tasks use the task definition family name \(for example, `family:my-task-definition`\) as the task group name if a custom task group name isn't specified\. Tasks launched as part of a service use the service name as the task group name and can't be changed\.

The following requirements for the task group name must be considered\.
+ A task group name must be 255 or fewer characters\.
+ Each task can be in exactly one group\.
+ After launching a task, you can't modify its task group\.