# Service load balancing<a name="service-load-balancing"></a>

Your Amazon ECS service can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service\.

Amazon ECS services support the Application Load Balancer, Network Load Balancer, and Classic Load Balancer load balancer types\. Application Load Balancers are used to route HTTP/HTTPS \(or layer 7\) traffic\. Network Load Balancers are used to route TCP or UDP \(or layer 4\) traffic\. Classic Load Balancers are used to route TCP traffic\. For more information, see [Load balancer types](load-balancer-types.md)\.

Application Load Balancers offer several features that make them attractive for use with Amazon ECS services:
+ Each service can serve traffic from multiple load balancers and expose multiple load balanced ports by specifying multiple target groups\.
+ They are supported by tasks using both the Fargate and EC2 launch types\.
+ Application Load Balancers allow containers to use dynamic host port mapping \(so that multiple tasks from the same service are allowed per container instance\)\.
+ Application Load Balancers support path\-based routing and priority rules \(so that multiple services can use the same listener port on a single Application Load Balancer\)\.

We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of these latest features, unless your service requires a feature that is only available with Network Load Balancers or Classic Load Balancers\. For more information about Elastic Load Balancing and the differences between the load balancer types, see the [Elastic Load Balancing User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\.

**Topics**
+ [Service load balancing considerations](#load-balancing-considerations)
+ [Load balancer types](load-balancer-types.md)
+ [Creating a load balancer](create-load-balancer.md)
+ [Registering multiple target groups with a service](register-multiple-targetgroups.md)

## Service load balancing considerations<a name="load-balancing-considerations"></a>

Consider the following when you use service load balancing\.

### Application Load Balancer and Network Load Balancer considerations<a name="alb-considerations"></a>

The following considerations are specific to Amazon ECS services using Application Load Balancers or Network Load Balancers:
+ For services that use an Application Load Balancer or Network Load Balancer, you cannot attach more than five target groups to a service\.
+ For services with tasks using the `awsvpc` network mode, when you create a target group for your service, you must choose `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.
+ If your service uses an Application Load Balancer and requires access to multiple load balanced ports, such as port 80 and port 443 for an HTTP/HTTPS service, you can configure two listeners\. One listener is responsible for HTTPS that forwards the request to the service, and another listener that is responsible for redirecting HTTP requests to the appropriate HTTPS port\. For more information, see [Create a listener to your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-listener.html) in the *User Guide for Application Load Balancers*\.
+ Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.
+ After you create a service, the target group ARN or load balancer name, container name, and container port specified in the service definition are immutable\. You cannot add, remove, or change the load balancer configuration of an existing service\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\. 
+ If a service's task fails the load balancer health check criteria, the task is stopped and restarted\. This process continues until your service reaches the number of desired running tasks\.
+ The Application Load Balancer slow start mode is supported\. For more information, see [Application Load Balancer slow start mode considerations](#alb-slowstart-considerations)\.
+ When using Network Load Balancers configured with IP addresses as targets, requests are seen as coming from the Network Load Balancers private IP address\. This means that services behind an Network Load Balancer are effectively open to the world as soon as you allow incoming requesets and health checks in the target's security group\.
+ Using a Network Load Balancer to route UDP traffic to your Amazon ECS on Fargate tasks require the task to use platform version 1\.4 and is only supported in the following Regions:
  + US East \(N\. Virginia\) \- `us-east-1`
  + US West \(Oregon\) \- `us-west-2`
  + EU \(Ireland\) \- `eu-west-1`
  + Asia Pacific \(Tokyo\) \- `ap-northeast-1`
+ If you are experiencing problems with your load balancer\-enabled services, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.

### Application Load Balancer slow start mode considerations<a name="alb-slowstart-considerations"></a>

Application Load Balancers enabled for slow start mode are supported for Amazon ECS services\. For more information about slow start mode, see [Target groups for your Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html)\.

To ensure that the service scheduler ignores unhealthy container health checks until your tasks have warmed up and are ready to receive traffic, the following configurations are required:
+ You must configure your container health check to return an `UNHEALTHY` status until the slow start period has ended\.
+ You must configure the health check grace period value for your Amazon ECS service for the same duration as the slow start mode duration\.

Consider the following when you use different task network modes with Application Load Balancer slow start mode:
+ When using `awsvpc` network mode, each task is assigned its own elastic network interface \(ENI\) and IP address which allows the Application Load Balancer to register each task as a target in the target group\. This enables each newly registered target to have slow start mode enabled\.
+ When using `host` network mode, the task bypasses the Docker networking constructs and maps container ports directly to the Amazon EC2 instance’s network interface or interfaces\. You register the container instance as the Application Load Balancer target as opposed to the IP address of the task\. This means you can only run one task per instance if you want slow start mode to work effectively\. If you were to update an existing task or service, or restart the container instance, this does not re\-register the container instance as an Application Load Balancer target, which would not cause the slow start duration to begin\.
+ When using `bridge` network mode, similarly to using `host` network mode, you register the container instance as the Application Load Balancer target as opposed to the Amazon ECS task so the same considerations described above apply\.

Additionally, the following considerations are specific for using Application Load Balancer slow start mode and adding Amazon ECS tasks as targets:
+ When you enable slow start for a target group, the targets already registered with the target group do not enter slow start mode\.
+ When you enable slow start for an empty target group and then register one or more targets using a single registration operation, these targets do not enter slow start mode\. Newly registered targets enter slow start mode only when there is at least one registered target that is not in slow start mode\.
+ If you deregister a target in slow start mode, the target exits slow start mode\. If you register the same target again, it enters slow start mode again\.
+ If a target in slow start mode becomes unhealthy and then healthy again before the duration period elapses, the target remains in slow start mode until the duration period elapses and then exits slow start mode\. If a target that is not in slow start mode changes from unhealthy to healthy, it does not enter slow start mode\.

### Classic Load Balancer considerations<a name="clb-considerations"></a>

The following considerations are specific to Amazon ECS services using Classic Load Balancers:
+ Services with tasks that use the `awsvpc` network mode, such as those with the Fargate launch type, do not support Classic Load Balancers\.
+ All of the containers that are launched in a single task definition are always placed on the same container instance\. For Classic Load Balancers, you may choose to put multiple containers \(in the same task definition\) behind the same load balancer by defining multiple host ports in the service definition and adding those listener ports to the load balancer\. For example, if a task definition consists of Elasticsearch using port 3030 on the container instance, with Logstash and Kibana using port 4040 on the container instance, the same load balancer can route traffic to Elasticsearch and Kibana through two listeners\. For more information, see [Listeners for your Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html) in the *User Guide for Classic Load Balancers*\.
**Important**  
We do not recommend connecting multiple services to the same Classic Load Balancer\. Because entire container instances are registered and deregistered with Classic Load Balancers, and not with host and port combinations, this configuration can cause issues if a task from one service stops\. In this scenario, a task from one service stopping can cause the entire container instance to be deregistered from the Classic Load Balancer while another task from a different service on the same container instance is still using it\. If you want to connect multiple services to a single load balancer we recommend using an Application Load Balancer\.