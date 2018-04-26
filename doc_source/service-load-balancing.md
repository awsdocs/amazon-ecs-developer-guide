# Service Load Balancing<a name="service-load-balancing"></a>

Your Amazon ECS service can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service\.

Elastic Load Balancing supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers, and Amazon ECS services can use either type of load balancer\. Application Load Balancers are used to route HTTP/HTTPS \(or Layer 7\) traffic\. Network Load Balancers and Classic Load Balancers are used to route TCP \(or Layer 4\) traffic\. For more information, see [Load Balancer Types](load-balancer-types.md)\.

Application Load Balancers offer several features that make them attractive for use with Amazon ECS services:
+ Application Load Balancers allow containers to use dynamic host port mapping \(so that multiple tasks from the same service are allowed per container instance\)\.
+ Application Load Balancers support path\-based routing and priority rules \(so that multiple services can use the same listener port on a single Application Load Balancer\)\.

We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of these latest features, unless your service requires a feature that is only available with Network Load Balancers or Classic Load Balancers\. For more information about Elastic Load Balancing and the differences between the load balancer types, see the [Elastic Load Balancing User Guide](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\.

**Note**  
Currently, Amazon ECS services can only specify a single load balancer or target group\. If your service requires access to multiple load balanced ports \(for example, port 80 and port 443 for an HTTP/HTTPS service\), you must use a Classic Load Balancer with multiple listeners\. To use an Application Load Balancer, separate the single HTTP/HTTPS service into two services, where each handles requests for different ports\. Then, each service could use a different target group behind a single Application Load Balancer\.

**Topics**
+ [Load Balancing Concepts](#load-balancing-concepts)
+ [Load Balancer Types](load-balancer-types.md)
+ [Check the Service Role for Your Account](check-service-role.md)
+ [Creating a Load Balancer](create-load-balancer.md)

## Load Balancing Concepts<a name="load-balancing-concepts"></a>
+ All of the containers that are launched in a single task definition are always placed on the same container instance\. For Classic Load Balancers, you may choose to put multiple containers \(in the same task definition\) behind the same load balancer by defining multiple host ports in the service definition and adding those listener ports to the load balancer\. For example, if a task definition consists of Elasticsearch using port 3030 on the container instance, with Logstash and Kibana using port 4040 on the container instance, the same load balancer can route traffic to Elasticsearch and Kibana through two listeners\. For more information, see [Listeners for Your Classic Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html) in the *User Guide for Classic Load Balancers*\.
**Important**  
We do not recommend connecting multiple services to the same Classic Load Balancer\. Because entire container instances are registered and deregistered with Classic Load Balancers \(and not host and port combinations\), this configuration can cause issues if a task from one service stops, causing the entire container instance to be deregistered from the Classic Load Balancer while another task from a different service on the same container instance is still using it\. If you want to connect multiple services to a single load balancer \(for example, to save costs\), we recommend using an Application Load Balancer\.
+ There is a limit of one load balancer or target group per service\.
+ Services with tasks that use the `awsvpc` network mode \(for example, those with the Fargate launch type\) only support Application Load Balancers and Network Load Balancers; Classic Load Balancers are not supported\. Also, when you create any target groups for these services, you must choose `ip` as the target type, not `instance`, because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.
+ Your load balancer subnet configuration must include all Availability Zones that your container instances reside in\.
+ After you create a service, the target group ARN or load balancer name, container name, and container port specified in the service definition are immutable\. You cannot add, remove, or change the load balancer configuration of an existing service\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\. 
+ If a service's task fails the load balancer health check criteria, the task is killed and restarted\. This process continues until your service reaches the number of desired running tasks\.
+ If you are experiencing problems with your load balancer\-enabled services, see [Troubleshooting Service Load Balancers](troubleshoot-service-load-balancers.md)\.