# Auto Scaling group capacity providers<a name="asg-capacity-providers"></a>

Amazon ECS capacity providers can use Auto Scaling groups to manage the Amazon EC2 instances registered to their clusters\. You can use the managed scaling feature to have Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group or you can manage the scaling actions yourself\. For more information, see [Amazon ECS cluster Auto Scaling](cluster-auto-scaling.md)\.

**Topics**
+ [Auto Scaling group capacity providers considerations](#asg-capacity-providers-considerations)
+ [Creating an Auto Scaling group](asg-capacity-providers-create-auto-scaling-group.md)
+ [Creating an Auto Scaling group capacity provider using the classic console](asg-capacity-providers-create-capacity-provider.md)
+ [Updating an Auto Scaling group capacity provider using the classic console](asg-capacity-providers-update-capacity-provider.md)
+ [Creating a cluster with an Auto Scaling group capacity provider](asg-capacity-providers-create-cluster.md)
+ [Deleting an Auto Scaling group capacity provider using the classic console](asg-capacity-providers-delete-capacity-provider.md)

## Auto Scaling group capacity providers considerations<a name="asg-capacity-providers-considerations"></a>

Consider the following when using Auto Scaling group capacity providers in the classic console:
+ We recommend that you create a new empty Auto Scaling group to use with a capacity provider rather than using an existing one\. If you use an existing Auto Scaling group, any Amazon EC2 instances associated with the group that were already running and registered to an Amazon ECS cluster prior to the Auto Scaling group being used to create a capacity provider may not be properly registered with the capacity provider\. This may cause issues when using the capacity provider in a capacity provider strategy\. The `DescribeContainerInstances` API can confirm whether a container instance is associated with a capacity provider or not\.
**Note**  
To create an empty Auto Scaling group, set the desired count to zero\. After you have created the capacity provider and associated it with a cluster, you can then scale it out\.
+ An Auto Scaling group must have a `MaxSize` greater than zero to turn on it to scale out\.
+ If the Auto Scaling group is unable to scale out to accommodate the number of tasks run, the tasks will fail to transition beyond the `PROVISIONING` state\.
+ When you use managed termination protection, you must also use managed scaling otherwise managed termination protection won't work\.
+ When using managed scaling, the Auto Scaling group shouldn't have any scaling policies attached to it other than the ones Amazon ECS creates, otherwise the Amazon ECS created scaling plans will receive an `ActiveWithProblems` error\. For more information, see [Avoiding the ActiveWithProblems error](https://docs.aws.amazon.com/autoscaling/plans/userguide/gs-best-practices.html#gs-activewithproblems) in the *AWS Auto Scaling User Guide*\.
+ You can add a warm pool to your Auto Scaling group\. A warm pool is a group of pre\-initialized Amazon ECS instances that are ready to be included in the cluster whenever your application needs to scale out\. For more information about warm pools, see [Using a warm pool for your Auto Scaling group](asg-capacity-providers-create-auto-scaling-group.md#using-warm-pool) 
+ The Auto Scaling group can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.