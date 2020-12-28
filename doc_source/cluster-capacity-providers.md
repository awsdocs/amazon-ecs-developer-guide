# Amazon ECS capacity providers<a name="cluster-capacity-providers"></a>

Amazon ECS capacity providers are used to manage the infrastructure the tasks in your clusters use\. Each cluster can have one or more capacity providers and an optional default capacity provider strategy\. The capacity provider strategy determines how the tasks are spread across the cluster's capacity providers\. When you run a standalone task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\.

## Capacity provider concepts<a name="capacity-providers-concepts"></a>

Capacity providers consist of the following components\.

Capacity provider  
A *capacity provider* is associated with a cluster and is used in a capacity provider strategy to determine the infrastructure that a task runs on\.  
For Amazon ECS on AWS Fargate users, there is a `FARGATE` and a `FARGATE_SPOT` capacity provider\. The AWS Fargate capacity providers are reserved and don't need to be created nor can they be deleted\. After you associate them with your cluster, you may add them to a capacity provider strategy\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.  
For Amazon ECS on Amazon EC2 users, a capacity provider consists of a capacity provider name, an Auto Scaling group, and the settings for managed scaling and managed termination protection\. With managed scaling, Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group which provides auto scaling for your cluster's infrastructure\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

Capacity provider strategy  
A *capacity provider strategy* gives you control over how your tasks use one or more capacity providers\.  
When creating a service or running a standalone task, you specify a capacity provider strategy\. A capacity provider strategy consists of one or more capacity providers with an optional *base* and *weight* specified for each provider\.  
The *base* value designates how many tasks, at a minimum, to run on the specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\.  
The *weight* value designates the relative percentage of the total number of launched tasks that should use the specified capacity provider\. For example, if you have a strategy that contains two capacity providers, and both have a weight of `1`, then when the base is satisfied, the tasks will be split evenly across the two capacity providers\. Using that same logic, if you specify a weight of `1` for *capacityProviderA* and a weight of `4` for *capacityProviderB*, then for every one task that is run using *capacityProviderA*, four tasks would use *capacityProviderB*\.

Default capacity provider strategy  
A *default capacity provider strategy* is associated with each Amazon ECS cluster\. This determines the capacity provider strategy the cluster uses when there isn't a custom capacity provider strategy or launch type specified when creating a service or running a standalone task\.

## Capacity provider considerations<a name="capacity-providers-considerations"></a>

The following should be considered when using capacity providers:
+ A capacity provider must be associated with a cluster prior to being specified in a capacity provider strategy\.
+ When you specify a capacity provider strategy, the number of capacity providers that can be specified is limited to six\.
+ A cluster may contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers, however a capacity provider strategy may only contain one or the other but not both\.
+ A cluster may contain a mix of services and standalone tasks using both capacity providers and launch types\. A service may be updated to use a capacity provider strategy rather than a launch type, however you must force a new deployment when doing so\.
+ When managed termination protection is enabled, managed scaling must also be enabled otherwise managed termination protection won't work\.
+ Using capacity providers is not supported when using the blue/green deployment type for your services\.
+ Using capacity providers is not supported when using Classic Load Balancers for your services\.