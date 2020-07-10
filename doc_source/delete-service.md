# Deleting a service<a name="delete-service"></a>

You can delete an Amazon ECS service using the console\. Before deletion, the service is automatically scaled down to zero\. If you have a load balancer or service discovery resources associated with the service, they are not affected by the service deletion\. To delete your Elastic Load Balancing resources, see one of the following topics, depending on your load balancer type: [Delete an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-delete.html) or [Delete a Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-delete.html)\. To delete your service discovery resources, follow the procedure below\.

**To delete an Amazon ECS service**

Use the following procedure to delete an Amazon ECS service\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the Region that your cluster is in\.

1. In the navigation pane, choose **Clusters** and select the name of the cluster in which your service resides\.

1. On the **Cluster : *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Delete**\.

1. Confirm the service deletion by entering the text phrase and choose **Delete**\.

**To delete the service discovery resources \(AWS CLI\)**

To delete the remaining service discovery resources, you can use the AWS CLI to delete the service discovery service and service discovery namespace\.

1. Ensure that the latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading your AWS CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

1. Retrieve the ID of the service discovery service to delete\.

   ```
   aws servicediscovery list-services --region <region_name>
   ```
**Note**  
If no service discovery service is returned, continue to step 4\.

1. Using the service discovery service ID from the previous output, delete the service\.

   ```
   aws servicediscovery delete-service --id <service_discovery_service_id> --region <region_name>
   ```

1. Retrieve the ID of the service discovery namespace to delete\.

   ```
   aws servicediscovery list-namespaces --region <region_name>
   ```

1. Using the service discovery namespace ID from the previous output, delete the namespace\.

   ```
   aws servicediscovery delete-namespace --id <service_discovery_namespace_id> --region <region_name>
   ```