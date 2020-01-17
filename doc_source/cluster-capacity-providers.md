# Amazon ECS Cluster Capacity Providers<a name="cluster-capacity-providers"></a>

Amazon ECS cluster capacity providers determine the infrastructure to use for your tasks\. Each cluster has one or more capacity providers and an optional default capacity provider strategy\. The capacity provider strategy determines how the tasks are spread across the capacity providers\. When you run a task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\.

## Cluster Capacity Provider Concepts<a name="capacity-providers-concepts"></a>

Cluster capacity providers consist of the following components\.

Capacity provider  
A *capacity provider* is used in association with a cluster to determine the infrastructure that a task runs on\.  
For Amazon ECS on AWS Fargate users, the `FARGATE` and `FARGATE_SPOT` capacity providers are provided automatically\. For more information, see [Using AWS Fargate Capacity Providers](fargate-capacity-providers.md)\.  
For Amazon ECS on Amazon EC2 users, a capacity provider consists of a name, an Auto Scaling group, and the settings for managed scaling and managed termination protection\. This type of capacity provider is used in cluster auto scaling\. For more information, see [Auto Scaling Group Capacity Providers](cluster-auto-scaling.md#asg-capacity-providers)\.  
One or more capacity providers are specified in a capacity provider strategy, which is then associated with a cluster\.

Capacity provider strategy  
A *capacity provider strategy* gives you control over how your tasks use one or more capacity providers\.  
When you run a task or create a service, you specify a capacity provider strategy\. A capacity provider strategy consists of one or more capacity providers with an optional *base* and *weight* specified for each provider\.  
The *base* value designates how many tasks, at a minimum, to run on the specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\.  
The *weight* value designates the relative percentage of the total number of launched tasks that should use the specified capacity provider\. For example, if you have a strategy that contains two capacity providers, and both have a weight of `1`, then when the base is satisfied, the tasks will be split evenly across the two capacity providers\. Using that same logic, if you specify a weight of `1` for *capacityProviderA* and a weight of `4` for *capacityProviderB*, then for every one task that is run using *capacityProviderA*, four tasks would use *capacityProviderB*\.

Default capacity provider strategy  
A *default capacity provider strategy* is associated with each Amazon ECS cluster\. This determines the capacity provider strategy the cluster will use if no other capacity provider strategy or launch type is specified when running a task or creating a service\.

## Cluster Capacity Provider Considerations<a name="capacity-providers-considerations"></a>

The following should be considered when using cluster capacity providers:
+ When you specify a capacity provider strategy, the number of capacity providers that can be specified is limited to six\.
+ A cluster may contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers, however when specifying a capacity provider strategy they may only contain one or the other but not both\.
+ A cluster may contain a mix of tasks and services using both capacity providers and launch types\. A service may also be updated to use a capacity provider strategy rather than a launch type, however you must force a new deployment when doing so\.
+ When you specify a capacity provider strategy, the `base` value is only supported when running tasks\. When creating a service, the capacity provider strategy `base` parameter is not supported\.
+ When using managed termination protection, managed scaling must also be used otherwise managed termination protection will not work\.
+ Using cluster capacity providers is not supported when using the blue/green deployment type for your services\.