# Step 3: Configuring Your Service to Use a Load Balancer<a name="service-create-loadbalancer"></a>

Services can be configured to use a load balancer to distribute incoming traffic to the tasks in your service\. If your service is using the rolling update deployment type, this is optional\. If your service is using the blue/green deployment type, then it is required to use either an Application Load Balancer or Network Load Balancer\.

If you are not configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [Step 4: Configuring Your Service to Use Service Discovery](service-configure-servicediscovery.md)\.

If you have an available Elastic Load Balancing load balancer configured, you can attach it to your service with the following procedures, or you can configure a new load balancer\. For more information, see [Creating a load balancer](create-load-balancer.md)\.

**Important**  
Before following these procedures, you must create your Elastic Load Balancing load balancer resources\.

**Topics**
+ [Configuring a Load Balancer for the Rolling Update Deployment Type](service-create-loadbalancer-rolling.md)
+ [Configuring a Load Balancer for the Blue/Green Deployment Type](service-create-loadbalancer-bluegreen.md)