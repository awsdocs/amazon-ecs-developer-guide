# Creating an Auto Scaling group<a name="asg-capacity-providers-create-auto-scaling-group"></a>

When creating an Auto Scaling group, you use an Amazon EC2 launch template\. Amazon EC2 launch templates specify the Amazon EC2 instance configuration, including the AMI, the instance type, a key pair, security groups, and the other parameters that you use to launch Amazon EC2 instances\.

**Note**  
When using the Amazon ECS console **Create Cluster** wizard with the **EC2 Linux \+ Networking** option, Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group could then be used in a capacity provider for that cluster\.  
For more information on replacing an Auto Scaling launch configuration with an Amazon EC2 launch template, see [Replacing a launch configuration with a launch template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/replace-launch-config.html) in the *Amazon EC2 Auto Scaling User Guide*

The following should be considered when creating an Auto Scaling group for a capacity provider\.
+ If managed termination protection is enabled when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in enabled as well\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.
+ If managed scaling is enabled when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling group, see [Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.