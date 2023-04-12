# Updating an Auto Scaling group capacity provider using the classic console<a name="asg-capacity-providers-update-capacity-provider"></a>


|  | 
| --- |
| The new experience is now the default in the Amazon ECS console\. For more information, see [Updating a capacity provider using the console](update-capacity-provider-console-v2.md)\. | 

A capacity provider can be updated to change its managed scaling and managed termination protection settings\. Use the following steps to update an existing capacity provider\.

**To update an Auto Scaling group capacity provider \(classic AWS Management Console\)**

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region the cluster the capacity provider is associated with exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose the **Capacity Providers** tab\.

1. Select the capacity provider to update and choose **Update**\.

1. On the **Update Capacity Provider** page, the following parameters can be updated\.

   1. For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling policies\. When managed scaling is turned off, you manage your Auto Scaling groups yourself\.

   1. For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.

   1. For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled and if managed scaling is enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.

1. Choose **Update** to request capacity provider update\.

1. To verify whether the capacity provider update was successful, check the **Update Status** column on the **Capacity Providers** tab\.

**To update an Auto Scaling group capacity provider \(AWS CLI\)**
+ Use the following command to create a new capacity provider\.
  + [update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html) \(AWS CLI\)

    ```
    aws ecs update-capacity-provider \
         --name CapacityProviderName \
         --auto-scaling-group-provider managedScaling=\{status='ENABLED|DISABLED',targetCapacity=integer,minimumScalingStepSize=integer,maximumScalingStepSize=integer\},managedTerminationProtection="ENABLED|DISABLED" \
         --region us-east-2
    ```

    If you prefer to use a JSON input file with the `create-capacity-provider` command, use the following command to generate a CLI skeleton\.

    ```
    aws ecs update-capacity-provider --generate-cli-skeleton
    ```