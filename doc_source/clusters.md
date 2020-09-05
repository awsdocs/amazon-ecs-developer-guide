# Amazon ECS clusters<a name="clusters"></a>

An Amazon ECS cluster is a logical grouping of tasks or services\. If you are running tasks or services that use the EC2 launch type, a cluster is also a grouping of container instances\. If you are using capacity providers, a cluster is also a logical grouping of capacity providers\. When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate\.

**Topics**
+ [Cluster concepts](#clusters-concepts)
+ [Creating a cluster](create_cluster.md)
+ [Amazon ECS capacity providers](cluster-capacity-providers.md)
+ [Amazon ECS cluster auto scaling](cluster-auto-scaling.md)
+ [Amazon ECS clusters in Local Zones, Wavelength Zones, and AWS Outposts](cluster-regions-zones.md)
+ [Updating cluster settings](update-cluster-settings.md)
+ [Deleting a cluster](delete_cluster.md)

## Cluster concepts<a name="clusters-concepts"></a>

The following are general concepts about Amazon ECS clusters\.
+ Clusters are Region\-specific\.
+ The following are the possible states that a cluster can be in\.  
ACTIVE  
The cluster is ready to accept tasks and, if applicable, you can register container instances with the cluster\.  
PROVISIONING  
The cluster has capacity providers associated with it and the resources needed for the capacity provider are being created\.  
DEPROVISIONING  
The cluster has capacity providers associated with it and the resources needed for the capacity provider are being deleted\.  
FAILED  
The cluster has capacity providers associated with it and the resources needed for the capacity provider have failed to create\.  
INACTIVE  
The cluster has been deleted\. Clusters with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` clusters persisting\.
+ A cluster may contain a mix of tasks using either the Fargate or EC2 launch types\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.
+ A cluster may contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers, however when specifying a capacity provider strategy they may only contain one or the other but not both\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.
+ For tasks using the EC2 launch type, clusters can contain multiple different container instance types, but each container instance may only be registered to one cluster at a time\.
+ Custom IAM policies may be created to allow or restrict user access to specific clusters\. For more information, see the [Cluster Examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies) section in [Amazon Elastic Container Service Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.