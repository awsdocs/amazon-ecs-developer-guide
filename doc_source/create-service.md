# Creating an Amazon ECS service<a name="create-service"></a>

When you create an Amazon ECS service, you specify the parameters that define what makes up your service and how your service behaves\. These parameters create a service definition\. For more information, see [Service definition parameters](service_definition_parameters.md)\.

For services that are hosted on Fargate or Amazon EC2 instances, you can optionally configure an Elastic Load Balancing load balancer to distribute traffic across the containers in your service\. For more information, see [Service load balancing](service-load-balancing.md)\.

**Note**  
When using a load balancer with services that are hosted on Amazon EC2 instances, verify that your instances can receive traffic from your load balancers\. You can allow traffic to all ports on your instances from your load balancer's security group\. This ensures that traffic can reach any containers that use dynamically assigned ports\.