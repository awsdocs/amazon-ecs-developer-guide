# Creating a Service<a name="create-service"></a>

When you create an Amazon ECS service, you specify the basic parameters that define what makes up your service and how it should behave\. These parameters create a service definition\.

You can optionally configure additional features, such as an Elastic Load Balancing load balancer to distribute traffic across the containers in your service\. For more information, see [Service Load Balancing](service-load-balancing.md)\. You must verify that your container instances can receive traffic from your load balancers\. You can allow traffic to all ports on your container instances from your load balancer's security group to ensure that traffic can reach any containers that use dynamically assigned ports\.

## Configuring Basic Service Parameters<a name="basic-service-params"></a>

All services require some basic configuration parameters that define the service, such as the task definition to use, which cluster the service should run on, how many tasks should be placed for the service, and so on; this is called the service definition\. For more information about the parameters defined in a service definition, see [Service Definition Parameters](service_definition_parameters.md)\.

This procedure covers creating a service with the basic service definition parameters that are required\. After you have configured these parameters, you can create your service or move on to the procedures for optional service definition configuration, such as configuring your service to use a load balancer\.

**To configure the basic service definition parameters**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the region that your cluster is in\.

1. In the navigation pane, choose **Task Definitions** and select the task definition from which to create your service\.

1. On the **Task Definition name** page, select the revision of the task definition from which to create your service\.

1. Review the task definition, and choose **Actions**, **Create Service**\.

1. On the **Configure service** page, fill out the following parameters accordingly:
   + **Launch type**: Choose whether your service should run tasks on Fargate infrastructure, or Amazon EC2 container instances that you maintain\.
   + **Platform version**: If you selected the Fargate launch type, then select the platform version to use\.
   + **Cluster**: Select the cluster in which to create your service\.
   + **Service name**: Type a unique name for your service\.
   + **Number of tasks**, type the number of tasks to launch and maintain on your cluster\.
