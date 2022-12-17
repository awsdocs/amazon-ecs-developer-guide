# Creating a service using the new console<a name="create-service-console-v2"></a>

You can create an Amazon ECS service using the new console\. 

Consider the following when you use the new console;
+ Currently, the new console supports only the **Rolling update** \(`ECS`\) deployment type\. To use any other deployment type, switch to the classic console\.
+ Currently, the new console supports only the **Target tracking** scaling policy\. To use step scaling, switch to the classic console\.
+ There are two compute options that distribute your tasks\.
  + A **capacity provider strategy** causes Amazon ECS to distribute your tasks in one or across multiple capacity providers\. 
  + A **launch type** causes Amazon ECS to launch our tasks directly on either Fargate or on the Amazon EC2 instances that you have manually registered to your clusters\.
+ Task definitions that use the `awsvpc` network mode or services configured to use a load balancer must have a networking configuration\. By default, the console selects the default Amazon VPC along with all subnets and the default security group within the default Amazon VPC\. 
+ The default the task placement strategy distributes tasks evenly across Availability Zones\. 
+ When you use the **Launch Type** for your service deployment, by default the service starts in the subnets in your cluster VPC\.
+ For the **capacity provider strategy**, the console selects a compute option by default\. The following describes the order that the console uses to select a default:
  + If your cluster has a default capacity provider strategy defined, it is selected\.
  + If your cluster doesn't have a default capacity provider strategy defined but you do have the Fargate capacity providers added to the cluster, a custom capacity provider strategy that uses the `FARGATE` capacity provider is selected\.
  + If your cluster doesn't have a default capacity provider strategy defined but you do have one or more Auto Scaling group capacity providers added to the cluster, the **Use custom \(Advanced\)** option is selected and you need to manually define the strategy\.
  + If your cluster doesn't have a default capacity provider strategy defined and no capacity providers added to the cluster, the Fargate launch type is selected\.

## Quickly create a service<a name="create-default-service"></a>

You can use the new console to quickly create and deploy a service\. The service has the following configuration:
+ Deploys in the VPC and subnets associated with your cluster
+ Deploys one task
+ Uses the rolling deployment
+ Uses the capacity provider strategy with your default capacity provider
+ Uses the deployment circuit breaker to detect failures and sets the option to automatically roll back the deployment on failure

To deploy a service using the default parameters follow these steps\.

**To create a service \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster to create the service in\.

1. From the **Services** tab, choose **Create**\.

1. Under **Deployment configuration**, specify how your application is deployed\.

   1. For **Application type**, choose **Service**\.

   1. For **Task definition**, choose the task definition family and revision to use\.

   1. For **Service name**, enter a name for your service\.

   1. For **Desired tasks**, enter the number of tasks to launch and maintain in the service\.

1. \(Optional\) To add tags in the form of key\-value pairs, to the service, expand the **Tags** section\.

## Create a service using defined parameters<a name="create-custom-service"></a>

To create a service using defined parameters, follow these steps\.

**To create a service \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. Determine the resource from where you launch the service\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service-console-v2.html)

1. \(Optional\) Choose how your tasks are distributed across your cluster infrastructure\. Expand **Compute configuration**, and then choose your option\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service-console-v2.html)

