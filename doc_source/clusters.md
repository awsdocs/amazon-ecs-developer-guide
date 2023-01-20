# Amazon ECS clusters<a name="clusters"></a>

An Amazon ECS cluster groups together tasks, and services, and allows for shared capacity and common configurations\. An Amazon ECS cluster is a logical grouping of tasks or services\. Your tasks and services are run on infrastructure that is registered to a cluster\. The infrastructure capacity can be provided by AWS Fargate, which is serverless infrastructure that AWS manages, Amazon EC2 instances that you manage, or an on\-premise server or virtual machine \(VM\) that you manage remotely\. In most cases, Amazon ECS capacity providers can be used to manage the infrastructure that the tasks in your clusters use\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.

When you first use Amazon ECS, a default cluster is created for you, but you can create multiple clusters in an account to keep your resources separate\.

**Topics**
+ [Cluster concepts](#clusters-concepts)
+ [Cluster management in the Amazon ECS console](available-cluster-actions.md)
+ [Amazon ECS capacity providers](cluster-capacity-providers.md)
+ [Amazon ECS cluster Auto Scaling](cluster-auto-scaling.md)
+ [Amazon ECS clusters in Local Zones, Wavelength Zones, and AWS Outposts](cluster-regions-zones.md)
+ [Cluster and capacity provider management using the AWS CLI](cluster-management-cli.md)

## Cluster concepts<a name="clusters-concepts"></a>

The following are general concepts about Amazon ECS clusters\.
+ Clusters are AWS Region specific\.
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
The cluster has been deleted\. Clusters with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so make sure you do not rely on `INACTIVE` clusters persisting\.
+ A cluster can contain a mix of tasks that are hosted on AWS Fargate, Amazon EC2 instances, or external instances\. Tasks can run on Fargate or EC2 infrastructure as a launch type or a capacity provider strategy\. If you use EC2 as a launch type, ECS doesn't track and scale the capacity of Amazon EC2 Auto Scaling groups\. For more information about launch types, see [Amazon ECS launch types](launch_types.md)\.
+ A cluster can contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers\. However, when you specify a capacity provider strategy, they may only contain one or the other but not both\. For more information, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.
+ For tasks that use the EC2 launch type or Auto Scaling group capacity providers, clusters can contain multiple different container instance types\. However, each container instance can only be registered to one cluster at a time\.
+ Custom IAM policies may be created to allow or restrict user access to specific clusters\. For more information, see the [Cluster examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies) section in [Identity\-based policy examples for Amazon Elastic Container Service](security_iam_id-based-policy-examples.md)\.
+  \*You can configure a default Service Connect namespace for a cluster\. After you set a default Service Connect namespace, any new services created in the cluster can be added as client services in the namespace by turning on Service Connect\. No additional configuration is required\. For more information, see [Service Connect ](service-connect.md)\*\.