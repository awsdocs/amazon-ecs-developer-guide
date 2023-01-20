# Deleting a service using the console<a name="delete-service-v2"></a>

You can delete an Amazon ECS service using the console\. The service is automatically scaled down to zero before it is deleted\. Load balancer resources or service discovery resources associated with the service are not affected by the service deletion\. To delete your Elastic Load Balancing resources, see one of the following topics, depending on your load balancer type: [Delete an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-delete.html) or [Delete a Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-delete.html)\. 

**To delete a service \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster for the service\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose the **Services** tab\. 

1. To delete a service even if it wasn't scaled down to zero tasks, select **Force delete service**\.

1. Select the services, and then choose **Delete**\.

1. At the confirmation prompt, enter **delete** and then choose **Delete**\. 