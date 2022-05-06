# Service load balancing<a name="service-load-balancing"></a>

Your Amazon ECS service can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service\.

**Note**  
When you use tasks sets, all the tasks in the set must all be configured to use Elastic Load Balancing or to not use Elastic Load Balancing\. 

Amazon ECS services hosted on Amazon EC2 instances support the Application Load Balancer, Network Load Balancer, and Classic Load Balancer load balancer types\. Amazon ECS services hosted on AWS Fargate support Application Load Balancer and Network Load Balancer only\. Application Load Balancers are used to route HTTP/HTTPS \(or layer 7\) traffic\. Network Load Balancers are used to route TCP or UDP \(or layer 4\) traffic\. Classic Load Balancers are used to route TCP traffic\. For more information, see [Load balancer types](load-balancer-types.md)\.

Application Load Balancers offer several features that make them attractive for use with Amazon ECS services:
+ Each service can serve traffic from multiple load balancers and expose multiple load balanced ports by specifying multiple target groups\.
+ They are supported by tasks hosted on both Fargate and EC2 instances\.
+ Application Load Balancers allow containers to use dynamic host port mapping \(so that multiple tasks from the same service are allowed per container instance\)\.
+ Application Load Balancers support path\-based routing and priority rules \(so that multiple services can use the same listener port on a single Application Load Balancer\)\.

We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of these latest features, unless your service requires a feature that is only available with Network Load Balancers or Classic Load Balancers\. For more information about Elastic Load Balancing and the differences between the load balancer types, see the [Elastic Load Balancing User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\.

With your load balancer, you pay only for what you use\. For more information, see [Elastic Load Balancing pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. 

**Topics**
+ [Service load balancing considerations](#load-balancing-considerations)
+ [Load balancer types](load-balancer-types.md)
+ [Creating a load balancer](create-load-balancer.md)
+ [Registering multiple target groups with a service](register-multiple-targetgroups.md)

## Service load balancing considerations<a name="load-balancing-considerations"></a>

Consider the following when you use service load balancing\.

### Application Load Balancer and Network Load Balancer considerations<a name="alb-considerations"></a>

The following considerations are specific to Amazon ECS services using Application Load Balancers or Network Load Balancers:
+ Amazon ECS requires the service\-linked IAM role which provides the permissions needed to register and deregister targets with your load balancer when tasks are created and stopped\. For more information, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.
+ For services that use an Application Load Balancer or Network Load Balancer, you cannot attach more than five target groups to a service\.
+ For services with tasks using the `awsvpc` network mode, when you create a target group for your service, you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.
+ If your service uses an Application Load Balancer and requires access to multiple load balanced ports, such as port 80 and port 443 for an HTTP/HTTPS service, you can configure two listeners\. One listener is responsible for HTTPS that forwards the request to the service, and another listener that is responsible for redirecting HTTP requests to the appropriate HTTPS port\. For more information, see [Create a listener to your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-listener.html) in the *User Guide for Application Load Balancers*\.
+ Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.
+ After you create a service, the target group ARN or load balancer name, container name, and container port specified in the service definition are immutable\. You can use the AWS CLI or SDK to modify the load balancer configuration\. For information about how to modify the configuration, see [UpdateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html) in the *Amazon Elastic Container Service API Reference*\. You cannot add, remove, or change the load balancer configuration of an existing service\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\. 
+ If a service's task fails the load balancer health check criteria, the task is stopped and restarted\. This process continues until your service reaches the number of desired running tasks\.
+ When using Network Load Balancers configured with IP addresses as targets, requests are seen as coming from the Network Load Balancers private IP address\. This means that services behind an Network Load Balancer are effectively open to the world as soon as you allow incoming requests and health checks in the target's security group\.
+ Using a Network Load Balancer to route UDP traffic to your Amazon ECS tasks on Fargate require the task to use platform version `1.4.0` \(Linux\) or `1.0.0` \(Windows\)\.
+ Minimize errors in your client applications by setting the `StopTimeout` in the task definition longer than the target group deregistration delay, which should be longer than your client connection timeout\. See the Builders Library for more information on recommended client configuration [ here ](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter)\.

  Also, the Network Load Balancer target group attribute for connection termination closes all remaining connections after the deregistration time\. This can cause clients to display undesired error messages, if the client does not handle them\.
+ If you are experiencing problems with your load balancer\-enabled services, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.

### Classic Load Balancer considerations<a name="clb-considerations"></a>

The following considerations are specific to Amazon ECS services using Classic Load Balancers:
+ Services with tasks that use the `awsvpc` network mode, such as those with the Fargate launch type, do not support Classic Load Balancers\.
+ All of the containers that are launched in a single task definition are always placed on the same container instance\. For Classic Load Balancers, you may choose to put multiple containers \(in the same task definition\) behind the same load balancer by defining multiple host ports in the service definition and adding those listener ports to the load balancer\. For example, if a task definition consists of OpenSearch Service using port 3030 on the container instance, with Logstash and OpenSearch Dashboards using port 4040 on the container instance, the same load balancer can route traffic to OpenSearch Service and OpenSearch Dashboards through two listeners\. For more information, see [Listeners for your Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html) in the *User Guide for Classic Load Balancers*\.
**Important**  
We do not recommend connecting multiple services to the same Classic Load Balancer\. Because entire container instances are registered and deregistered with Classic Load Balancers, and not with host and port combinations, this configuration can cause issues if a task from one service stops\. In this scenario, a task from one service stopping can cause the entire container instance to be deregistered from the Classic Load Balancer while another task from a different service on the same container instance is still using it\. If you want to connect multiple services to a single load balancer we recommend using an Application Load Balancer\.
+ Using capacity providers is not supported when using Classic Load Balancers for your services\.