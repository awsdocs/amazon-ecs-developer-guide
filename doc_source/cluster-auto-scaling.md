# Amazon ECS cluster auto scaling<a name="cluster-auto-scaling"></a>

Amazon ECS cluster auto scaling enables you to have more control over how you scale the Amazon EC2 instances within a cluster\. When creating an Auto Scaling group capacity provider with managed scaling enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group used when creating the capacity provider\. On your behalf, Amazon ECS creates an AWS Auto Scaling scaling plan with a target tracking scaling policy based on the target capacity value you specify\. Amazon ECS then associates this scaling plan with your Auto Scaling group\.

For each of the Auto Scaling group capacity providers with managed scaling enabled, an Amazon ECS managed CloudWatch metric with the prefix `AWS/ECS/ManagedScaling` is created along with two CloudWatch alarms\. The CloudWatch metrics and alarms are used to monitor the Amazon EC2 instance capacity in your Auto Scaling groups and will trigger the Auto Scaling group to scale in and scale out as needed\.

**Note**  
Managed scaling is only supported in Regions that AWS Auto Scaling is available in\. For a list of supported Regions, see [AWS Auto Scaling Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/autoscaling_region.html) in the *Amazon Web Services General Reference*\.

Each cluster has one or more Auto Scaling group capacity providers and an optional default capacity provider strategy\. The capacity providers determine the infrastructure to use for the tasks, and the capacity provider strategy determines how the tasks are spread across the capacity providers\. When you run a task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\. For more information about capacity providers, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.

## Cluster auto scaling considerations<a name="cluster-auto-scaling-considerations"></a>

The following should be considered when using cluster auto scaling:
+ Amazon ECS uses the AWSServiceRoleForECS service\-linked IAM role for the permissions it requires to call AWS Auto Scaling, on your behalf\. For more information on using and creating Amazon ECS service\-linked IAM roles, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\.
+ When using capacity providers with Auto Scaling groups, the IAM user creating the capacity providers, needs the `autoscaling:CreateOrUpdateTags` permission\. This is because Amazon ECS adds a tag to the Auto Scaling group when it associates it with the capacity provider\.
**Important**  
Ensure any tooling you use does not remove the `AmazonECSManaged` tag from the Auto Scaling group\. If this tag is removed, Amazon ECS is not able to manage it when scaling your cluster\.