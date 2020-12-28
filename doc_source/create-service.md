# Creating a service<a name="create-service"></a>

When you create an Amazon ECS service, you specify the basic parameters that define what makes up your service and how it should behave\. These parameters create a service definition\.

You can optionally configure additional features, such as an Elastic Load Balancing load balancer to distribute traffic across the containers in your service\. For more information, see [Service load balancing](service-load-balancing.md)\. You must verify that your container instances can receive traffic from your load balancers\. You can allow traffic to all ports on your container instances from your load balancer's security group to ensure that traffic can reach any containers that use dynamically assigned ports\.

The following documents take you through each step of the create service wizard in the AWS Management Console\.

**Topics**
+ [Creating a service using the new console](create-service-console-v2.md)
+ [Creating a service using the old console](create-service-console-v1.md)