# Deleting a Cluster<a name="delete_cluster"></a>

If you are finished using a cluster, you can delete it\. When you delete a cluster in the Amazon ECS console, the associated resources that are deleted with it vary depending on how the cluster was created\. [Step 5](#step-delete-cluster) of the following procedure changes based on that condition\.

If your cluster was created with the console first\-run experience described in [Getting Started with Amazon ECS](ECS_GetStarted.md) after November 24, 2015, or the cluster creation wizard described in [Creating a Cluster](create_cluster.md), then the AWS CloudFormation stack that was created for your cluster is also deleted when you delete your cluster\. 

If your cluster was created manually \(without the cluster creation wizard\) or with the console first run experience before November 24, 2015, then you must deregister \(or terminate\) any container instances associated with the cluster before you can delete it\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\. In this case, after the cluster is deleted, you should delete any remaining AWS CloudFormation stack resources or Auto Scaling groups associated with the cluster to avoid incurring any future charges for those resources\. For more information, see [Delete the AWS CloudFormation Stack](ECS_CleaningUp.md#cleanup-CFN-stack)\.

**To delete a cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the cluster to delete\.
**Note**  
If your cluster has registered container instances, you must deregister or terminate them\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\.

1. <a name="step-delete-cluster"></a>Choose **Delete Cluster**\. You see one of two confirmation prompts:
   + **Deleting the cluster also deletes the CloudFormation stack EC2ContainerService\-*cluster\_name*: ** Deleting this cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.
   + **Deleting the cluster does not affect CloudFormation resources\.\.\.:** Deleting this cluster does not clean up any resources that are associated with the cluster, including Auto Scaling groups, VPCs, or load balancers\. Also, any container instances that are registered with this cluster must be deregistered or terminated before you can delete the cluster\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\. You can visit the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/) to update or delete any of these resources\. For more information, see [Delete the AWS CloudFormation Stack](ECS_CleaningUp.md#cleanup-CFN-stack)\.