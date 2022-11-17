# Load balancer types<a name="load-balancer-types"></a>

Elastic Load Balancing supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. Amazon ECS services can use these types of load balancer\. Application Load Balancers are used to route HTTP/HTTPS \(or Layer 7\) traffic\. Network Load Balancers and Classic Load Balancers are used to route TCP \(or Layer 4\) traffic\.

**Topics**
+ [Application Load Balancer](#alb)
+ [Network Load Balancer](#nlb)
+ [Classic Load Balancer](#clb)

## Application Load Balancer<a name="alb"></a>

An Application Load Balancer makes routing decisions at the application layer \(HTTP/HTTPS\), supports path\-based routing, and can route requests to one or more ports on each container instance in your cluster\. Application Load Balancers support dynamic host port mapping\. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance \(such as 32768 to 61000 on the latest Amazon ECS\-optimized AMI\)\. When the task is launched, the NGINX container is registered with the Application Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container\. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance\. For more information, see the [User Guide for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)\.

![\[Application Load Balancer\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alb.png)

### Application Load Balancer and Network Load Balancer considerations<a name="alb-considerations"></a>

The following considerations are specific to Amazon ECS services using Application Load Balancers or Network Load Balancers:
+ Amazon ECS requires the service\-linked IAM role which provides the permissions needed to register and deregister targets with your load balancer when tasks are created and stopped\. For more information, see [Using service\-linked roles for Amazon ECS](using-service-linked-roles.md)\.
+ For services that use an Application Load Balancer or Network Load Balancer, you cannot attach more than five target groups to a service\.
+ For services with tasks using the `awsvpc` network mode, when you create a target group for your service, you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.
+ If your service uses an Application Load Balancer and requires access to multiple load balanced ports, such as port 80 and port 443 for an HTTP/HTTPS service, you can configure two listeners\. One listener is responsible for HTTPS that forwards the request to the service, and another listener that is responsible for redirecting HTTP requests to the appropriate HTTPS port\. For more information, see [Create a listener to your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-listener.html) in the *User Guide for Application Load Balancers*\.
+ Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.
+ After you create a service, the load balancer configuration can't be changed from the AWS Management Console\. You can use the AWS Copilot, AWS CloudFormation, AWS CLI or SDK to modify the load balancer configuration for the `ECS` rolling deployment controller only, not AWS CodeDeploy blue/green or external\. When you add, update, or remove a load balancer configuration, Amazon ECS starts a new deployment with the updated Elastic Load Balancing configuration\. This causes tasks to register to and deregister from load balancers\. We recommend that you verify this on a test environment before you update the Elastic Load Balancing configuration\. For information about how to modify the configuration, see [UpdateService](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html) in the *Amazon Elastic Container Service API Reference*\. 
+ If a service's task fails the load balancer health check criteria, the task is stopped and restarted\. This process continues until your service reaches the number of desired running tasks\.
+ When using Network Load Balancers configured with IP addresses as targets, requests are seen as coming from the Network Load Balancers private IP address\. This means that services behind an Network Load Balancer are effectively open to the world as soon as you allow incoming requests and health checks in the target's security group\.
+ Using a Network Load Balancer to route UDP traffic to your Amazon ECS tasks on Fargate require the task to use platform version `1.4.0` \(Linux\) or `1.0.0` \(Windows\)\.
+ Minimize errors in your client applications by setting the `StopTimeout` in the task definition longer than the target group deregistration delay, which should be longer than your client connection timeout\. See the Builders Library for more information on recommended client configuration [ here ](https://aws.amazon.com/builders-library/timeouts-retries-and-backoff-with-jitter)\.

  Also, the Network Load Balancer target group attribute for connection termination closes all remaining connections after the deregistration time\. This can cause clients to display undesired error messages, if the client does not handle them\.
+ If you are experiencing problems with your load balancer\-enabled services, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.
+ Your tasks and load balancer \(Application Load Balancer or Network Load Balancer\) must be in the same VPC\.

## Network Load Balancer<a name="nlb"></a>

A Network Load Balancer makes routing decisions at the transport layer \(TCP/SSL\)\. It can handle millions of requests per second\. After the load balancer receives a connection, it selects a target from the target group for the default rule using a flow hash routing algorithm\. It attempts to open a TCP connection to the selected target on the port specified in the listener configuration\. It forwards the request without modifying the headers\. Network Load Balancers support dynamic host port mapping\. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance \(such as 32768 to 61000 on the latest Amazon ECS\-optimized AMI\)\. When the task is launched, the NGINX container is registered with the Network Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container\. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance\. For more information, see the [User Guide for Network Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)\.

![\[Network Load Balancer\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alb.png)

## Classic Load Balancer<a name="clb"></a>


|  | 
| --- |
| We retired EC2\-Classic on August 15, 2022\. To avoid interruptions to your workloads, we recommend that you migrate from EC2\-Classic to a VPC prior to August 15, 2022\. For more information, see the blog post [EC2\-Classic Networking is Retiring \- Here's How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/)\. For information about how to migrate EC2\-Classic to a VPC, see [Migrate from EC2 Classic to a VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html) in the Amazon EC2 User Guide for Linux Instances\. | 

### Classic Load Balancer considerations<a name="clb-considerations"></a>

The following considerations are specific to Amazon ECS services using Classic Load Balancers:
+ Services with tasks that use the `awsvpc` network mode, such as those with the Fargate launch type, do not support Classic Load Balancers\.
+ All of the containers that are launched in a single task definition are always placed on the same container instance\. For Classic Load Balancers, you may choose to put multiple containers \(in the same task definition\) behind the same load balancer by defining multiple host ports in the service definition and adding those listener ports to the load balancer\. For example, if a task definition consists of OpenSearch Service using port 3030 on the container instance, with Logstash and OpenSearch Dashboards using port 4040 on the container instance, the same load balancer can route traffic to OpenSearch Service and OpenSearch Dashboards through two listeners\. For more information, see [Listeners for your Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html) in the *User Guide for Classic Load Balancers*\.
**Important**  
We do not recommend connecting multiple services to the same Classic Load Balancer\. Because entire container instances are registered and deregistered with Classic Load Balancers, and not with host and port combinations, this configuration can cause issues if a task from one service stops\. In this scenario, a task from one service stopping can cause the entire container instance to be deregistered from the Classic Load Balancer while another task from a different service on the same container instance is still using it\. If you want to connect multiple services to a single load balancer we recommend using an Application Load Balancer\.
+ Using capacity providers is not supported when using Classic Load Balancers for your services\.