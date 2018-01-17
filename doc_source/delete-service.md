# Deleting a Service<a name="delete-service"></a>

You can delete a service if you have no running tasks in it and the desired task count is zero\. If the service is actively maintaining tasks, you cannot delete it, and you must update the service to a desired task count of zero\. For more information, see [Updating a Service](update-service.md)\.

**Note**  
When you delete a service, if there are still running tasks that require cleanup, the service status moves from `ACTIVE` to `DRAINING`, and the service is no longer visible in the console or in `ListServices` API operations\. After the tasks have stopped, then the service status moves from `DRAINING` to `INACTIVE`\. Services in the `DRAINING` or `INACTIVE` status can still be viewed with `DescribeServices` API operations\. However, in the future, `INACTIVE` services may be cleaned up and purged from Amazon ECS record keeping, and `DescribeServices` API operations on those services return a `ServiceNotFoundException` error\.

Use the following procedure to delete an empty service\.

**To delete an empty service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster in which your service resides\.

1. On the **Cluster : *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Delete**\.
**Note**  
Your service must have zero desired or running tasks before it can be deleted\.

1. Choose **Yes, Delete** to confirm your service deletion\.