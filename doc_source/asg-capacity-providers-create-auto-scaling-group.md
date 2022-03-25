# Creating an Auto Scaling group<a name="asg-capacity-providers-create-auto-scaling-group"></a>

When creating an Auto Scaling group, you use an Amazon EC2 launch template\. Amazon EC2 launch templates specify the Amazon EC2 instance configuration, including the AMI, the instance type, a key pair, security groups, and the other parameters that you use to launch Amazon EC2 instances\.

**Note**  
When using the Amazon ECS console **Create Cluster** wizard with the **EC2 Linux \+ Networking** option, Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group could then be used in a capacity provider for that cluster\.  
For more information on replacing an Auto Scaling launch configuration with an Amazon EC2 launch template, see [Replacing a launch configuration with a launch template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/replace-launch-config.html) in the *Amazon EC2 Auto Scaling User Guide*

The following should be considered when creating an Auto Scaling group for a capacity provider\.
+ If managed termination protection is enabled when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in enabled as well\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.
+ If managed scaling is enabled when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.
+ The Auto Scaling group can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling group, see [Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Using a warm pool for your Auto Scaling group<a name="using-warm-pool"></a>

Amazon ECS supports Amazon EC2 Auto Scaling warm pools\. A warm pool is a group of pre\-initialized Amazon EC2 instances ready to be placed into service\. Whenever your application needs to scale out, Amazon EC2 Auto Scaling uses the pre\-initialized instances from the warm pool rather than launching cold instances, allows for any final initialization process to run, and then places the instance into service\.

To learn more about warm pools and how to add a warm pool to your Auto Scaling group, see [Warm pools for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-warm-pools.html) *in the Amazon EC2 Auto Scaling User Guide*\.

To use warm pools with your Amazon ECS cluster, set the `ECS_WARM_POOLS_CHECK` agent configuration variable to `true` in the **User data** field of your Amazon EC2 Auto Scaling group launch template\. The following shows an example of how the agent configuration variable can be specified in the **User data** field of an Amazon EC2 launch template\.

```
#!/bin/bash
cat <<'EOF' >> /etc/ecs/ecs.config
ECS_CLUSTER=MyCluster
ECS_WARM_POOLS_CHECK=true
EOF
```

The `ECS_WARM_POOLS_CHECK` variable is only supported on agent versions `1.59.0` and later\. For more information about the variable, see the [Available Parameters](ecs-agent-config.md#ecs-agent-availparam) page\.