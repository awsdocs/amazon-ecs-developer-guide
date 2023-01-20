# Deleting a capacity provider using the console<a name="delete-capacity-provider-console-v2"></a>

If you are finished using an Auto Scaling group capacity provider, you can delete it\. After the group is deleted, the Auto Scaling group capacity provider will transition to the `INACTIVE` state\. Capacity providers with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` capacity providers persisting\. Before the Auto Scaling group capacity provider is deleted, the capacity provider must be removed from the capacity provider strategy from all services\. You can use the `UpdateService` API or the update service workflow in the Amazon ECS console to remove a capacity provider from a service's capacity provider strategy\. Use the he force new deployment option to ensure that any tasks using the Amazon EC2 instance capacity provided by the capacity provider are transitioned to use the capacity from the remaining capacity providers\.

**To create a capacity provider for the cluster \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose **Infrastructure**, the Auto Scaling group, and then choose **Delete**\.

1. In the confirmation box, enter **delete *Auto Scaling group name***

1. Choose **Delete**\.