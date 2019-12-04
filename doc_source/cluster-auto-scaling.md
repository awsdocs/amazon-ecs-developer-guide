# Amazon ECS Cluster Auto Scaling<a name="cluster-auto-scaling"></a>

Amazon ECS cluster auto scaling enables you to have more control over how you scale tasks within a cluster\. Each cluster has one or more capacity providers and an optional default capacity provider strategy\. The capacity providers determine the infrastructure to use for the tasks, and the capacity provider strategy determines how the tasks are spread across the capacity providers\. When you run a task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\. For more information about cluster capacity providers, see [Amazon ECS Cluster Capacity Providers](cluster-capacity-providers.md)\.

**Topics**
+ [Cluster Auto Scaling Considerations](#cluster-auto-scaling-considerations)
+ [Auto Scaling Group Capacity Providers](#asg-capacity-providers)
+ [Tutorial: Using Cluster Auto Scaling with the AWS Management Console](tutorial-cluster-auto-scaling-console.md)
+ [Tutorial: Using Cluster Auto Scaling with the AWS CLI](tutorial-cluster-auto-scaling-cli.md)

## Cluster Auto Scaling Considerations<a name="cluster-auto-scaling-considerations"></a>

The following should be considered when using cluster auto scaling:
+ The Amazon ECS service\-linked IAM role is required to use cluster auto scaling\. For more information, see [Service\-Linked Roles for Amazon ECS](using-service-linked-roles.md)\.
+ When using capacity providers with Auto Scaling groups, the `autoscaling:CreateOrUpdateTags` permission is needed\. This is because Amazon ECS adds a tag to the Auto Scaling group when it associates it with the capacity provider\.

## Auto Scaling Group Capacity Providers<a name="asg-capacity-providers"></a>

Amazon ECS capacity providers use Auto Scaling groups to manage the Amazon EC2 instances registered to their clusters\.

**Topics**
+ [Auto Scaling Group Capacity Providers Considerations](#asg-capacity-providers-considerations)
+ [Using Managed Scaling](#asg-capacity-providers-managed-scaling)
+ [Creating an Auto Scaling Group](#asg-capacity-providers-create-auto-scaling-group)
+ [Creating a Capacity Provider](#asg-capacity-providers-create-capacity-provider)
+ [Creating a Cluster](#asg-capacity-providers-create-cluster)

### Auto Scaling Group Capacity Providers Considerations<a name="asg-capacity-providers-considerations"></a>

The following should be considered when using Auto Scaling group capacity providers\.
+ It is recommended that you create a new Auto Scaling group to use with a capacity provider rather than using an existing one\. If you use an existing Auto Scaling group, any Amazon EC2 instances associated with the group that were already running and registered to an Amazon ECS cluster prior to the Auto Scaling group being used to create a capacity provider may not be properly registered with the capacity provider\. This may cause issues when using the capacity provider in a capacity provider strategy\. The DescribeContainerInstances API can confirm that a container instance is associated with a capacity provider\.
+ An Auto Scaling group must have a `MaxSize` greater than zero to scale out\.
+ Managed scaling is only supported in Regions that AWS Auto Scaling is available in\. For a list of supported Regions, see [AWS Auto Scaling Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#autoscaling_region) in the *Amazon Web Services General Reference*\.

### Using Managed Scaling<a name="asg-capacity-providers-managed-scaling"></a>

When creating a capacity provider, you can optionally enable managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\. On your behalf, Amazon ECS creates an AWS Auto Scaling scaling plan with a target tracking scaling policy based on the target capacity value you specify\. Amazon ECS then associates this scaling plan with your Auto Scaling group\. For each of the capacity providers with managed scaling enabled, an Amazon ECS managed CloudWatch metric with the prefix `AWS/ECS/ManagedScaling` is created along with two CloudWatch alarms\. The CloudWatch metrics and alarms are used to monitor the container instance capacity in your Auto Scaling groups and will trigger the Auto Scaling group to scale in and scale out as needed\.

Managed scaling is only supported in Regions that AWS Auto Scaling is available in\. For a list of supported Regions, see [AWS Auto Scaling Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#autoscaling_region) in the *Amazon Web Services General Reference*\.

### Creating an Auto Scaling Group<a name="asg-capacity-providers-create-auto-scaling-group"></a>

When creating an Auto Scaling group, you use either a launch template or launch configuration\. The launch template or launch configuration specifies the Amazon EC2 instance configuration, including the AMI, the instance type, a key pair, security groups, and the other parameters that you use to launch Amazon EC2 instances\.

If you use the Amazon ECS console **Create Cluster** wizard with the **EC2 Linux \+ Networking** option, then Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group can then be used in a capacity provider for that cluster\.

The following should be considered when creating an Auto Scaling group for a capacity provider\.
+ If managed termination protection is enabled when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in enabled as well\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.
+ If managed scaling is enabled when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.

For more information on creating an Amazon EC2 Auto Scaling launch configuration, see [Launch Configurations](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchConfiguration.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.

### Creating a Capacity Provider<a name="asg-capacity-providers-create-capacity-provider"></a>

A *capacity provider* is used in association with a cluster to determine the infrastructure that a task runs on\. When creating a capacity provider, you specify the following details:
+ An Auto Scaling group Amazon Resource Name \(ARN\)
+ Whether or not to enable managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.
+ Whether or not to enable managed termination protection\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled\.

#### To create a capacity provider \(AWS Management Console\)<a name="create-capacity-provider-console"></a>

Use the following steps to create a new capacity provider for an existing Amazon ECS cluster\.

**To create a capacity provider**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region your cluster exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose **Capacity Providers**, and then choose **Create**\.

1. For **Capacity provider name**, enter a capacity provider name\.

1. For **Auto Scaling group**, select the Auto Scaling group to associate with the capacity provider\. The Auto Scaling group must already be created\. For more information, see [Creating an Auto Scaling Group](#asg-capacity-providers-create-auto-scaling-group)\.

1. For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.

1. For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.

1. For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.

1. Choose **Create** to complete the capacity provider creation\.

#### To create a capacity provider \(AWS CLI\)<a name="create-capacity-provider-cli"></a>

Use the following command to create a new capacity provider\.
+ [create\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-capacity-provider.html) \(AWS CLI\)

  ```
  aws ecs create-capacity-provider --name CapacityProviderName --auto-scaling-group-provider autoScalingGroupArn="AutoScalingGroupARN",managedScaling={status='ENABLED|DISABLED',targetCapacity=integer,minimumScalingStepSize=integer,maximumScalingStepSize=integer},managedTerminationProtection="ENABLED|DISABLED" --region us-east-2
  ```

  If you prefer to use a JSON input file with the `create-capacity-provider` command, use the following command to generate a CLI skeleton\.

  ```
  aws ecs create-capacity-provider --generate-cli-skeleton
  ```

### Creating a Cluster<a name="asg-capacity-providers-create-cluster"></a>

When a new Amazon ECS cluster is created, you specify one or more capacity providers to associate with the cluster\. The associated capacity providers determine the infrastructure to run your tasks on\.

For AWS Management Console steps, see [Creating a Cluster](create_cluster.md)\.

#### To create a cluster with a capacity provider \(AWS CLI\)<a name="create-cluster-cli"></a>

Use the following command to create a new capacity provider\.
+ [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\)

  ```
  aws ecs create-cluster --cluster-name ASGCluster --capacity-providers CapacityProviderA CapacityProviderB --default-capacity-provider-strategy capacityProvider=CapacityProviderA,weight=1,base=1 capacityProvider=CapacityProviderB,weight=1 --region us-west-2
  ```

  If you prefer to use a JSON input file with the `create-cluster` command, use the following command to generate a CLI skeleton\.

  ```
  aws ecs create-cluster --generate-cli-skeleton
  ```