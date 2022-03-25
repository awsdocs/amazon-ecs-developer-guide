# Deleting a cluster using the new console<a name="delete_cluster-new-console"></a>

If you are finished using a cluster, you can delete it\. After you delete the cluster, it transitions to the `INACTIVE` state\. Clusters with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` clusters persisting\.

Before you delete a cluster, you must perform the following operations:
+ Delete all services in the cluster\. For more information, see [Deleting a service](delete-service.md)\.
+ Stop all currently running tasks\. For more information, see [Stopping tasks using the new console](stop-task-console-v2.md)\.
+ Deregister all registered container instances in the cluster\. For more information, see [Deregister an Amazon EC2 backed container instance](deregister_container_instance.md)\.
+ If you created your cluster with the new console, delete the AWS CloudFormation stack that was created for your cluster\. The stack is named ***cluster\-name*\-ECS\-Infra**\. For example, if the cluster name is "example\-cluster\-new\-console", then the stack name is `example-cluster-new-console-ECS-Infra`\. For more information, see [Deleting a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) in the *AWS CloudFormation User Guide*\.

If you created a cluster using the classic console and you receive an error when you use the new console, you might need to delete the cluster using the classic console, For more information, see [Deleting a cluster using the classic console](delete_cluster.md)\.

**To delete a cluster using the new console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.

1. In the upper\-right of the page, choose **Delete Cluster**\. 

   A message is displayed when you did not delete all the resource associated with the cluster\.

1. In the confirmation box, enter **delete *cluster name***\.