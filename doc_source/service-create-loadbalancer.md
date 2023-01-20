# Step 3: Configuring your service to use a load balancer<a name="service-create-loadbalancer"></a>

**Important**  
Amazon ECS has a new console experience for creating a service\. For more information, see [Creating a service using the console](create-service-console-v2.md)\.

Services can be configured to use a load balancer to distribute incoming traffic to the tasks in your service\. If your service is using the rolling update deployment type, this is optional\. If your service is using the blue/green deployment type, then it is required to use either an Application Load Balancer or Network Load Balancer\.

If you aren't configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [Step 4: Configuring your service to use Service Discovery](service-configure-servicediscovery.md)\.

If you configured an available Elastic Load Balancing load balancer, you can attach it to your service with the following procedures, or you can configure a new load balancer\. For more information, see [Creating a load balancer](create-load-balancer.md)\.

**Important**  
Before following these procedures, you must create your Elastic Load Balancing load balancer resources\.

**Topics**
+ [Configuring a load balancer for the rolling update deployment type](service-create-loadbalancer-rolling.md)
+ [Configuring a load balancer for the blue/green deployment type](service-create-loadbalancer-bluegreen.md)