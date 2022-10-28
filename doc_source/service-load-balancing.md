# Service load balancing<a name="service-load-balancing"></a>

Your Amazon ECS service can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service\.

**Note**  
When you use tasks sets, all the tasks in the set must all be configured to use Elastic Load Balancing or to not use Elastic Load Balancing\. 

Application Load Balancers offer several features that make them attractive for use with Amazon ECS services:
+ Each service can serve traffic from multiple load balancers and expose multiple load balanced ports by specifying multiple target groups\.
+ They are supported by tasks hosted on both Fargate and EC2 instances\.
+ Application Load Balancers allow containers to use dynamic host port mapping \(so that multiple tasks from the same service are allowed per container instance\)\.
+ Application Load Balancers support path\-based routing and priority rules \(so that multiple services can use the same listener port on a single Application Load Balancer\)\.

We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of these latest features, unless your service requires a feature that is only available with Network Load Balancers or Classic Load Balancers\. For more information about Elastic Load Balancing and the differences between the load balancer types, see the [Elastic Load Balancing User Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\.

With your load balancer, you pay only for what you use\. For more information, see [Elastic Load Balancing pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\. 

**Topics**
+ [Load balancer types](load-balancer-types.md)
+ [Creating a load balancer](create-load-balancer.md)
+ [Registering multiple target groups with a service](register-multiple-targetgroups.md)