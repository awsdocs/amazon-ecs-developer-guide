# Deleting a service using the new console<a name="delete-service-v2"></a>

You can delete an Amazon ECS service using the console\. Before deletion, the service is automatically scaled down to zero\. If you have a load balancer or service discovery resources associated with the service, they are not affected by the service deletion\. To delete your Elastic Load Balancing resources, see one of the following topics, depending on your load balancer type: [Delete an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-delete.html) or [Delete a Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-delete.html)\. To delete your service discovery resources, follow the procedure below\.

**To delete a service \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster for the service\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose the **Services** tab\. 

1. Select the services, and then choose **Delete**\.

1. At the confirmation prompt, enter **delete me** and then choose **Delete**\. 