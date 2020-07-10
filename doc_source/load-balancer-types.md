# Load balancer types<a name="load-balancer-types"></a>

Elastic Load Balancing supports the following types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. Amazon ECS services can use either type of load balancer\. Application Load Balancers are used to route HTTP/HTTPS \(or Layer 7\) traffic\. Network Load Balancers and Classic Load Balancers are used to route TCP \(or Layer 4\) traffic\.

**Topics**
+ [Application Load Balancer](#alb)
+ [Network Load Balancer](#nlb)
+ [Classic Load Balancer](#clb)

## Application Load Balancer<a name="alb"></a>

An Application Load Balancer makes routing decisions at the application layer \(HTTP/HTTPS\), supports path\-based routing, and can route requests to one or more ports on each container instance in your cluster\. Application Load Balancers support dynamic host port mapping\. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance \(such as 32768 to 61000 on the latest Amazon ECS\-optimized AMI\)\. When the task is launched, the NGINX container is registered with the Application Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container\. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance\. For more information, see the [User Guide for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)\.

![\[Application Load Balancer\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alb.png)

## Network Load Balancer<a name="nlb"></a>

A Network Load Balancer makes routing decisions at the transport layer \(TCP/SSL\)\. It can handle millions of requests per second\. After the load balancer receives a connection, it selects a target from the target group for the default rule using a flow hash routing algorithm\. It attempts to open a TCP connection to the selected target on the port specified in the listener configuration\. It forwards the request without modifying the headers\. Network Load Balancers support dynamic host port mapping\. For example, if your task's container definition specifies port 80 for an NGINX container port, and port 0 for the host port, then the host port is dynamically chosen from the ephemeral port range of the container instance \(such as 32768 to 61000 on the latest Amazon ECS\-optimized AMI\)\. When the task is launched, the NGINX container is registered with the Network Load Balancer as an instance ID and port combination, and traffic is distributed to the instance ID and port corresponding to that container\. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance\. For more information, see the [User Guide for Network Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)\.

![\[Network Load Balancer\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alb.png)

## Classic Load Balancer<a name="clb"></a>

A Classic Load Balancer makes routing decisions at either the transport layer \(TCP/SSL\) or the application layer \(HTTP/HTTPS\)\. Classic Load Balancers currently require a fixed relationship between the load balancer port and the container instance port\. For example, it is possible to map the load balancer port 80 to the container instance port 3030 and the load balancer port 4040 to the container instance port 4040\. However, it is not possible to map the load balancer port 80 to port 3030 on one container instance and port 4040 on another container instance\. This static mapping requires that your cluster has at least as many container instances as the desired count of a single service that uses a Classic Load Balancer\. For more information, see the [User Guide for Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)\.

![\[Classic Load Balancer\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/clb.png)