**Note**  
If your launch type is `EC2`, and your task definition uses static host port mappings on your container instances, then you need at least one container instance with the specified port available in your cluster for each task in your service\. This restriction does not apply if your task definition uses dynamic host port mappings with the `bridge` network mode\. For more information, see [portMappings](task_definition_parameters.md#ContainerDefinition-portMappings)\.
   + **Minimum healthy percent**: Specify a lower limit on the number of your service's tasks that must remain in the `RUNNING` state during a deployment, as a percentage of the service's desired number of tasks \(rounded up to the nearest integer\)\. For example, if your service has a desired number of four tasks and a minimum healthy percent of 50%, the scheduler may stop two existing tasks to free up cluster capacity before starting two new tasks\. Tasks for services that do not use a load balancer are considered healthy if they are in the `RUNNING` state\. Tasks for services that do use a load balancer are considered healthy if they are in the `RUNNING` state and when the container instance on which it is hosted is reported as healthy by the load balancer\. The default value for the minimum healthy percent is 50% in the console, and 100% with the AWS CLI or SDKs\.
   + **Maximum percent**: Specify an upper limit on the number of your service's tasks that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the service's desired number of tasks \(rounded down to the nearest integer\)\. For example, if your service has a desired number of four tasks and a maximum percent value of 200%, the scheduler may start four new tasks before stopping the four older tasks \(provided that the cluster resources required to do this are available\)\. The default value for maximum percent is 200%\.

1. \(Optional\) If you selected the EC2 launch type, for **Task Placement**, you can specify how tasks are placed using task placement strategies and constraints\. Choose from the following options:
   + **AZ Balanced Spread** \- distribute tasks across Availability Zones and across container instances in the Availability Zone\.
   + **AZ Balanced BinPack** \- distribute tasks across Availability Zones and across container instances with the least available memory\.
   + **BinPack** \- distribute tasks based on the least available amount of CPU or memory\.
   + **One Task Per Host** \- place, at most, one task from the service on each container instance\.
   + **Custom** \- define your own task placement strategy\. See [Amazon ECS Task Placement](task-placement.md) for examples\.

    For more information, see [Amazon ECS Task Placement](task-placement.md)\.

1. Choose **Next step** to proceed\.

## Configure a Network<a name="service-configure-network"></a>

### VPC and Security Groups<a name="service-awsvpc"></a>

If your service's task definition uses the `awsvpc` network mode, you must configure a VPC, subnet, and security group settings for your service\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.

**To configure VPC and security group settings for your service**

1. For **Cluster VPC**, if you selected the EC2 launch type, choose the VPC in which your container instances reside\. If you selected the Fargate launch type, select the VPC that the Fargate tasks should use\. Ensure that the VPC you choose was not configured to require dedicated hardware tenancy as that is not supported by Fargate tasks\.

1. For **Subnets**, choose the available subnets for your service task placement\.
**Important**  
Only private subnets are supported for the `awsvpc` network mode\. Because tasks do not receive public IP addresses, a NAT gateway is required for outbound internet access, and inbound internet traffic should be routed through a load balancer\.

1. For **Security groups**, a security group has been created for your service's tasks, which allows HTTP traffic from the internet \(0\.0\.0\.0/0\)\. To edit the name or the rules of this security group, or to choose an existing security group, choose **Edit** and then modify your security group settings\.

1. For **Auto\-assign Public IP**, choose whether to have your tasks receive a public IP address\. If you are using Fargate tasks, a public IP address needs to be assigned to the task's elastic network interface, with a route to the internet, or a NAT gateway that can route requests to the internet, in order for the task to pull container images\.

### \(Optional\) Health Check Grace Period<a name="service-health-check-grace-period"></a>

If your service's tasks take a while to start and respond to Elastic Load Balancing health checks, you can specify a health check grace period of up to 7,200 seconds during which the ECS service scheduler ignores health check status\. This grace period can prevent the ECS service scheduler from marking tasks as unhealthy and stopping them before they have time to come up\. This is only valid if your service is configured to use a load balancer\.
+ **Health check grace period**: Enter the period of time, in seconds, that the Amazon ECS service scheduler should ignore unhealthy Elastic Load Balancing target health checks after a task has first started\.

### \(Optional\) Configuring Your Service to Use a Load Balancer<a name="service-configure-load-balancing"></a>

If you are not configuring your service to use a load balancer, you can choose **None** as the load balancer type and move on to the next section, [\(Optional\) Configuring Your Service to Use Service Auto Scaling](#service-configure-auto-scaling)\.

If you have an available Elastic Load Balancing load balancer configured, you can attach it to your service with the following procedures, or you can configure a new load balancer\. For more information, see [Creating a Load Balancer](create-load-balancer.md)\.

**Note**  
You must create your Elastic Load Balancing load balancer resources before following these procedures\.

First, you must choose the load balancer type to use with your service\. Then you can configure your service to work with the load balancer\.

**To choose a load balancer type**

1. If you have not done so already, follow the basic service creation procedures in [Configuring Basic Service Parameters](#basic-service-params)\.

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
If you choose to use an existing `ecsServiceRole` IAM role, you must verify that the role has the proper permissions to use Application Load Balancers and Classic Load Balancers\. For more information, see [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md)\.

1. For **ELB Name**, choose the name of the load balancer to use with your service\. Only load balancers that correspond to the load balancer type you selected earlier are visible here\.

1. The next step depends on the load balancer type for your service\. If you've chosen an Application Load Balancer, follow the steps in [To configure an Application Load Balancer](#create-service-configure-alb)\. If you've chosen a Network Load Balancer, follow the steps in [To configure a Network Load Balancer](#create-service-configure-nlb)\. If you've chosen a Classic Load Balancer, follow the steps in [To configure a Classic Load Balancer](#create-service-configure-clb)\.<a name="create-service-configure-alb"></a>

**To configure an Application Load Balancer**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Listener port**, choose the listener port and protocol of the listener that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol in **Listener protocol**\.

1. For **Target group name**, choose the target group that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
   + For **Target group name**, enter a name for your target group\.
   + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
   + For **Path pattern**, if your listener does not have any existing rules, the default path pattern \(`/`\) is used\. If your listener already has a default rule, then you must enter a path pattern that matches traffic that you want to have sent to your service's target group\. For example, if your service is a web application called `web-app`, and you want traffic that matches `http://my-elb-url/web-app` to route to your service, then you would enter `/web-app*` as your path pattern\. For more information, see [ListenerRules](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#listener-rules) in the *User Guide for Application Load Balancers*\.
   + For **Health check path**, enter the path to which the load balancer should send health check pings\.

1. When you are finished configuring your Application Load Balancer, choose **Next step**\.<a name="create-service-configure-nlb"></a>

**To configure a Network Load Balancer**

1. For **Container to load balance**, choose the container and port combination from your task definition that your load balancer should distribute traffic to, and choose **Add to load balancer**\.

1. For **Listener port**, choose the listener port and protocol of the listener that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new listener and then enter a port number and choose a port protocol in **Listener protocol**\.

1. For **Target group name**, choose the target group that you created in [Creating an Application Load Balancer](create-application-load-balancer.md) \(if applicable\), or choose **create new** to create a new target group\.
**Important**  
If your service's task definition uses the `awsvpc` network mode \(which is required for the Fargate launch type\), your target group must use `ip` as the target type, not `instance`\. This is because tasks that use the `awsvpc` network mode are associated with an elastic network interface, not an Amazon EC2 instance\.

1. \(Optional\) If you chose to create a new target group, complete the following fields as follows:
   + For **Target group name**, enter a name for your target group\.
   + For **Target group protocol**, enter the protocol to use for routing traffic to your tasks\.
   + For **Health check path**, enter the path to which the load balancer should send health check pings\.

1. When you are finished configuring your Network Load Balancer, choose **Next Step**\.<a name="create-service-configure-clb"></a>

**To configure a Classic Load Balancer**

1. The **Health check port**, **Health check protocol**, and **Health check path** fields are all pre\-populated with the values you configured in [Creating a Classic Load Balancer](create-standard-load-balancer.md) \(if applicable\)\. You can update these settings in the Amazon EC2 console\.

1. For **Container for ELB health check**, choose the container to send health checks\.

1. When you are finished configuring your Classic Load Balancer, choose **Next step**\.

## \(Optional\) Configuring Your Service to Use Service Discovery<a name="service-configure-servicediscovery"></a>

Your Amazon ECS service can optionally enable service discovery integration, which allows your service to be discoverable via DNS\. For more information, see [Service Discovery](service-discovery.md)\.

**To configure service discovery**

1. If you have not done so already, follow the basic service creation procedures in [Configuring Basic Service Parameters](#basic-service-params)\.

1. On the **Configure network** page, select **Enable service discovery integration**\.

1. For **Namespace**, select an existing Amazon Route 53 namespace, if you have one, otherwise select **create new private namespace**\.

1. If creating a new namespace, for **Namespace name** enter a descriptive name for your namespace\. This is the name used for the Amazon Route 53 hosted zone\.

1. For **Configure service discovery service**, select to either create a new service discovery service or select an existing one\.

1. If creating a new service discovery service, for **Service discovery name** enter a descriptive name for your service discovery service\. This is used as the prefix for the DNS records to be created\.

1. Select **Enable ECS task health propagation** if you want health checks enabled for your service discovery service\.

1. For **DNS record type**, select the DNS record type to create for your service\. Amazon ECS service discovery only supports A and SRV records, depending on the network mode that your task definition specifies\. For more information about these record types, see [DnsRecord](http://docs.aws.amazon.com/Route53/latest/APIReference/API_autonaming_DnsRecord.html)\.
   + If the task definition that your service task specifies uses the `bridge` or `host` network mode, only type SRV records are supported\. Choose a container name and port combination to associate with the record\.
   + If the task definition that your service task specifies uses the `awsvpc` network mode, select either the A or SRV record type\. If the type A DNS record is selected, skip to the next step\. If the type SRV is selected, specify either the port that the service can be found on or a container name and port combination to associate with the record\.

1. For **TTL**, enter the resource record cache time to live \(TTL\), in seconds\. This value determines how long a record set is cached by DNS resolvers and by web browsers\.

1. Choose **Next step**\.

## \(Optional\) Configuring Your Service to Use Service Auto Scaling<a name="service-configure-auto-scaling"></a>

Your Amazon ECS service can optionally be configured to use Auto Scaling to adjust its desired count up or down in response to CloudWatch alarms\. 

Amazon ECS Service Auto Scaling supports the following types of scaling policies:
+ [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)—Increase or decrease  the number of tasks that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.
+ [Step Scaling Policies](service-autoscaling-stepscaling.md)—Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, which vary based on the size of the alarm breach\.

For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

**To configure basic Service Auto Scaling parameters**

1. If you have not done so already, follow the basic service creation procedures in [Configuring Basic Service Parameters](#basic-service-params)\.

1. On the **Set Auto Scaling** page, select **Configure Service Auto Scaling to adjust your service’s desired count**\.

1. For **Minimum number of tasks**, enter the lower limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted below this amount\.

1. For **Desired number of tasks**, this field is pre\-populated with the value that you entered earlier\. You can change your service's desired count at this time, but this value must be between the minimum and maximum number of tasks specified on this page\.

1. For **Maximum number of tasks**, enter the upper limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted above this amount\.

1. For **IAM role for Service Auto Scaling**, choose an IAM role to authorize the Application Auto Scaling service to adjust your service's desired count on your behalf\. If you have not previously created such a role, choose **Create new role** and the role is created for you\. For future reference, the role that is created for you is called `ecsAutoscaleRole`\. For more information, see [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

1. The following procedures provide steps for creating either target tracking or step scaling policies for your service\. Choose your desired scaling policy type\.

**To configure target tracking scaling policies for your service**

These steps help you create target tracking scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. You can create a scale\-out alarm to increase the desired count of your service, and a scale\-in alarm to decrease the desired count of your service\.

1. For **Scaling policy type**, choose **Target tracking**\.

1. For **Policy name**, enter a descriptive name for your policy\.

1. For **ECS service metric**, choose the metric you want to track\.

1. For **Target value**, enter the metric value you want the policy to maintain\.

1. For **Scale\-out cooldown period**, enter the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. During this time, resources that have been launched do not contribute to the Auto Scaling group metrics\.

1. For **Scale\-in cooldown period**, enter the amount of time, in seconds, after a scale in activity completes before another scale in activity can start\. During this time, resources that have been launched do not contribute to the Auto Scaling group metrics\.

1. \(Optional\) Choose **Disable scale\-in**, if you wish to disable the scale\-in actions for this policy\. This allows you to create a separate scaling policy for scale\-in later if you would like\.

1. Choose **Next step**\.

**To configure step scaling policies for your service**

These steps help you create step scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. You can create a **Scale out** alarm to increase the desired count of your service, and a **Scale in** alarm to decrease the desired count of your service\.

1. <a name="policy-name-step"></a>For **Scaling policy type**, choose **Step scaling**\.

1. For **Policy name**, enter a descriptive name for your policy\.

1. For **Execute policy when**, select the CloudWatch alarm that you want to use to scale your service up or down\.

   You can use an existing CloudWatch alarm that you have previously created, or you can choose to create a new alarm\. The **Create new alarm** workflow allows you to create CloudWatch alarms that are based on the `CPUUtilization` and `MemoryUtilization` of the service that you are creating\. To use other metrics, you can create your alarm in the CloudWatch console and then return to this wizard to choose that alarm\.

1. \(Optional\) If you've chosen to create a new alarm, complete the following steps\.

   1. For **Alarm name**, enter a descriptive name for your alarm\. For example, if your alarm should trigger when your service CPU utilization exceeds 75%, you could call the alarm `service_name-cpu-gt-75`\.

   1. For **ECS service metric**, choose the service metric to use for your alarm\. For more information about these service utilization metrics, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\.

   1. For **Alarm threshold**, enter the following information to configure your alarm:
      + Choose the CloudWatch statistic for your alarm \(the default value of **Average** works in many cases\)\. For more information, see [Statistics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistic) in the *Amazon CloudWatch User Guide*\.
      + Choose the comparison operator for your alarm and enter the value that the comparison operator checks against \(for example, `>` and `75`\)\.
      + Enter the number of consecutive periods before the alarm is triggered and the period length\. For example, two consecutive periods of 5 minutes would take 10 minutes before the alarm triggered\. Because your Amazon ECS tasks can scale up and down quickly, consider using a low number of consecutive periods and a short period duration to react to alarms as soon as possible\.

   1. Choose **Save** to save your alarm\.

1. <a name="scaling-action-step-adjustment"></a>For **Scaling action**, enter the following information to configure how your service responds to the alarm:
   + Choose whether to add to, subtract from, or set a specific desired count for your service\.
   + If you chose to add or subtract tasks, enter the number of tasks \(or percent of existing tasks\) to add or subtract when the scaling action is triggered\. If you chose to set the desired count, enter the desired count that your service should be set to when the scaling action is triggered\. 
   + \(Optional\) If you chose to add or subtract tasks, choose whether the previous value is used as an integer or a percent value of the existing desired count\.
   + Enter the lower boundary of your step scaling adjustment\. By default, for your first scaling action, this value is the metric amount where your alarm is triggered\. For example, the following scaling action adds 100% of the existing desired count when the CPU utilization is greater than 75%\.  
![\[Scaling activity example\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scaling-activity.png)

1. \(Optional\) You can repeat [Step 5](#scaling-action-step-adjustment) to configure multiple scaling actions for a single alarm \(for example, to add one task if CPU utilization is between 75\-85%, and to add two tasks if CPU utilization is greater than 85%\)\.

1. \(Optional\) If you chose to add or subtract a percentage of the existing desired count, enter a minimum increment value for **Add tasks in increments of *N* task\(s\)**\.

1. <a name="cooldown-period-step"></a>For **Cooldown period**, enter the number of seconds between scaling actions\.

1. Repeat [Step 1](#policy-name-step) through [Step 8](#cooldown-period-step) for the **Scale in** policy and choose **Save** to save your Service Auto Scaling configuration\.

1. Choose **Next step**\.

## Review and Create Your Service<a name="create-service-review"></a>

After you have configured your basic service definition parameters and optionally configured your service to use a load balancer, you can review your configuration\. Then, choose **Create Service** to finish creating your service\.

**Note**  
After you create a service, the target group ARN or load balancer name, container name, and container port specified in the service definition are immutable\. You cannot add, remove, or change the load balancer configuration of an existing service\. If you update the task definition for the service, the container name and container port that were specified when the service was created must remain in the task definition\.