# Amazon ECS cluster auto scaling<a name="cluster-auto-scaling"></a>

Amazon ECS cluster auto scaling enables you to have more control over how you scale the Amazon EC2 instances within a cluster\. When creating an Auto Scaling group capacity provider with managed scaling enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group used when creating the capacity provider\. On your behalf, Amazon ECS creates an AWS Auto Scaling scaling plan with a target tracking scaling policy based on the target capacity value you specify\. Amazon ECS then associates this scaling plan with your Auto Scaling group\.

For each of the Auto Scaling group capacity providers with managed scaling enabled, an Amazon ECS managed CloudWatch metric with the prefix `AWS/ECS/ManagedScaling` is created along with two CloudWatch alarms\. The CloudWatch metrics and alarms are used to monitor the Amazon EC2 instance capacity in your Auto Scaling groups and will trigger the Auto Scaling group to scale in and scale out as needed\.

Each cluster has one or more Auto Scaling group capacity providers and an optional default capacity provider strategy\. The capacity providers determine the infrastructure to use for the tasks, and the capacity provider strategy determines how the tasks are spread across the capacity providers\. When you run a task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\. For more information about capacity providers, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.

## Cluster auto scaling considerations<a name="cluster-auto-scaling-considerations"></a>

The following should be considered when using cluster auto scaling:
+ Amazon ECS uses the `AWSServiceRoleForECS` service\-linked IAM role for the permissions it requires to call AWS Auto Scaling, on your behalf\. For more information on using and creating Amazon ECS service\-linked IAM roles, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.
+ Cluster auto scaling is not available in the Asia Pacific \(Osaka\) Region\.
+ When using capacity providers with Auto Scaling groups, the IAM user creating the capacity providers, needs the `autoscaling:CreateOrUpdateTags` permission\. This is because Amazon ECS adds a tag to the Auto Scaling group when it associates it with the capacity provider\.
**Important**  
Ensure any tooling you use does not remove the `AmazonECSManaged` tag from the Auto Scaling group\. If this tag is removed, Amazon ECS is not able to manage it when scaling your cluster\.
+ Managed scaling works best if your Auto Scaling group uses the same or similar instance types\. For more information, see [Managed scale\-out behavior](#managed-scaling-scaleout)\.
+ When using an Auto Scaling group with On\-Demand instances and multiple instance types, place the larger instance types higher in the priority list and don't specify a weight\. Specifying a weight is not supported at this time\. For more information, see [Auto Scaling groups with multiple instance types](https://docs.aws.amazon.com/autoscaling/ec2/userguide/asg-purchase-options.html) in the *AWS Auto Scaling User Guide*\.
+ When creating a service, specifying a task placement strategy that spreads across Availability Zones or a binpack strategy based on CPU or memory works best\. Don't use an instance spread strategy as scaling works slower with that strategy type\.
+ The desired capacity for the Auto Scaling group associated with a capacity provider shouldn't be changed or managed any any scaling policies other than the one Amazon ECS manages\.

## Managed scale\-out behavior<a name="managed-scaling-scaleout"></a>

When using Auto Scaling group capacity providers with managed scaling enabled, Amazon ECS estimates the lower bound on the optimal number of instances to add to your cluster and uses this value to determine how many instances to request\. The following describes the scale\-out behavior in more detail\.

1. Group all of the provisioning tasks so that each group has the same exact resource requirements\.

1. When multiple instance types are used, the instances in the Auto Scaling group are sorted by their attributes, such as vCPU, memory, elastic network interface \(ENI\), ports, and GPUs and the largest instance types for each attribute are selected\.

1. For each group of tasks, the number of instances required to run the unplaced tasks is calculated\. This calculation uses a `binpack` strategy which accounts for the vCPU, memory, elastic network interfaces \(ENI\), ports, and GPUs requirements of the tasks and the resource availability of the Amazon EC2 instances\. This value will be treated as the maximum calculated instance count\.
**Note**  
This calculation takes into account any task placement constraints that are defined, but we recommend only using the `distinctInstance` task placement constraint\.

1. Amazon ECS will then launch either the `minimumScalingStepSize`, if the maximum calculated instance count is less than the minimum scaling step size, or the lower of either the `maximumScalingStepSize` or the maximum calculated instance count value\.

For a more detailed explanation of how this logic works, see [Deep dive on Amazon ECS cluster auto scaling](https://aws.amazon.com/blogs/containers/deep-dive-on-amazon-ecs-cluster-auto-scaling/)\.