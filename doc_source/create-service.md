# Creating a service<a name="create-service"></a>

When you create an Amazon ECS service, you specify the basic parameters that define what makes up your service and how it should behave\. These parameters create a service definition\.

You can optionally configure additional features, such as an Elastic Load Balancing load balancer to distribute traffic across the containers in your service\. For more information, see [Service load balancing](service-load-balancing.md)\. You must verify that your container instances can receive traffic from your load balancers\. You can allow traffic to all ports on your container instances from your load balancer's security group to ensure that traffic can reach any containers that use dynamically assigned ports\.

The following documents take you through each step of the create service wizard in the AWS Management Console\.

**Topics**
+ [Step 1: Configuring Basic Service Parameters](basic-service-params.md)
+ [Step 2: Configure a Network](service-configure-network.md)
+ [Step 3: Configuring Your Service to Use a Load Balancer](service-create-loadbalancer.md)
+ [Step 4: Configuring Your Service to Use Service Discovery](service-configure-servicediscovery.md)
+ [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)
+ [Step 6: Review and create your service](create-service-review.md)