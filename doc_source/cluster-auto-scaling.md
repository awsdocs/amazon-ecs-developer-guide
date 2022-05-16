# Amazon ECS cluster Auto Scaling<a name="cluster-auto-scaling"></a>

Amazon ECS cluster auto scaling provides control over how you scale the Amazon EC2 instances within a cluster\. When you use managed scaling, Amazon ECS creates the Auto Scaling group capacity provider infrastructure, and manages the scale\-in and scale\-out actions of the Auto Scaling group based on your clusters' tasks load\.

Auto Scaling is for the EC2 launch type\. For information about Fargate capacity providers, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.

We recommend that you review [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

## How cluster Auto Scaling works<a name="how-it-works"></a>

There are two options you need to configure in for cluster Auto Scaling \(for more information, see [Turn on cluster Auto Scaling](turn-on-cluster-auto-scaling.md)\):
+ Associate a capacity provider with a cluster
+ Turn on managed scaling for the associated capacity provider

When these options are configured, Amazon ECS creates the following dedicated resources for each capacity provider associated with a cluster:
+ An Auto Scaling plan
+ A low metric value CloudWatch alarm
+ A high metric value CloudWatch alarm
+ An Auto Scaling group

When you turn off managed scaling, or disassociate the capacity provider from the cluster, Amazon ECS removes the resources listed above\.

The following metrics help to determine what actions to take:
+  `CapacityProviderReservation` \- The percent of cluster container instances in use for a specific capacity provider\. Amazon ECS generates this metric\.
+ `DesiredCapacity` \- The amount of capacity for the Auto Scaling group\. 

Amazon ECS starts the cluster Auto Scaling process for each capacity provider associated with your cluster\. Every minute, Amazon ECS collects information which determines whether the Auto Scaling group needs to scale in or out\. When launched tasks cannot be placed on available instances, the Auto Scaling group scales\-out by launching new instances\. When there are running instances with no tasks, the Auto Scaling group scales\-in by terminating an instance with no running tasks\.

Amazon ECS initiates the `CapacityProviderReservation` metric, and then publishes the metric to CloudWatch in the `AWS/ECS/ManagedScaling` namespace\. The `CapacityProviderReservation` metric causes one of the following actions to happen:
+ The `CapacityProviderReservation` value equals 100% \- The Auto Scaling group does not need to scale\-in or scale\-out, because all container instances are running at least one non\-daemon task\.
+ The `CapacityProviderReservation` value is greater than 100% \- There are no available instances for the launched tasks\. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. The Auto Scaling group uses this value to launch EC2 instances, and then register them with the cluster\.
+ The `CapacityProviderReservation` value is less than 100% \- There is at least one container instance that is not running a non\-daemon task\. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. The Auto Scaling group uses this value to terminate EC2 container instances, and then deregister them from the cluster\.

### Cluster Auto Scaling considerations<a name="cluster-auto-scaling-considerations"></a>

Consider the following when using cluster Auto Scaling:
+ The Auto Scaling group that acts as the cluster capacity provider must not have any scaling policies\.
+ The number of Auto Scaling plans per account is limited\. For more information, see [Quotas for your scaling plans](https://docs.aws.amazon.com/autoscaling/plans/userguide/scaling-plan-quotas.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ The desired capacity for the Auto Scaling group associated with a capacity provider shouldn't be changed or managed by any scaling policies other than the one Amazon ECS manages\.
+ Amazon ECS uses the `AWSServiceRoleForECS` service\-linked IAM role for the permissions it requires to call AWS Auto Scaling, on your behalf\. For more information on using and creating Amazon ECS service\-linked IAM roles, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.
+ When using capacity providers with Auto Scaling groups, the IAM user who creates the capacity providers, needs the `autoscaling:CreateOrUpdateTags` permission because Amazon ECS adds a tag to the Auto Scaling group when it associates it with the capacity provider\.
**Important**  
Make sure any tooling you use does not remove the `AmazonECSManaged` tag from the Auto Scaling group\. If this tag is removed, Amazon ECS is not able to manage it when scaling your cluster\.
+ Cluster Auto Scaling does not modify the **MinimumCapacity** or **MaximumCapacity** for the group\. In order for the group to scale\-out, the value for **MaximumCapacity** must be greater than 0\.
+ A capacity provider can only be connected to one cluster at the same time when Auto Scaling \(managed scaling\) is turned on\. If your capacity provider has managed scaling turned off, you can associate it with multiple clusters\.
+ When managed scaling is turned off, the capacity provider does not perform scale\-in or scale\-out operations\. For this case, you can use a capacity provider strategy to balance your tasks between capacity providers\.
+ Amazon ECS uses placement strategy and placement constraints with the existing capacity at the current time\. A placement strategy can spread tasks across Availability Zones or Amazon ECS instances\. This eventually spreads all tasks all instances so that each running task launches on its own dedicated instance\. To prevent this behavior, do not use the `spread` strategy in conjunction with the `binpack` strategy\. For more information, see [Amazon ECS task placement strategies](task-placement-strategies.md)\.

The following considerations apply when you use the new console:
+ The Amazon ECS managed scaling feature is on by default\. For more information, see [Managed scale\-out behavior](#managed-scaling-scaleout)\.
+ Managed termination is off by default\.
+ Auto Scaling instance\-scale\-in protection is off by default\. For more information, see [Using instance scale\-in protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-instance-protection.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ The Auto Scaling group used with your capacity provider can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.

## Managed termination protection<a name="managed-termination-protection"></a>

Auto Scaling instance scale\-in protection controls which EC2 instances can be terminated\. Instances with the scale\-in feature turned on cannot be terminated during the scale\-in process\. Managed termination protection means that Amazon ECS only terminates EC2 instances that do not any have running non\-daemon Amazon ECS tasks\. When you use managed termination protection, Amazon ECS first turns on the scale\-in protection option for the EC2 instances in the Auto Scaling group, and then places the tasks on the instances\. When all non\-daemon tasks are stopped on an instance, Amazon ECS initiates the scale\-in process and turns off scale\-in protection for the EC2 instance\. The Auto Scaling group can then terminate the instance\.

### Managed termination protection considerations<a name="managed-termination-protection-considerations"></a>

Consider the following when using managed termination protection with the new console:
+ The Auto Scaling group is automatically configured to use managed instance protection and managed scaling\.
+ Auto Scaling instance scale\-in protection is off by default\. For more information, see [Using instance scale\-in protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-instance-protection.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Managed scale\-out behavior<a name="managed-scaling-scaleout"></a>

When you have Auto Scaling group capacity providers that use managed scaling, Amazon ECS estimates the lower bound for the optimal number of instances to add to your cluster and uses this value to determine how many instances to request\. The following describes the scale\-out behavior in more detail\.

1. Group all of the provisioning tasks so that each group has the same exact resource requirements\.

1. When you use multiple instance types in a group, the instances in the Auto Scaling group are sorted by their parameters, such as vCPU, memory, elastic network interfaces \(ENIs\), ports, and GPUs\. The smallest and the largest instance types for each parameter are selected\.

1. For each group of tasks, Amazon ECS calculates the number of instances required to run the unplaced tasks\. This calculation uses a `binpack` strategy which accounts for the vCPU, memory, elastic network interfaces \(ENI\), ports, and GPUs requirements of the tasks and the resource availability of the Amazon EC2 instances\. The values for the largest instance types are treated as the maximum calculated instance count\. The values for the smallest instance type are used as protection\. If the smallest instance type cannot run at least one instance of the task, the calculation considers the task as not compatible and it is excluded from scale\-out calculation\. When all tasks are not compatible with the smallest instance type, cluster Auto Scaling stops and the `CapacityProviderReservation` value remains at 100%\.

1. Amazon ECS publishes the `CapacityProviderReservation` metric to CloudWatch with respect to the `minimumScalingStepSize` if the maximum calculated instance count is less than the minimum scaling step size, or the lower value of either the `maximumScalingStepSize` or the maximum calculated instance count\.

1. CloudWatch alarms consume the `CapacityProviderReservation` metric \(published by Amazon ECS\) for capacity providers and increase the `DesiredCapacity` of the Auto Scaling group only when the value is greater than the `targetCapacity` value\. The `targetCapacity` value is a capacity provider setting that is sent to the CloudWatch alarm during the cluster auto scaling activation phase\. 

   You can set the `targetCapacity` when you create the Auto Scaling group, or modify it after the group is created\. The default is 100%\.

1. The Auto Scaling group launches additional EC2 instances\. To prevent over\-provisioning of the scale\-out operation, Auto Scaling makes sure that recently launched EC2 instance capacity is stabilized before it launches new instances\. Auto Scaling checks if all existing instances have passed the `instanceWarmupPeriod` \(now minus the instance launch time\) If any instance is within the `instanceWarmupPeriod`, the scale\-out is blocked until that time\. 

   The default number of seconds for a newly launched instance to warm up is 300\.

For a more detailed explanation of how this logic works, see [Deep dive on Amazon ECS cluster auto scaling](https://aws.amazon.com/blogs/containers/deep-dive-on-amazon-ecs-cluster-auto-scaling/)\.

### Scale\-out considerations<a name="scale-out-considerations"></a>

 Consider the following for the scale\-out process:

1. Although there are multiple placement constraints, we recommend that you only use the `distinctInstance` task placement constraint\. This prevents the scale\-out process from stopping because you are using a placement constraint that is not compatible with the sampled instances\.

1. Managed scaling works best if your Auto Scaling group uses the same or similar instance types\. For more information, see [Managed scale\-out behavior](#managed-scaling-scaleout)\.

1. When a scale\-out process is required and there are no available container instances, and then a container instance becomes available, Amazon ECS always scales\-out to 200% \(two instances\)\.

1. The `instanceWarmupPeriod` might affect the overall scale limit, because second scale\-out step needs to wait until the `instanceWarmupPeriod` time expires\. \. If you need to reduce this time, make sure that the value is large enough for the EC2 instance to launch and start the Amazon ECS agent \(which prevents over\-provisioning\)\.

1. Cluster Auto Scaling supports Launch Configuration, Launch Templates and multiple instance types in the capacity provider Auto Scaling group\. You can also use attribute\-based instance type selection without multiple instances types\.

1. When using an Auto Scaling group with On\-Demand instances and multiple instance types or Spot Instances, place the larger instance types higher in the priority list and don't specify a weight\. Specifying a weight is not supported at this time\. For more information, see [Auto Scaling groups with multiple instance types](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-purchase-options.html) in the *AWS Auto Scaling User Guide*\.

1. Amazon ECS will then launch either the `minimumScalingStepSize`, if the maximum calculated instance count is less than the minimum scaling step size, or the lower of either the `maximumScalingStepSize` or the maximum calculated instance count value\.

1. If an Amazon ECS service, or the `run-task` API launches a task and the capacity provider container instances do not have enough resources to start the task, then Amazon ECS limits the number of tasks with this status for each cluster and prevents any tasks to run above this limit\. For more information, see [Amazon ECS service quotas](service-quotas.md)\.

## Managed scale\-in behavior<a name="managed-scaling-scalein"></a>

Amazon ECS monitors container instances for each capacity provider within cluster\. When at least one container instance does not have at least one running task it considered as empty and Amazon ECS starts scale\-in process\. The following describes the scale\-in behavior in more detail\.

1. Amazon ECS calculates the number of empty container instances\. A container instance is considered empty when there are no no\-daemon tasks running\.

1. Amazon ECS sets the `CapcityProviderReservation` value to 100 \- the number of empty container instances\. For example, if the number of empty container instances is 2, the value is set to 98%\. Then, Amazon ECS publishes the metric to CloudWatch\.

1. The `CapacityProviderReservation` metric generates a CloudWatch alarm\. This alarm updates the `DesiredCapacity` value for the Auto Scaling group\. One of the following actions happens:
   + If you do not use capacity provider managed termination, the Auto Scaling group terminates the number Auto Scaling group will terminate the number of EC2 instances to reach `DesiredCapacity`\. The container instances are then deregistered from the cluster\.
   + If all the container instances use capacity provider managed termination protection, Amazon ECS removes the scale\-in protection on the container instances that do not have non\-daemon tasks running\. The Auto Scaling group will then be able to terminate the EC2 instances\. The container instances are then deregistered from the cluster\.

### Scale\-in considerations<a name="scale-in-considerations"></a>

 Consider the following for the scale\-in process:
+ Amazon ECS container instance are considered available for scale\-in when there are no running non\-daemon tasks\.
+ CloudWatch scale\-in alarms require 15 data points \(15 minutes\) before the scale\-in process for the Auto Scaling group starts\. After the scale\-in process starts until Amazon ECS needs to reduce the number of registered container instances, the Auto Scaling group sets the `DesireCapacity` value to be greater than one instance and less than 10% each minute\.
+ When Amazon ECS requests a scale\-out \(when `CapcityProviderReservation` is greater than 100\) while a scale\-in process is in progress, the scale\-in process is stopped and will start from the beginning if required\.

## `TargetTracking`<a name="target-tracking"></a>

 You can set the `TargetTracking` so that future tasks can launch more quickly by not waiting for the Auto Scaling group to launch more instances\. This value will set the CapacityProviderReservation metric in a way that the Auto Scaling group is treated as a steady state so that no scaling action is required\. The values can be from 0\-100%\. To configure Amazon ECS to keep 10% free capacity on top of that used by Amazon ECS tasks, set the value to 90%\.

### `TargetTracking` considerations<a name="target-tracking-considerations"></a>

 Consider the following when using `TargetTracking`:
+ A `TargetTracking` value of less than 100% represents the amount of free capacity \(EC2 instances\) that need to be present in the cluster\. Free capacity means that there are no running tasks\.
+ Placement constraints such as Availability Zones, without additional `binpack` forces Amazon ECS to eventually run one task per instance, which might not be the desired behavior\. To prevent this behavior, do not use the `spread` strategy in conjunction with the `binpack` strategy\.