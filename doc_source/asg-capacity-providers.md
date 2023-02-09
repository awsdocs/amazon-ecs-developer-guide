# Auto Scaling group capacity providers<a name="asg-capacity-providers"></a>

Amazon ECS capacity providers can use Auto Scaling groups to manage the Amazon EC2 instances registered to their clusters\. You can use the managed scaling feature to have Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group or you can manage the scaling actions yourself\. For more information, see [Amazon ECS cluster Auto Scaling](cluster-auto-scaling.md)\.

## Auto Scaling group capacity providers considerations<a name="asg-capacity-providers-considerations"></a>

Consider the following when using Auto Scaling group capacity providers in the classic console:
+ We recommend that you create a new empty Auto Scaling group to use with a capacity provider rather than using an existing one\. If you use an existing Auto Scaling group, any Amazon EC2 instances that are associated with the group that were already running and registered to an Amazon ECS cluster before the Auto Scaling group being used to create a capacity provider might not be properly registered with the capacity provider\. This might cause issues when using the capacity provider in a capacity provider strategy\. The `DescribeContainerInstances` API can confirm whether a container instance is associated with a capacity provider or not\.
**Note**  
To create an empty Auto Scaling group, set the desired count to zero\. After you created the capacity provider and associated it with a cluster, you can then scale it out\.  
When you use the Amazon ECS console **Create Cluster** with the **Amazon EC2 instances** option under **Infrastructure**, Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group could then be used in a capacity provider for that cluster\.
+ An Auto Scaling group must have a `MaxSize` greater than zero to scale out\.
+ If the Auto Scaling group can scale out to accommodate the number of tasks run, the tasks fails to transition beyond the `PROVISIONING` state\.
+ When you use managed termination protection, you must also use managed scaling\. Otherwise, managed termination protection won't work\.
+ When managed scaling is turned on, the Auto Scaling group capacity provider creates a scaling policy resource to manage the scaling of your Auto Scaling group\. You can identify these resources by the `ECSManaged` prefix\. 
+ Don't modify the scaling policy resource associated with your Auto Scaling groups that are managed by capacity providers\. 
+ You can add a warm pool to your Auto Scaling group\. A warm pool is a group of pre\-initialized Amazon EC2 instances that are ready to be included in the cluster whenever your application needs to scale out\. For more information about warm pools, see [Using a warm pool for your Auto Scaling group](#using-warm-pool)\.
+ The Auto Scaling group can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.
+ If managed termination protection is turned on when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in turned on\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.
+ If managed scaling is turned on when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is turned on, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.

For more information about creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information about creating an Amazon EC2 Auto Scaling group, see [Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Using a warm pool for your Auto Scaling group<a name="using-warm-pool"></a>

Amazon ECS supports Amazon EC2 Auto Scaling warm pools\. A warm pool is a group of pre\-initialized Amazon EC2 instances ready to be placed into service\. Whenever your application needs to scale out, Amazon EC2 Auto Scaling uses the pre\-initialized instances from the warm pool rather than launching cold instances, allows for any final initialization process to run, and then places the instance into service\.

To learn more about warm pools and how to add a warm pool to your Auto Scaling group, see [Warm pools for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-warm-pools.html) *in the Amazon EC2 Auto Scaling User Guide*\.

When you create or update a warm pool for an Auto Scaling group for Amazon ECS , you cannot set the option that returns instances to the warm pool on scale in \(`ReuseOnScaleIn`\)\. For more information, see [put\-warm\-pool](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/put-warm-pool.html) in the *AWS Command Line Interface Reference*\.

To use warm pools with your Amazon ECS cluster, set the `ECS_WARM_POOLS_CHECK` agent configuration variable to `true` in the **User data** field of your Amazon EC2 Auto Scaling group launch template\. The following shows an example of how the agent configuration variable can be specified in the **User data** field of an Amazon EC2 launch template\.

```
#!/bin/bash
cat <<'EOF' >> /etc/ecs/ecs.config
ECS_CLUSTER=MyCluster
ECS_WARM_POOLS_CHECK=true
EOF
```

The `ECS_WARM_POOLS_CHECK` variable is only supported on agent versions `1.59.0` and later\. For more information about the variable, see the [Available Parameters](ecs-agent-config.md#ecs-agent-availparam) page\.
