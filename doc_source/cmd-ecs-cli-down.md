# ecs\-cli down<a name="cmd-ecs-cli-down"></a>

Deletes the AWS CloudFormation stack that was created by ecs\-cli up and the associated resources\.

**Note**  
The Amazon ECS CLI can only manage tasks, services, and container instances that were created with the Amazon ECS CLI\. To manage tasks, services, and container instances that weren't created by the Amazon ECS CLI, use the AWS Command Line Interface or the AWS Management Console\.

The ecs\-cli down command attempts to delete the cluster specified in `~/.ecs/config`\. However, if there are any active services \(even with a desired count of 0\) or registered container instances in your cluster that were not created by ecs\-cli up, the cluster is not deleted and the services and pre\-existing container instances remain active\. This might happen, for example, if you used an existing ECS cluster with registered container instances, such as the default cluster\.

If you have remaining services or container instances in your cluster that you would like to remove, you can follow the procedures in [ Deleting a Cluster  If you are finished using a cluster, you can delete it\. When you delete a cluster in the Amazon ECS console, the associated resources that are deleted with it vary depending on how the cluster was created\. [Step 5](#step-delete-cluster) of the following procedure changes based on that condition\. If your cluster was created with the console first\-run experience described in [Getting Started with Amazon ECS](ECS_GetStarted.md) after November 24, 2015, or the cluster creation wizard described in [Creating a Cluster](create_cluster.md), then the AWS CloudFormation stack that was created for your cluster is also deleted when you delete your cluster\.  If your cluster was created manually \(without the cluster creation wizard\) or with the console first\-run experience before November 24, 2015, then you must deregister \(or terminate\) any container instances associated with the cluster before you can delete it\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\. In this case, after the cluster is deleted, you should delete any remaining AWS CloudFormation stack resources or Auto Scaling groups associated with the cluster to avoid incurring any future charges for those resources\. You can visit the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/) to update or delete any of these resources\. To delete a cluster Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  From the navigation bar, select the Region to use\.   In the navigation pane, choose **Clusters**\.   On the **Clusters** page, select the cluster to delete\.  If your cluster has registered container instances, you must deregister or terminate them\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\.    Choose **Delete Cluster**\. You see one of two confirmation prompts:   **Deleting the cluster also deletes the CloudFormation stack EC2ContainerService\-*cluster\_name* – ** Deleting this cluster cleans up the associated resources that were created with the cluster, including Auto Scaling groups, VPCs, or load balancers\.   **Deleting the cluster does not affect CloudFormation resources** – Deleting this cluster does not clean up any resources that are associated with the cluster, including Auto Scaling groups, VPCs, or load balancers\. Also, any container instances that are registered with this cluster must be deregistered or terminated before you can delete the cluster\. For more information, see [Deregister a Container Instance](deregister_container_instance.md)\. You can visit the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation/](https://console.aws.amazon.com/cloudformation/) to update or delete any of these resources\.     ](delete_cluster.md) to delete your cluster\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-down-syntax"></a>

**ecs\-cli down \[\-\-force\] \[\-\-cluster *cluster\_name*\] \[\-\-region *region*\] \[\-\-help\]** 

## Options<a name="cmd-ecs-cli-down-options"></a>


| Name | Description | 
| --- | --- | 
|  `--force, -f`  |  Acknowledges that this command permanently deletes resources and bypasses the confirmation prompt\. Required: No  | 
|  `--region, -r region`  |  Specifies the AWS Region to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--cluster-config cluster_config_name`  |  Specifies the name of the Amazon ECS cluster configuration to use\. Defaults to the cluster configuration set as the default\. Type: String Required: No  | 
|  `--ecs-profile ecs_profile`  |  Specifies the name of the Amazon ECS profile configuration to use\. Defaults to the profile configured using the configure profile command\. Type: String Required: No  | 
|  `--aws-profile aws_profile`  |  Specifies the AWS profile to use\. Enables you to use the AWS credentials from an existing named profile in `~/.aws/credentials`\. Type: String Required: No  | 
|  `--cluster, -c cluster_name`  |  Specifies the Amazon ECS cluster name to use\. Defaults to the cluster configured using the configure command\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-down-examples"></a>

### Example 1<a name="cmd-ecs-cli-down-example-1"></a>

This example deletes a cluster that contains resources\.

```
ecs-cli down --cluster ecs-cli-fargate-demo --force
```

Output:

```
INFO[0001] Waiting for your cluster resources to be deleted
INFO[0001] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0062] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0123] Cloudformation stack status                   stackStatus=DELETE_IN_PROGRESS
INFO[0154] Deleted cluster
```

### Example 2<a name="cmd-ecs-cli-down-example-2"></a>

This example deletes an empty cluster\.

```
ecs-cli down --cluster ecs-cli-empty-demo --force
```

Output:

```
INFO[0002] No CloudFormation stack found for cluster 'ecs-cli-empty-demo'. 
INFO[0003] Deleted cluster                               cluster=ecs-cli-empty-demo
```