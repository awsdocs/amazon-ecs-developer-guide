# Deleting a cluster using the classic console<a name="delete_cluster"></a>


|  | 
| --- |
| The new experience is now the default in the Amazon ECS console\. For more information, see [Deleting a cluster using the console](delete_cluster-new-console.md)\. | 

If you are finished using a cluster, you can delete it\. After you delete the cluster, it transitions to the `INACTIVE` state\. Clusters with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` clusters persisting\.

When you delete a cluster in the Amazon ECS console, the associated resources that are deleted with it vary depending on how the cluster was created\. This condition is discussed in step 5 of the following procedure\.

If your cluster was created with the AWS Management Console then the AWS CloudFormation stack that was created for your cluster is also deleted when you delete your cluster\. If you have added or modified the underlying cluster resources you may receive an error when attempting to delete the cluster\. AWS CloudFormation refers to this as *stack drift*\. For more information on detecting drift on an existing AWS CloudFormation stack, see [Detect drift on an entire AWS CloudFormation stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift-stack.html) in the *AWS CloudFormation User Guide*\.

**To delete a cluster using the classic console**

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.
**Note**  
If your cluster has registered container instances, you must deregister or terminate them\. For more information, see [Deregister an Amazon EC2 backed container instance](deregister_container_instance.md)\.

1. In the upper\-right of the page, choose **Delete Cluster**\. You see one of two confirmation prompts:
   + **Deleting the cluster also deletes the AWS CloudFormation stack EC2ContainerService\-*cluster\_name* – ** Deleting this cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.
   + **Deleting the cluster does not affect AWS CloudFormation resources** – Deleting this cluster does not clean up any resources that are associated with the cluster, including Auto Scaling groups, VPCs, or load balancers\. Also, any container instances that are registered with this cluster must be deregistered or terminated before you can delete the cluster\. For more information, see [Deregister an Amazon EC2 backed container instance](deregister_container_instance.md)\. You can visit the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/) to update or delete any of these resources\.

1. In the confirmation box, enter **delete me**\.