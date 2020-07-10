# Auto Scaling group capacity providers<a name="asg-capacity-providers"></a>

Amazon ECS capacity providers can use Auto Scaling groups to manage the Amazon EC2 instances registered to their clusters\. You can use the managed scaling feature to have Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group or you can manage the scaling actions yourself\. This enables you to effectively use cluster auto scaling\.

## Auto Scaling group capacity providers considerations<a name="asg-capacity-providers-considerations"></a>

The following should be considered when using Auto Scaling group capacity providers\.
+ It is recommended that you create a new Auto Scaling group to use with a capacity provider rather than using an existing one\. If you use an existing Auto Scaling group, any Amazon EC2 instances associated with the group that were already running and registered to an Amazon ECS cluster prior to the Auto Scaling group being used to create a capacity provider may not be properly registered with the capacity provider\. This may cause issues when using the capacity provider in a capacity provider strategy\. The DescribeContainerInstances API can confirm whether a container instance is associated with a capacity provider or not\.
+ An Auto Scaling group must have a `MaxSize` greater than zero to enable it to scale out\.
+ When using managed termination protection, managed scaling must be enabled otherwise managed termination protection will not work\.

## Using managed scaling<a name="asg-capacity-providers-managed-scaling"></a>

When creating a capacity provider, you can optionally enable managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group used when creating the capacity provider\. On your behalf, Amazon ECS creates an AWS Auto Scaling scaling plan with a target tracking scaling policy based on the target capacity value you specify\. Amazon ECS then associates this scaling plan with your Auto Scaling group\. For each of the capacity providers with managed scaling enabled, an Amazon ECS managed CloudWatch metric with the prefix `AWS/ECS/ManagedScaling` is created along with two CloudWatch alarms\. The CloudWatch metrics and alarms are used to monitor the container instance capacity in your Auto Scaling groups and will trigger the Auto Scaling group to scale in and scale out as needed\.

Managed scaling is only supported in Regions that AWS Auto Scaling is available in\. For a list of supported Regions, see [AWS Auto Scaling Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/autoscaling_region.html) in the *Amazon Web Services General Reference*\.

## Creating an Auto Scaling group<a name="asg-capacity-providers-create-auto-scaling-group"></a>

When creating an Auto Scaling group, you use either a launch template or launch configuration\. The launch template or launch configuration specifies the Amazon EC2 instance configuration, including the AMI, the instance type, a key pair, security groups, and the other parameters that you use to launch Amazon EC2 instances\.

If you use the Amazon ECS console **Create Cluster** wizard with the **EC2 Linux \+ Networking** option, then Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group can then be used in a capacity provider for that cluster\.

The following should be considered when creating an Auto Scaling group for a capacity provider\.
+ If managed termination protection is enabled when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in enabled as well\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.
+ If managed scaling is enabled when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.

