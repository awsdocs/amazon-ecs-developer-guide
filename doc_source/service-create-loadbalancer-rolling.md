# Configuring a Load Balancer for the Rolling Update Deployment Type<a name="service-create-loadbalancer-rolling"></a>

If your service's tasks take a while to start and respond to Elastic Load Balancing health checks, you can specify a health check grace period of up to 2,147,483,647 seconds\. During that time, the service scheduler ignores health check status\. This grace period can prevent the service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\. This is only valid if your service is configured to use a load balancer\.<a name="service-health-check-grace-period"></a>

**To configure a health check grace period**

1. If you have not done so already, follow the basic service configuration procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. For **Health check grace period**: Enter the period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks after a task has first started\.

To configure your service to use a load balancer, you must choose the load balancer type to use with your service\.

**To choose a load balancer type**

1. If you have not done so already, follow the basic service creation procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. For **Load balancer type**, choose the load balancer type to use with your service:  
Application Load Balancer  
Allows containers to use dynamic host port mapping, which enables you to place multiple tasks using the same port on a single container instance\. Multiple services can use the same listener port on a single load balancer with rule\-based routing and paths\.  
Network Load Balancer  
Allows containers to use dynamic host port mapping, which enables you to place multiple tasks using the same port on a single container instance\. Multiple services can use the same listener port on a single load balancer with rule\-based routing\.  
Classic Load Balancer  
Requires static host port mappings \(only one task allowed per container instance\); rule\-based routing and paths are not supported\.

   We recommend that you use Application Load Balancers for your Amazon ECS services so that you can take advantage of the advanced features available to them\.

1. For **Select IAM role for service**, choose **Create new role** to create a new role for your service, or select an existing IAM role to use for your service \(by default, this is `ecsServiceRole`\)\.
**Important**  
If you choose to use an existing `ecsServiceRole` IAM role, you must verify that the role has the proper permissions to use Application Load Balancers and Classic Load Balancers\. For more information, see [Service Scheduler IAM Role](ecs-legacy-iam-roles.md#service_IAM_role)\.

1. For **ELB Name**, choose the name of the load balancer to use with your service\. Only load balancers that correspond to the load balancer type you selected earlier are visible here\.

1. The next step depends on the load balancer type for your service\. If you've chosen an Application Load Balancer, follow the steps in [To configure an Application Load Balancer](#create-service-configure-alb)\. If you've chosen a Network Load Balancer, follow the steps in [To configure a Network Load Balancer](#create-service-configure-nlb)\. If you've chosen a Classic Load Balancer, follow the steps in [To configure a Classic Load Balancer](#create-service-configure-clb)\.<a name="create-service-configure-alb"></a>

**To configure an Application Load Balancer**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Listener port**, choose the listener port and protocol of the listener that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol for **Listener protocol**\.

1. For **Target group name**, choose the target group that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
   + For **Target group name**, a default name is provided for you\.
   + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
   + For **Path pattern**, if your listener does not have any existing rules, the default path pattern \(`/`\) is used\. If your listener already has a default rule, then you must enter a path pattern that matches traffic that you want to have sent to your service's target group\. For example, if your service is a web application called `web-app`, and you want traffic that matches `http://my-elb-url/web-app` to route to your service, then you would enter `/web-app*` as your path pattern\. For more information, see [ListenerRules](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#listener-rules) in the *User Guide for Application Load Balancers*\.
   + For **Health check path**, enter the path to which the load balancer should send health check pings\.

1. When you are finished configuring your Application Load Balancer, choose **Next step**\.<a name="create-service-configure-nlb"></a>

**To configure a Network Load Balancer**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Listener port**, choose the listener port and protocol of the listener that you created in [Creating a Network Load Balancer](create-network-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol for **Listener protocol**\.

1. For **Target group name**, choose the target group that you created in [Creating a Network Load Balancer](create-network-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
   + For **Target group name**, a default name is provided for you\. 
   + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
   + For **Health check path**, enter the path to which the load balancer should send health check pings\.

1. When you are finished configuring your Network Load Balancer, choose **Next Step**\.<a name="create-service-configure-clb"></a>

**To configure a Classic Load Balancer**

1. The **Health check port**, **Health check protocol**, and **Health check path** fields are all pre\-populated with the values you configured in [Creating a Classic Load Balancer](create-standard-load-balancer.md) \(if applicable\)\. You can update these settings in the Amazon EC2 console\.

1. For **Container for ELB health check**, choose the container to send health checks\.

1. When you are finished configuring your Classic Load Balancer, choose **Next step**\.