1. To specify how your service is deployed, expand **Deployment configuration**, and then choose your options\.

   1. For **Application type**, choose **Service**\.

   1. For **Task definition** and **Revision**, choose the task definition family and revision to use\.

   1. For **Service name**, enter a name for your service\.

   1. For **Service type**, choose the service scheduling strategy\.
      + To have the scheduler deploy exactly one task on each active container instance that meets all of the task placement constraints, choose **Daemon**\.
      + To have the scheduler place and maintain the desired number of tasks in your cluster, choose **Replica**\.

      For more information, see [Service scheduler concepts](ecs_services.md#service_scheduler)\.

   1. If you chose **Replica**, for **Desired tasks**, enter the number of tasks to launch and maintain in the service\.

   1. To change the minimum healthy percent and maximum percent of running tasks allowed during a service deployment, expand **Deployment options**, and then specify the following parameters\.

      1. For **Min running tasks**, enter the lower limit on the number of tasks in the service that must remain in the `RUNNING` state during a deployment, as a percentage of the desired number of tasks \(rounded up to the nearest integer\)\. For more information, see [Deployment configuration](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service_definition_parameters.html#sd-deploymentconfiguration)\.

      1. For **Max running tasks**, enter the upper limit on the number of tasks in the service that are allowed in the `RUNNING` or `PENDING` state during a deployment, as a percentage of the desired number of tasks \(rounded down to the nearest integer\)\.

   1. To configure how Amazon ECS detects and handles deployment failures, expand **Deployment failure detection**, and then choose your options\. 

      1. To stop a deployment when the tasks cannot start, select **Use the Amazon ECS deployment circuit breaker**\.

         To have the software automatically roll back the deployment to the last completed deployment state when the deployment circuit breaker sets the deployment to a failed state, select **Rollback on failure**\.

      1. To stop a deployment based on application metrics\., select **Use CloudWatch alarms**\. Then, from **CloudWatch alarm names**, choose the alarms\. To create a new alarm, choose **Create new alarm**\.

         To have the software automatically roll back the deployment to the last completed deployment state when a CloudWatch alarm sets the deployment to a failed state, select **Rollback on failure**\.

1. To configure service auto scaling, expand **Service auto scaling**, and then specify the following parameters\.

   1. To use service auto scaling, select **Service auto scaling**\.

   1. For **Minimum number of tasks**, enter the lower limit of the number of tasks for Service Auto Scaling to use\. The desired count will not go below this count\.

   1. For **Maximum number of tasks**, enter the upper limit of the number of tasks for Service Auto Scaling to use\. The desired count will not go above this count\.

   1. For **Scaling policy type**, choose **Target tracking**\.

   1. For **Policy name**, enter the name of the policy\.

   1. For **ECS service metric**, select one of the following metrics:
      + **ECSServiceAverageCPUUtilization**: Average CPU utilization of the service\. 
      + **ECSServiceAverageMemoryUtilization**: Average memory utilization of the service\. 
      + **ALBRequestCountPerTarget**: Number of requests completed per target in an Application Load Balancer target group\. 

        The metrics require an Application Load Balancer and a target group for the Application Load Balancer\.

   1. For **Target value**, enter the value in percent that the service maintains for the selected metric\.

   1. For **Scale\-out cooldown period**, enter the time in seconds after a scale\-out activity that no other scale outs can happen\.

   1. For **Scale\-in cooldown period**, enter the time in seconds after a scale\-in activity that no other scale ins can happen\.

   1. To prevent the policy from performing a scale\-in activity, select **Turn off scale\-in**\.

1. \(Optional\) To use Service Connect, select **Turn on Service Connect**, and then specify the following:

   1. Under** Service Connect configuration**, specify the client mode\.
      + If your service runs s network client application that only needs to connect to other services in the namespace, choose **Client side only**\.
      + If your service runs a network or web service application and needs to provide endpoints for this service, and connects to other services in the namespace, choose **Client and server**\.

   1. To use a namespace that is not the default cluster namespace, for **Namespace**, choose the service namespace\.

   1. \(Optional\) Select the **Use log collection** option to specify a log configuration\. For each available log driver, there are log driver options to specify\. The default option sends container logs to CloudWatch Logs\. The other log driver options are configured using AWS FireLens\. For more information, see [Custom log routing](using_firelens.md)\.

      The following describes each container log destination in more detail\.
      + **Amazon CloudWatch** — Configure the task to send container logs to CloudWatch Logs\. The default log driver options are provided which creates a CloudWatch log group on your behalf\. To specify a different log group name, change the driver option values\.
      + **Amazon Kinesis Data Firehose** — Configure the task to send container logs to Kinesis Data Firehose\. The default log driver options are provided which sends logs to an Kinesis Data Firehose delivery stream\. To specify a different delivery stream name, change the driver option values\.
      + **Amazon Kinesis Data Streams** — Configure the task to send container logs to Kinesis Data Streams\. The default log driver options are provided which sends logs to an Kinesis Data Streams stream\. To specify a different stream name, change the driver option values\.
      + **Amazon OpenSearch Service** — Configure the task to send container logs to an OpenSearch Service domain\. The log driver options must be provided\. For more information, see [Forwarding logs to an Amazon OpenSearch Service domain](firelens-example-taskdefs.md#firelens-example-opensearch)\.
      + **Amazon S3** — Configure the task to send container logs to an Amazon S3 bucket\. The default log driver options are provided but you must specify a valid Amazon S3 bucket name\.

1. \(Optional\) To configure a load balancer for your service, expand **Load balancing**\.

   Choose the load balancer\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service-console-v2.html)

1. \(Optional\) To use a task placement strategy other than the default, expand **Task Placement**, and then choose from the following options\.

    For more information, see [Amazon ECS task placement](task-placement.md)\.
   + **AZ Balanced Spread** \- Distribute tasks across Availability Zones and across container instances in the Availability Zone\.
   + **AZ Balanced BinPack** \- Distribute tasks across Availability Zones and across container instances with the least available memory\.
   + **BinPack** \- Distribute tasks based on the least available amount of CPU or memory\.
   + **One Task Per Host** \- Place, at most, one task from the service on each container instance\.
   + **Custom** \- Define your own task placement strategy\. 

   If you chose **Custom**, define the algorithm for placing tasks and the rules that are considered during task placement\.
   + Under **Strategy**, for **Type** and **Field**, choose the algorithm and the entity to use for the algorithm\.

     You can enter a maximum of 5 strategies\.
   + Under **Constraint**, for **Type** and **Expression**, choose the rule and attribute to use for the constraint\.

     When you enter the **Expression**, do not enter the double quotation marks \(`" "`\)\. For example, to set the constraint to place tasks on T2 instances, for the **Expression**, enter **attribute:ecs\.instance\-type =\~ t2\.\***\.

     You can enter a maximum of 10 constraints\.

1. If your task definition uses the `awsvpc` network mode, expand **Networking**\. Use the following steps to specify a custom configuration\.

   1. For **VPC**, select the VPC to use\.

   1. For **Subnets**, select one or more subnets in the VPC that the task scheduler considers when placing your tasks\.
**Important**  
Only private subnets are supported for the `awsvpc` network mode\. Tasks don't receive public IP addresses\. Therefore, a NAT gateway is required for outbound internet access, and inbound internet traffic is routed through a load balancer\.

   1. For **Security group**, you can either select an existing security group or create a new one\. To use an existing security group, select the security group and move to the next step\. To create a new security group, choose **Create a new security group**\. You must specify a security group name, description, and then add one or more inbound rules for the security group\.

1. \(Optional\) To help identify your service and tasks, expand the **Tags** section, and then configure your tags\.

   To have Amazon ECS automatically tag all newly launched tasks with the cluster name and the task definition tags, select **Turn on Amazon ECS managed tags**, and then select **Task definitions**\.

   To have Amazon ECS automatically tag all newly launched tasks with the cluster name and the service tags, select **Turn on Amazon ECS managed tags**, and then select **Service**\.

   Add or remove a tag\.
   + \[Add a tag\] Choose **Add tag**, and then do the following:
     + For **Key**, enter the key name\.
     + For **Value**, enter the key value\.
   + \[Remove a tag\] Next to the tag, choose **Remove tag**\.