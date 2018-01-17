# Creating a Load Balancer<a name="create-load-balancer"></a>

This section provides a hands\-on introduction to using Elastic Load Balancing through the AWS Management Console to use with your Amazon ECS services\. In this section, you create an external load balancer that receives public network traffic and routes it to your Amazon ECS container instances\.

Elastic Load Balancing supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers, and Amazon ECS services can use either type of load balancer\. Application Load Balancers are used to route HTTP/HTTPS traffic\. Network Load Balancers and Classic Load Balancers are used to route TCP or Layer 7 traffic\.

Application Load Balancers offer several features that make them particularly attractive for use with Amazon ECS services:

+ Application Load Balancers allow containers to use dynamic host port mapping \(so that multiple tasks from the same service are allowed per container instance\)\.

+ Application Load Balancers support path\-based routing and priority rules \(so that multiple services can use the same listener port on a single Application Load Balancer\)\.

We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of these latest features\. For more information about Elastic Load Balancing and the differences between the load balancer types, see the [Elastic Load Balancing User Guide](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\.

**Note**  
Currently, Amazon ECS services can only specify a single load balancer or target group\. If your service requires access to multiple load balanced ports \(for example, port 80 and port 443 for an HTTP/HTTPS service\), you must use a Classic Load Balancer with multiple listeners\. To use an Application Load Balancer, separate the single HTTP/HTTPS service into two services, where each handles requests for different ports\. Then, each service could use a different target group behind a single Application Load Balancer\.


+ [Creating an Application Load Balancer](create-application-load-balancer.md)
+ [Creating a Network Load Balancer](create-network-load-balancer.md)
+ [Creating a Classic Load Balancer](create-standard-load-balancer.md)