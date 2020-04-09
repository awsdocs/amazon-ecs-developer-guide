# Configuring a Load Balancer for the Blue/Green Deployment Type<a name="service-create-loadbalancer-bluegreen"></a>

To configure your service that uses the blue/green deployment type to use a load balancer, you must use either an Application Load Balancer or a Network Load Balancer\.

**To choose a load balancer type**

1. If you have not done so already, follow the basic service creation procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. For **Load balancer type**, choose the load balancer type to use with your service:  
Application Load Balancer  
Allows containers to use dynamic host port mapping, which enables you to place multiple tasks using the same port on a single container instance\. Multiple services can use the same listener port on a single load balancer with rule\-based routing and paths\.  
Network Load Balancer  
Allows containers to use dynamic host port mapping, which enables you to place multiple tasks using the same port on a single container instance\. Multiple services can use the same listener port on a single load balancer with rule\-based routing\.

   We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of the advanced features available to them\.

1. For **Load balancer name**, choose the name of the load balancer to use with your service\. Only load balancers that correspond to the load balancer type you selected earlier are visible here\.

1. The next step depends on the load balancer type for your service\. If you've chosen an Application Load Balancer, follow the steps in [To configure an Application Load Balancer](service-create-loadbalancer-rolling.md#create-service-configure-alb)\. If you've chosen a Network Load Balancer, follow the steps in [To configure a Network Load Balancer](service-create-loadbalancer-rolling.md#create-service-configure-nlb)\.<a name="create-service-configure-alb-bluegreen"></a>

**To configure an Application Load Balancer for the blue/green deployment type**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Production listener port**, choose the listener port and protocol of the listener that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol for **Production listener protocol**\.

1. \(Optional\) Select **Test listener** if you want to configure a listener port and protocol on your load balancer to test updates to your service before routing traffic to your new taskset\. Complete the following step:

   1. For **Test listener port**, choose the listener port and protocol of the listener that you want to test traffic over, or choose **create new** to create a new test listener and then enter a port number and choose a port protocol in **Test listener protocol**\.

1. For blue/green deployments, two target groups are required\. Each target group binds to a separate taskset in the deployment\. Complete the following steps:

   1. For **Target group 1 name**, choose the target group that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

   1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
      + For **Target group name**, enter a name for your target group\.
      + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
      + For **Path pattern**, if your listener does not have any existing rules, the default path pattern \(`/`\) is used\. If your listener already has a default rule, then you must enter a path pattern that matches traffic that you want to have sent to your service's target group\. For example, if your service is a web application called `web-app`, and you want traffic that matches `http://my-elb-url/web-app` to route to your service, then you would enter `/web-app*` as your path pattern\. For more information, see [ListenerRules](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#listener-rules) in the *User Guide for Application Load Balancers*\.
      + For **Health check path**, enter the path to which the load balancer should send health check pings\.

   1. Repeat the steps for target group 2\.

   1. When you are finished configuring your Application Load Balancer, choose **Next step**\. Navigate to [Step 4: Configuring Your Service to Use Service Discovery](service-configure-servicediscovery.md)\.<a name="create-service-configure-nlb-bluegreen"></a>

**To configure a Network Load Balancer for the blue/green deployment type**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Listener port**, choose the listener port and protocol of the listener that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol for **Listener protocol**\.

1. For **Target group name**, choose the target group that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
   + For **Target group name**, enter a name for your target group\.
   + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
   + For **Health check path**, enter the path to which the load balancer should send health check pings\.

1. When you are finished configuring your Network Load Balancer, choose **Next Step**\. Navigate to [Step 4: Configuring Your Service to Use Service Discovery](service-configure-servicediscovery.md)\.