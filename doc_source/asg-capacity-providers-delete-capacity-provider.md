# Deleting an Auto Scaling group capacity provider<a name="asg-capacity-providers-delete-capacity-provider"></a>

If you are finished using an Auto Scaling group capacity provider, you can delete it\. Once deleted, the Auto Scaling group capacity provider will transition to the `INACTIVE` state\. Capacity providers with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` capacity providers persisting\.

Prior to an Auto Scaling group capacity provider being deleted, the capacity provider must be removed from the capacity provider strategy from all services\. The UpdateService API or the update service workflow in the AWS Management Console can be used to remove a capacity provider from a service's capacity provider strategy\. The force new deployment option can be used to ensure that any tasks using the Amazon EC2 instance capacity provided by the capacity provider are transitioned to use the capacity from the remaining capacity providers\.

There are other prerequisites that must be performed to delete a capacity provider but they are specific to the tool used and are mentioned in the following steps\.

Use the following steps to delete an Auto Scaling group capacity provider\.

## To delete an Auto Scaling group capacity provider \(AWS Management Console\)<a name="delete-capacity-provider-console"></a>

When deleting a capacity provider using the AWS Management Console, the console goes through two steps\. The capacity provider is first disassociated from the cluster completely and then it is deleted\. In rare cases, the capacity provider may be successfully disassociated from the cluster but is unable to be deleted\. In those cases, you must use either the Amazon ECS API or the AWS CLI to view the status of the capacity provider and delete it\.

**Note**  
Only capacity providers that are currently associated with a cluster are visible in the AWS Management Console\. To delete a capacity provider that is not associated with a cluster, you must use the Amazon ECS API, SDK, or AWS CLI\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region your cluster exists in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your cluster\.

1. On the **Cluster : *name*** page, choose the **Capacity Providers** tab\.

1. Select the capacity provider you want to delete and then choose **Delete**\.

## To delete an Auto Scaling group capacity provider \(AWS CLI\)<a name="delete-capacity-provider-cli"></a>

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