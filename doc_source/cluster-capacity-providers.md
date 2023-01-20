# Amazon ECS capacity providers<a name="cluster-capacity-providers"></a>

Amazon ECS capacity providers manage the scaling of infrastructure for tasks in your clusters\. Each cluster can have one or more capacity providers and an optional capacity provider strategy\. The capacity provider strategy determines how the tasks are spread across the cluster's capacity providers\. When you run a standalone task or create a service, you either use the cluster's default capacity provider strategy or a capacity provider strategy that overrides the default one\.

Capacity providers are an alternative to launch types\. Whether it's better to use a capacity provider strategy or launch types depends on whether your tasks run on Fargate or on Amazon EC2 instances\. For more information, see [Capacity provider concepts](#capacity-providers-concepts)\.

When you use external container instances on Amazon ECS Anywhere, you can't use capacity providers to manage them\.

## Capacity provider concepts<a name="capacity-providers-concepts"></a>

Capacity providers consist of the following components\.

Capacity provider  
A *capacity provider* defines the cluster capacity that Amazon ECS scales up and down of the infrastructure you specify\. Before a capacity provider can be used, it must be associated with a cluster\.  
You use a capacity provider in a capacity provider strategy to determine the infrastructure that a task runs on\. Every task must have a capacity provider strategy, a launch type, or use the default capacity provider strategy that's associated with the selected cluster\. A capacity provider can't be used by itself, it must be referenced by a capacity provider strategy\. If a task uses a launch type, the capacity it uses isn't counted by any capacity providers in the cluster\.  
For Amazon ECS on AWS Fargate users, there's a `FARGATE` and a `FARGATE_SPOT` capacity provider\. The AWS Fargate capacity providers are reserved and can't be created or deleted\. After you associate them with your cluster, you can add them to a capacity provider strategy\. The capacity provider strategy determines the fixed count and percentage of tasks that run on the two reserved capacity providers\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.  
For Amazon ECS on Amazon EC2 users, a capacity provider consists of a capacity provider name, an Auto Scaling group\. A capacity provider also consists of all of the settings for managed scaling and managed termination protection\. When you turn on managed scaling, Amazon ECS scales Auto Scaling groups in and out on your behalf\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

Default capacity provider strategy  
You can associate a *default capacity provider strategy* with an Amazon ECS cluster\. After you do this, a default capacity provider strategy is used when creating a service or running a standalone task in the cluster and whenever a custom capacity provider strategy or a launch type isn't specified\. We recommend that you define a default capacity provider strategy for each cluster\.

Capacity provider strategy  
A capacity provider strategy consists of one or more capacity providers\. You can specify an optional *base* and *weight* value for finer control\. A capacity provider strategy is part of the configuration of a cluster, service, or task\. However, you can't create re\-useable capacity provider strategies\. The capacity provider strategy of each cluster, service, or task capacity provider strategy is independent\.  
If the default capacity provider strategy for a cluster doesn't meet your capacity requirements, specify a custom *capacity provider strategy* when creating a service or running a standalone task\.  
Suppose that you set a launch type instead of a capacity provider strategy on tasks in clusters where the capacity is managed by capacity providers\. Then, in this case, the launch type overrides\. That is, those tasks aren't counted for capacity provider scaling actions\.
Only capacity providers that are both already associated with a cluster and have an `ACTIVE` or `UPDATING` status can be used in a capacity provider strategy\. You can associate a capacity provider with a cluster when you create a cluster\. You can also associate a capacity provider with an existing cluster using the PutClusterCapacityProviders API operation\.  
In a capacity provider strategy, the optional *base* value designates how many tasks, at a minimum, run on a specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\.  
The *weight* value determines the relative percentage of the total number of launched tasks that use the specified capacity provider\. Consider the following example\. You have a strategy that contains two capacity providers, and both have a weight of `1`\. When the base percentage is reached, the tasks are split evenly across the two capacity providers\. Using that same logic, suppose that you specify a weight of `1` for *capacityProviderA* and a weight of `4` for *capacityProviderB*\. Then, for every one task that's run using *capacityProviderA*, there are four tasks that use *capacityProviderB*\.

## Capacity provider types<a name="capacity-providers-types"></a>

The infrastructure that your Amazon ECS workloads are run on determines the type of capacity provider that you can use\.

For Amazon ECS workloads that are hosted on Fargate, the following predefined capacity providers are available:
+ Fargate
+ Fargate Spot

For Amazon ECS workloads that are hosted on Amazon EC2 instances, you must create and maintain a capacity provider that consists of the following components:
+ A name
+ An Auto Scaling group
+ The settings for managed scaling and managed termination protection\.

## Capacity provider considerations<a name="capacity-providers-considerations"></a>

Consider the following when using capacity providers:
+ A capacity provider must be associated with a cluster before being specified in a capacity provider strategy\.
+ When you specify a capacity provider strategy, the number of capacity providers that you can specify is limited to six\.
+ You can't update a service using an Auto Scaling group capacity provider to use a Fargate capacity provider\. The opposite is also the case\.
+ In a capacity provider strategy, if no `weight` value is specified for a capacity provider in the console, then the default value of `1` is used\. If using the API or AWS CLI, the default value of `0` is used\.
+ When multiple capacity providers are specified within a capacity provider strategy, at least one of the capacity providers must have a weight value that's greater than zero\. Moreover, any capacity providers with a weight of zero aren't used to place tasks\. If you specify multiple capacity providers in a strategy with all the same weight of zero, then any `RunTask` or `CreateService` actions using the capacity provider strategy fail\.
+ In a capacity provider strategy, only one capacity provider can have a defined *base* value\. If no base value is specified, the default value of zero is used\.
+ A cluster can contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers\. However, a capacity provider strategy can only contain Auto Scaling group or Fargate capacity providers, but not both\.
+ A cluster can contain a mix of services and standalone tasks that use both capacity providers and launch types\. A service can be updated to use a capacity provider strategy rather than a launch type\. However, you must force a new deployment when doing so\.
+ When you use managed termination protection, you must also use managed scaling\. Otherwise, managed termination protection doesn't work\.

The following sections provide information about the Fargate launch type and EC2 launch type capacity providers\.

**Topics**
+ [Capacity provider concepts](#capacity-providers-concepts)
+ [Capacity provider types](#capacity-providers-types)
+ [Capacity provider considerations](#capacity-providers-considerations)
+ [AWS Fargate capacity providers](fargate-capacity-providers.md)
+ [Auto Scaling group capacity providers](asg-capacity-providers.md)