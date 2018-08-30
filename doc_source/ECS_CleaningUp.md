# Cleaning Up your Amazon ECS Resources<a name="ECS_CleaningUp"></a>

When you are finished experimenting with or using a particular Amazon ECS cluster, you should clean up the resources associated with it to avoid incurring charges for resources that you are not using\.

Some Amazon ECS resources, such as tasks, services, clusters, and container instances, are cleaned up using the Amazon ECS console\. Other resources, such as Amazon EC2 instances, Elastic Load Balancing load balancers, and Auto Scaling groups, must be cleaned up manually in the Amazon EC2 console or by deleting the AWS CloudFormation stack that created them\.

**Topics**
+ [Scale Down Services](#cleanup-scale-down-services)
+ [Delete Services](#cleanup-delete-services)
+ [Deregister Container Instances](#cleanup-deregister-instances)
+ [Delete a Cluster](#cleanup-delete-cluster)
+ [Delete the AWS CloudFormation Stack](#cleanup-CFN-stack)

## Scale Down Services<a name="cleanup-scale-down-services"></a>

If your cluster contains any services, you should first scale down the desired count of tasks in these services to 0 so that Amazon ECS does not try to start new tasks on your container instances while you are cleaning up\. Follow the procedure in [Updating a Service](update-service.md) and enter 0 in the **Number of tasks** field\.

Alternatively, you can use the following AWS CLI command to scale down your service\. Be sure to substitute the region name, cluster name, and service name for each service that you are scaling down\.

```
aws ecs update-service --cluster default --service service_name --desired-count 0 --region us-west-2
```

## Delete Services<a name="cleanup-delete-services"></a>

Before you can delete a cluster, you must delete the services inside that cluster\. After your service has scaled down to 0 tasks, you can delete it\. For each service inside your cluster, follow the procedures in [Deleting a Service](delete-service.md) to delete it\.

Alternatively, you can use the following AWS CLI command to delete your services\. Be sure to substitute the region name, cluster name, and service name for each service that you are deleting\.

```
aws ecs delete-service --cluster default --service service_name --region us-west-2
```

## Deregister Container Instances<a name="cleanup-deregister-instances"></a>

Before you can delete a cluster, you must deregister the container instances inside that cluster\. For each container instance inside your cluster, follow the procedures in [Deregister a Container Instance](deregister_container_instance.md) to deregister it\.

Alternatively, you can use the following AWS CLI command to deregister your container instances\. Be sure to substitute the region name, cluster name, and container instance ID for each container instance that you are deregistering\.

```
aws ecs deregister-container-instance --cluster default --container-instance container_instance_id --region us-west-2 --force
```

## Delete a Cluster<a name="cleanup-delete-cluster"></a>

After you have removed the active resources from your Amazon ECS cluster, you can delete it\. Use the following procedure to delete your cluster\.

**To delete a cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the region that your cluster is in\.

1. In the navigation pane, select **Clusters**\.

1. On the **Clusters** page, click the **x** in the upper\-right\-hand corner of the cluster you want to delete\.  
![\[ECS cluster\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_cluster.png)

1. Choose **Yes, Delete** to delete the cluster\.

Alternatively, you can use the following AWS CLI command to delete your cluster\. Be sure to substitute the region name and cluster name for each cluster that you are deleting\.

```
aws ecs delete-cluster --cluster default --region us-west-2
```

## Delete the AWS CloudFormation Stack<a name="cleanup-CFN-stack"></a>

If you created your Amazon ECS resources by following the console first\-run wizard, then your resources are contained in a AWS CloudFormation stack\. You can completely clean up all of your remaining AWS resources that are associated with this stack by deleting it\. Deleting the CloudFormation stack terminates the EC2 instances, removes the Auto Scaling group, deletes any Elastic Load Balancing load balancers, and removes the Amazon VPC subnets and Internet gateway associated with the cluster\.

**To delete the AWS CloudFormation stack**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. From the navigation bar, select the region that your cluster was created in\.

1. Select the stack that is associated with your Amazon ECS resources\. The **Stack Name** value starts with `EC2ContainerService-default`\.

1. Choose **Delete Stack** and then choose **Yes, Delete** to delete your stack resources\.