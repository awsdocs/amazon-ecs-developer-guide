# Amazon ECS Clusters<a name="ECS_clusters"></a>

An Amazon ECS cluster is a logical grouping of tasks or services\. If you are running tasks or services that use the EC2 launch type, a cluster is also a grouping of container instances\. When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate\.

The following are general concepts about Amazon ECS clusters\.
+ Clusters are Region\-specific\.
+ Clusters can contain tasks using both the Fargate and EC2 launch types\. For more information about launch types, see [Amazon ECS Launch Types](launch_types.md)\. 
+ For tasks using the EC2 launch type, clusters can contain multiple different container instance types, but each container instance may only be part of one cluster at a time\.
+ You can create custom IAM policies for your clusters to allow or restrict user access to specific clusters\. For more information, see the [Cluster Examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies) section in [Amazon Elastic Container Service Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

**Topics**
+ [Creating a Cluster](create_cluster.md)
+ [Scaling a Cluster](scale_cluster.md)
+ [Updating Cluster Settings](update-cluster-settings.md)
+ [Deleting a Cluster](delete_cluster.md)