For more information on creating an Amazon EC2 Auto Scaling launch configuration, see [Launch Configurations](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchConfiguration.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\.

For more information on creating an Amazon EC2 Auto Scaling launch template, see [Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Creating a capacity provider<a name="asg-capacity-providers-create-capacity-provider"></a>

A *capacity provider* is used in association with a cluster to determine the infrastructure that a task runs on\. When creating a capacity provider, you specify the following details:
+ An Auto Scaling group Amazon Resource Name \(ARN\)
+ Whether or not to enable managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.
+ Whether or not to enable managed termination protection\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled\.

### To create a capacity provider \(AWS Management Console\)<a name="create-capacity-provider-console"></a>

Use the following steps to create a new capacity provider for an existing Amazon ECS cluster\.

**To create a capacity provider**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region your cluster exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose **Capacity Providers**, and then choose **Create**\.

1. For **Capacity provider name**, enter a capacity provider name\.

1. For **Auto Scaling group**, select the Auto Scaling group to associate with the capacity provider\. The Auto Scaling group must already be created\. For more information, see [Creating an Auto Scaling group](#asg-capacity-providers-create-auto-scaling-group)\.

1. For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.

1. For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.

1. For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled and if managed scaling is enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.

1. Choose **Create** to complete the capacity provider creation\.
**Important**  
If you receive an error during this step, try logging out and back in to the console\. If the error does not clear, we recommend using the AWS CLI instead\. For more information, see [To create a capacity provider \(AWS CLI\)To delete an Auto Scaling group capacity provider \(AWS CLI\)](#create-capacity-provider-cli)\.

### To create a capacity provider \(AWS CLI\)<a name="create-capacity-provider-cli"></a>

Use the following command to create a new capacity provider\.
+ [create\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-capacity-provider.html) \(AWS CLI\)

  ```
  aws ecs create-capacity-provider \
       --name CapacityProviderName \
       --auto-scaling-group-provider autoScalingGroupArn="AutoScalingGroupARN",managedScaling=\{status='ENABLED|DISABLED',targetCapacity=integer,minimumScalingStepSize=integer,maximumScalingStepSize=integer\},managedTerminationProtection="ENABLED|DISABLED" \
       --region us-east-2
  ```

  If you prefer to use a JSON input file with the `create-capacity-provider` command, use the following command to generate a CLI skeleton\.

  ```
  aws ecs create-capacity-provider --generate-cli-skeleton
  ```

## Creating a cluster with a capacity provider<a name="asg-capacity-providers-create-cluster"></a>

When a new Amazon ECS cluster is created, you can specify one or more capacity providers to associate with the cluster\. The associated capacity providers determine the infrastructure to run your tasks on\.

For AWS Management Console steps, see [Creating a cluster](create_cluster.md)\.

### To create a cluster with a capacity provider \(AWS CLI\)<a name="create-cluster-cli"></a>

Use the following command to create a new capacity provider\.
+ [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\)

  ```
  aws ecs create-cluster \
       --cluster-name ASGCluster \
       --capacity-providers CapacityProviderA CapacityProviderB \
       --default-capacity-provider-strategy capacityProvider=CapacityProviderA,weight=1,base=1 capacityProvider=CapacityProviderB,weight=1 \
       --region us-west-2
  ```

  If you prefer to use a JSON input file with the `create-cluster` command, use the following command to generate a CLI skeleton\.

  ```
  aws ecs create-cluster --generate-cli-skeleton
  ```

## Deleting an Auto Scaling group capacity provider<a name="asg-capacity-providers-delete-capacity-provider"></a>

If you are finished using an Auto Scaling group capacity provider, you can delete it\. Once deleted, the Auto Scaling group capacity provider will transition to the `INACTIVE` state\. Capacity providers with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` capacity providers persisting\.

Prior to an Auto Scaling group capacity provider being deleted, the capacity provider must be removed from the capacity provider strategy from all services\. The UpdateService API or the update service workflow in the AWS Management Console can be used to remove a capacity provider from a service's capacity provider strategy\. The force new deployment option can be used to ensure that any tasks using the Amazon EC2 instance capacity provided by the capacity provider are transitioned to use the capacity from the remaining capacity providers\.

There are other prerequisites that must be performed to delete a capacity provider but they are specific to the tool used and are mentioned in the following steps\.

Use the following steps to delete an Auto Scaling group capacity provider\.

### To delete an Auto Scaling group capacity provider \(AWS Management Console\)<a name="delete-capacity-provider-console"></a>

When deleting a capacity provider using the AWS Management Console, the console goes through two steps\. The capacity provider is first disassociated from the cluster completely and then it is deleted\. In rare cases, the capacity provider may be successfully disassociated from the cluster but is unable to be deleted\. In those cases, you must use either the Amazon ECS API or the AWS CLI to view the status of the capacity provider and delete it\.

**Note**  
Only capacity providers that are currently associated with a cluster are visible in the AWS Management Console\. To delete a capacity provider that is not associated with a cluster, you must use the Amazon ECS API, SDK, or AWS CLI\.

**To delete an Auto Scaling group capacity provider using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region your cluster exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose the **Capacity Providers** tab\.

1. Select the capacity provider you want to delete and then choose **Delete**\.

### To delete an Auto Scaling group capacity provider \(AWS CLI\)<a name="delete-capacity-provider-cli"></a>

When using the AWS CLI to delete a capacity provider, the capacity provider must first be disassociated from the cluster\. The following options are available to disassociate a capacity provider from a cluster\.

Option 1: Use the delete command to to delete the cluster\. This will disassociate the capacity provider from the cluster upon successful deletion of the cluster\.
+ [delete\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-cluster.html) \(AWS CLI\)

  ```
  aws ecs delete-cluster \
       --cluster MyCluster
  ```

Option 2: Use the put\-cluster\-capacity\-providers command to disassociate a capacity provider from a cluster\. If you have other capacity providers associated with the cluster that you want to have remain associated with the cluster, you must include those when using the command\.

The following example will remove all existing capacity providers from the specified cluster\.
+ [put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html) \(AWS CLI\)

  ```
  aws ecs put-cluster-capacity-providers \
       --cluster MyCluster \
       --capacity-providers [] \
       --default-capacity-provider-strategy []
  ```

Use the delete\-capacity\-provider command to delete a capacity provider\. You can specify the capacity provider using its short name or the full Amazon Resource Name \(ARN\)\.
+ [delete\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-capacity-provider.html) \(AWS CLI\)

  Example using the short name:

  ```
  aws ecs delete-capacity-provider \
       --capacity-provider ExampleCapacityProvider
  ```

  Example using the full ARN:

  ```
  aws ecs delete-capacity-provider \
       --capacity-provider arn:aws:ecs:us-west-2:123456789012:capacity-provider/ExampleCapacityProvider
  ```