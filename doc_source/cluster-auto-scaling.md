# Amazon ECS cluster Auto Scaling<a name="cluster-auto-scaling"></a>

**Important**  
As of May 27, 2022, Amazon ECS changed how the resources that facilitate cluster Auto Scaling is managed\. To learn more, see [Update on the way Amazon ECS creates resources for cluster auto scaling](#update-ecs-resources-cas)\.

Amazon ECS can manage the scaling of Amazon EC2 instances that are registered to your cluster\. This is referred to as Amazon ECS cluster auto scaling\. This is done by using an Amazon ECS Auto Scaling group capacity provider with managed scaling turned on\. When you use an Auto Scaling group capacity provider with managed scaling turned on, Amazon ECS creates two custom CloudWatch metrics and a target tracking scaling policy that attaches to your Auto Scaling group\. Amazon ECS then manages the scale\-in and scale\-out actions of the Auto Scaling group based on the load your tasks put on your cluster\. For more information about Auto Scaling group capacity providers, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

**Note**  
Amazon ECS cluster auto scaling is only supported with Auto Scaling group capacity providers\. For Amazon ECS workloads that are hosted on AWS Fargate, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.

## How cluster Auto Scaling works<a name="how-it-works"></a>

The following is the workflow used for Amazon ECS cluster auto scaling\. For more information, see [Turn on cluster Auto Scaling](turn-on-cluster-auto-scaling.md)\.

1. Create an Auto Scaling group

1. Create a capacity provider that uses that Auto Scaling group

1. Turn on managed scaling for the capacity provider

1. Associate the capacity provider with a cluster

1. Create a capacity provider strategy that uses the capacity provider\.

   The capacity provider strategy determines how the tasks are spread across the cluster's capacity providers\. When you run a standalone task or create a service, you either use the cluster's default capacity provider strategy or a capacity provider strategy that overrides the default one\.

For each Auto Scaling group capacity provider that's associated with a cluster, Amazon ECS creates and manages the following resources:
+ A low metric value CloudWatch alarm
+ A high metric value CloudWatch alarm
+ A target tracking scaling policy
**Note**  
Amazon ECS creates the target tracking scaling policy and attaches it to the Auto Scaling group\. To update the target tracking scaling policy, update the capacity provider managed scaling settings, rather than updating the scaling policy directly\.

When you turn off managed scaling or disassociate the capacity provider from a cluster, Amazon ECS removes both CloudWatch metrics and the target tracking scaling policy resources\.

The following metrics help to determine what actions to take:
+  `CapacityProviderReservation` \- The percent of cluster container instances in use for a specific capacity provider\. Amazon ECS generates this metric\.
+ `DesiredCapacity` \- The amount of capacity for the Auto Scaling group\.

Amazon ECS starts the cluster Auto Scaling process for each capacity provider that's associated with your cluster\. Every minute, Amazon ECS collects information, which determines whether the Auto Scaling group needs to scale in or out\. When launched tasks can't be placed on available instances, the Auto Scaling group scales out by launching new instances\. When there are running instances with no tasks, the Auto Scaling group scales in by terminating an instance with no running tasks\.

Amazon ECS initiates the `CapacityProviderReservation` metric, and then publishes the metric to CloudWatch in the `AWS/ECS/ManagedScaling` namespace\. The `CapacityProviderReservation` metric causes one of the following actions to occur:
+ **The `CapacityProviderReservation` value equals 100% \-** The Auto Scaling group doesn't need to scale in or scale out\. This is because all container instances are running at least one non\-daemon task\.
+ **The `CapacityProviderReservation` value is greater than 100% \- **There are no available instances for the launched tasks\. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. The Auto Scaling group uses this value to launch EC2 instances, and then register them with the cluster\.
+ **The `CapacityProviderReservation` value is less than 100% \- **There's at least one container instance that's not running a non\-daemon task\. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. The Auto Scaling group uses this value to terminate EC2 container instances, and then deregister them from the cluster\.

### Cluster Auto Scaling considerations<a name="cluster-auto-scaling-considerations"></a>

Consider the following when using cluster Auto Scaling:
+ Don't change or manage the desired capacity for the Auto Scaling group that's associated with a capacity provider with any scaling policies other than the one Amazon ECS manages\.
+ Amazon ECS uses the `AWSServiceRoleForECS` service\-linked IAM role for the permissions that it requires to call AWS Auto Scaling on your behalf\. For more information about using and creating Amazon ECS service\-linked IAM roles, see [Using service\-linked roles for Amazon ECS](using-service-linked-roles.md)\.
+ When using capacity providers with Auto Scaling groups, the user, group, or role that creates the capacity providers requires the `autoscaling:CreateOrUpdateTags` permission\. This is because Amazon ECS adds a tag to the Auto Scaling group when it associates it with the capacity provider\.
**Important**  
Make sure any tooling that you use doesn't remove the `AmazonECSManaged` tag from the Auto Scaling group\. If this tag is removed, Amazon ECS can't manage it when scaling your cluster\.
+ Cluster Auto Scaling doesn't modify the **MinimumCapacity** or **MaximumCapacity** for the group\. For the group to scale out, the value for **MaximumCapacity** must be greater than zero\.
+ When Auto Scaling \(managed scaling\) is turned on, a capacity provider can only be connected to one cluster at the same time\. If your capacity provider has managed scaling turned off, you can associate it with multiple clusters\.
+ When managed scaling is turned off, the capacity provider doesn't scale in or scale out\. You can use a capacity provider strategy to balance your tasks between capacity providers\.
+ Amazon ECS uses placement strategy and placement constraints with the existing capacity at the current time\. A placement strategy can spread tasks across Availability Zones or Amazon ECS instances\. This eventually spreads all the tasks and all the instances so that each running task launches on its own dedicated instance\. To prevent this, don't use the `spread` strategy together with the `binpack` strategy\. For more information, see [Amazon ECS task placement strategies](task-placement-strategies.md)\.

Consider the following when you use the new console:
+ By default, the Amazon ECS managed scaling feature is on\. For more information, see [Managed scale\-out behavior](#managed-scaling-scaleout)\.
+ By default, managed termination is off\.
+ By default, Auto Scaling instance scale\-in protection is off\. For more information, see [Using instance scale\-in protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-instance-protection.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ The Auto Scaling group that's used with your capacity provider can't use instance weighting settings\. Instance weighting isn't supported when used together with an Amazon ECS capacity provider\.

## Managed termination protection<a name="managed-termination-protection"></a>

Auto Scaling instance scale\-in protection controls which EC2 instances can be terminated\. Instances with the scale\-in feature turned on can't be terminated during the scale\-in process\.When you use managed termination protection, Amazon ECS only terminates EC2 instances that don't any have running Amazon ECS tasks\. When you use managed termination protection, Amazon ECS first turns on the scale\-in protection option for the EC2 instances in the Auto Scaling group\. Then, Amazon ECS places the tasks on the instances\. When all non\-daemon tasks are stopped on an instance, Amazon ECS initiates the scale\-in process and turns off scale\-in protection for the EC2 instance\. The Auto Scaling group can then terminate the instance\.

### Managed termination protection considerations<a name="managed-termination-protection-considerations"></a>

Consider the following when using managed termination protection with the new console:
+ The Auto Scaling group is automatically configured to use managed instance protection and managed scaling\.
+ By default, Auto Scaling instance scale\-in protection is off\. For more information, see [Using instance scale\-in protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-instance-protection.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Managed scale\-out behavior<a name="managed-scaling-scaleout"></a>

When you have Auto Scaling group capacity providers that use managed scaling, Amazon ECS estimates the optimal number of instances to add to your cluster\. Then, Amazon ECS uses this value to determine how many instances to request\. The following describes the scale\-out behavior in more detail\.

1. Amazon ECS selects a capacity provider for each task by following the capacity provider strategy from the service, from the standalone task, or from the cluster default\. Amazon ECS follows the rest of these steps for a single capacity provider\.

   If a task doesn't have a capacity provider strategy, then that task is ignored by capacity providers\. A pending task that doesn't have a capacity provider strategy won't cause any capacity provider to scale out\. Tasks or services can't set a capacity provider strategy if that task or service sets a launch type\.

1. Group all of the provisioning tasks for this capacity provider so that each group has the same exact resource requirements\.

1. When you use multiple instance types in an AS; group, the instance types in the Auto Scaling group are sorted by their parameters\. These parameters include vCPU, memory, elastic network interfaces \(ENIs\), ports, and GPUs\. The smallest and the largest instance types for each parameter are selected\. For more information about how to choose the instance type, see [Characterizing your application](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/capacity-autoscaling.html#capacity-autoscaling-app) in the *Amazon ECS Best Practices Guide*\.
**Important**  
If a group of tasks have resource requirements that are greater than the smallest instance type in the Auto Scaling group, then that group of tasks can’t run with this capacity provider\. The capacity provider doesn’t scale the Auto Scaling group\. The tasks remain in the `PENDING` state\.  
To prevent tasks from staying in the `PENDING` state, we recommend that you create separate Auto Scaling groups and capacity providers for different minimum resource requirements\. When you run tasks or create services, only add capacity providers to the capacity provider strategy that can run the task on the smallest instance type in the Auto Scaling group\. For other parameters, you can use placement constraints

1. For each group of tasks, Amazon ECS calculates the number of instances that are required to run the unplaced tasks\. This calculation uses a `binpack` strategy\. This strategy accounts for the vCPU, memory, elastic network interfaces \(ENI\), ports, and GPUs requirements of the tasks\. It also accounts for the resource availability of the Amazon EC2 instances\. The values for the largest instance types are treated as the maximum calculated instance count\. The values for the smallest instance type are used as protection\. If the smallest instance type can't run at least one instance of the task, the calculation considers the task as not compatible\. As a result, the task is excluded from scale\-out calculation\. When all the tasks aren't compatible with the smallest instance type, cluster Auto Scaling stops and the `CapacityProviderReservation` value remains at 100%\.

1. Amazon ECS publishes the `CapacityProviderReservation` metric to CloudWatch with respect to the `minimumScalingStepSize` if either of the following is the case\. Either, the maximum calculated instance count is less than the minimum scaling step size\. Or, the lower value of either the `maximumScalingStepSize` or the maximum calculated instance count\.

1. CloudWatch alarms use the `CapacityProviderReservation` metric for capacity providers\. When the `CapacityProviderReservation` metric is greater than the `targetCapacity` value, alarms also increase the `DesiredCapacity` of the Auto Scaling group\. The `targetCapacity` value is a capacity provider setting that's sent to the CloudWatch alarm during the cluster auto scaling activation phase\. 

   You can set the `targetCapacity` when you create the Auto Scaling group, or modify the value after the group is created\. The default is 100%\.

1. The Auto Scaling group launches additional EC2 instances\. To prevent over\-provisioning of the scale\-out operation, Auto Scaling makes sure that recently launched EC2 instance capacity is stabilized before it launches new instances\. Auto Scaling checks if all existing instances have passed the `instanceWarmupPeriod` \(now minus the instance launch time\)\. The scale\-out is blocked for Instances that are within the `instanceWarmupPeriod`\.

   The default number of seconds for a newly launched instance to warm up is 300\.

For more information, see [Deep dive on Amazon ECS cluster auto scaling](https://aws.amazon.com/blogs/containers/deep-dive-on-amazon-ecs-cluster-auto-scaling/)\.

### Scale\-out considerations<a name="scale-out-considerations"></a>

 Consider the following for the scale\-out process:

1. Although there are multiple placement constraints, we recommend that you only use the `distinctInstance` task placement constraint\. This prevents the scale\-out process from stopping because you're using a placement constraint that's not compatible with the sampled instances\.

1. Managed scaling works best if your Auto Scaling group uses the same or similar instance types\. For more information, see [Managed scale\-out behavior](#managed-scaling-scaleout)\.

1. When a scale\-out process is required and there are no available container instances, and then a container instance becomes available, Amazon ECS always scales\-out to 200% \(two instances\)\.

1. The second scale\-out step needs to wait until the `instanceWarmupPeriod` expires, which might affect the overall scale limit\. If you need to reduce this time, make sure that `instanceWarmupPeriod` is large enough for the EC2 instance to launch and start the Amazon ECS agent \(which prevents overprovisioning\)\.

1. Cluster Auto Scaling supports Launch Configuration, Launch Templates, and multiple instance types in the capacity provider Auto Scaling group\. You can also use attribute\-based instance type selection without multiple instances types\.

1. When using an Auto Scaling group with On\-Demand instances and multiple instance types or Spot Instances, place the larger instance types higher in the priority list and don't specify a weight\. Specifying a weight isn't supported at this time\. For more information, see [Auto Scaling groups with multiple instance types](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-purchase-options.html) in the *AWS Auto Scaling User Guide*\.

1. Amazon ECS then launch either the `minimumScalingStepSize`, if the maximum calculated instance count is less than the minimum scaling step size, or the lower of either the `maximumScalingStepSize` or the maximum calculated instance count value\.

1. If an Amazon ECS service or the `run-task` API launches a task and the capacity provider container instances don't have enough resources to start the task, then Amazon ECS limits the number of tasks with this status for each cluster and prevents any tasks from exceeding this limit\. For more information, see [Amazon ECS service quotas](service-quotas.md)\.

## Managed scale\-in behavior<a name="managed-scaling-scalein"></a>

Amazon ECS monitors container instances for each capacity provider within a cluster\. When a container instance isn't running any tasks, the container instance is considered empty and Amazon ECS starts the scale\-in process\. The following describes the scale\-in behavior in more detail:

1. Amazon ECS calculates the number of container instances that are empty\. A container instance is considered empty even when daemon tasks are running\.

1. Amazon ECS sets the `CapacityProviderReservation` value to 100 minus the number of container instances that are empty\. For example, if the number of empty container instances is two, the value is 98%\. Then, Amazon ECS publishes the metric to CloudWatch\.

1. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. Then, one of the following actions occurs:
   + If you don't use capacity provider managed termination, the Auto Scaling group selects EC2 instances using the Auto Scaling group termination policy and terminates the instances until the number of EC2 instances reaches the `DesiredCapacity`\. The container instances are then deregistered from the cluster\.
   + If all the container instances use managed termination protection, Amazon ECS removes the scale\-in protection on the container instances that are empty\. The Auto Scaling group will then be able to terminate the EC2 instances\. The container instances are then deregistered from the cluster\.

### Scale\-in considerations<a name="scale-in-considerations"></a>

 Consider the following for the scale\-in process:
+ When there are no running non\-daemon tasks, Amazon ECS container instances are considered available for scale in\.
+ CloudWatch scale\-in alarms require 15 data points \(15 minutes\) before the scale\-in process for the Auto Scaling group starts\. After the scale\-in process starts until Amazon ECS needs to reduce the number of registered container instances, the Auto Scaling group sets the `DesireCapacity` value to be greater than one instance and less than 50% each minute\.
+ When Amazon ECS requests a scale\-out \(when `CapacityProviderReservation` is greater than 100\) while a scale\-in process is in progress, the scale\-in process is stopped and starts from the beginning if required\.

## Target tracking considerations<a name="target-tracking"></a>

When creating or updating a capacity provider with managed scaling turned on, you can set the `targetCapacity` percentage\. This way, you can keep spare capacity so that future tasks can launch more quickly\. Spare capacity improves this by not waiting for the Auto Scaling group to launch more instances\. Amazon ECS uses the target capacity value to manage the CloudWatch metric that the service creates to facilitate cluster auto scaling\. Amazon ECS manages the CloudWatch metric\. This way, the Auto Scaling group is treated as a steady state so that no scaling action is required\. The values can be from 0\-100%\. For example, to configure Amazon ECS to keep 10% free capacity on top of that used by Amazon ECS tasks, set the target capacity value to 90%\.

Consider the following when setting the `targetCapacity` value on a capacity provider\.
+ A `targetCapacity` value of less than 100% represents the amount of free capacity \(Amazon EC2 instances\) that need to be present in the cluster\. Free capacity means that there are no running tasks\.
+ Placement constraints such as Availability Zones, without additional `binpack` forces Amazon ECS to eventually run one task for each instance, which might not be the desired behavior\. To prevent this behavior, don't use the `spread` strategy together with the `binpack` strategy\.

## Update on the way Amazon ECS creates resources for cluster auto scaling<a name="update-ecs-resources-cas"></a>

As of May 27, 2022, Amazon ECS changed how it manages the resources that facilitate cluster Auto Scaling\. To simplify the experience, Amazon ECS no longer requires an AWS Auto Scaling scaling plan when you turn on managed scaling for an Auto Scaling group capacity provider\. *This change has no functional impact on your cluster Auto Scaling workflows and no pricing or performance impact*\. 

### Capacity providers created before May 27, 2022<a name="to-use-cp-created-before-May27-2022"></a>

Capacity providers that were created before May 27, 2022, and that use AWS Auto Scaling scaling plans continue to function as before\. 

Review the following considerations: 
+ We don't recommend that you update or delete the `ECS-managed` AWS Auto Scaling scaling plan or the scaling policy resources that are associated with your capacity providers\. 
+ You can access the scaling plan resource for clusters on the Auto Scaling console and by the `describe-clusters` with attachments\. For more information, see the API documentation [DescribeClusters](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeClusters.html)\. 
+ You can't add scaling policies to the Auto Scaling group that functions as the cluster capacity provider\.
+ The number of Auto Scaling plans for each account is limited\. For more information, see [Quotas for your scaling plans](https://docs.aws.amazon.com/autoscaling/plans/userguide/scaling-plan-quotas.html) in the *Amazon EC2 Auto Scaling User Guide*\.

### Capacity providers created on or after May 27, 2022<a name="to-use-cp-created-after-May27-2022"></a>

As of May 27, 2022, Amazon ECS no longer creates an AWS Auto Scaling scaling plan for newly created capacity providers\. Instead, Amazon ECS uses the target tracking scaling policy attached to the Auto Scaling group to perform dynamic scaling based on your target capacity specification\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

With this new release, you can use an existing Auto Scaling group with a scaling policy for use when creating a new capacity provider\. We don't recommend that you modify the ECS managed scaling policy or plan resources\. However, when creating new capacity provider resources, if you have customized tooling that made modifications to the AWS Auto Scaling scaling plan, do one of the following:
+ \(Recommended\) Update your capacity provider to modify the Amazon ECS managed scaling settings\. For more information, see [UpdateCapacityProvider](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateCapacityProvider.html)\. 
+ Update the scaling policy associated with your Auto Scaling group to modify the target tracking configuration used\. For more information, see [PutScalingPolicy](https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_PutScalingPolicy.html)\.