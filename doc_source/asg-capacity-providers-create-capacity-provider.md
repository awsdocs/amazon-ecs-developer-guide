# Creating an Auto Scaling group capacity provider using the classic console<a name="asg-capacity-providers-create-capacity-provider"></a>

A *capacity provider* is used in association with a cluster to determine the infrastructure that a task runs on\. When creating a capacity provider, you specify the following details:
+ An Auto Scaling group Amazon Resource Name \(ARN\)
+ Whether or not to turn on managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling policies\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.
+ Whether or not to turn on managed termination protection\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled\.

Use the following steps to create a new capacity provider for an existing Amazon ECS cluster\.

**To create an Auto Scaling group capacity provider \(classic AWS Management Console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region your cluster exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose **Capacity Providers**, and then choose **Create**\.

1. For **Capacity provider name**, enter a capacity provider name\.

1. For **Auto Scaling group**, select the Auto Scaling group to associate with the capacity provider\. The Auto Scaling group must already be created\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\.

1. For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling policies\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.

1. For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.

1. For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled and if managed scaling is enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.

1. Choose **Create** to complete the capacity provider creation\.

**To create an Auto Scaling group capacity provider \(AWS CLI\)**
+ Use the following command to create a new capacity provider\.
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