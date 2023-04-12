# New Amazon ECS console<a name="new-console"></a>

Welcome to the new and improved console experience—a quicker and easier way to deploy containerized applications, configure load balancing, networking, monitoring, and gives you the new workflows for the effective operations and troubleshooting\. 

While most of the classic console functionality is available in the new console, it is still a work in progress\. We also want to give customers time learn and migrate to the new console\. Therefore the classic console will remain available to opt\-in and you can toggle back to the classic console for the duration of your login session\.

## Configuration which is not available in the new console<a name="classic-console-use-cases"></a>

We’re working continuously to improve the experience\. The sections below describe when you need to use the classic console\. There will be many more functionality added to the new console\.

### First run wizard<a name="classic-console-first-run"></a>

Although the first run wizard is not available, we have provided instructions on how to get started using the new console\. For more information, see [Getting started with Amazon ECS](getting-started.md)\.

### Clusters<a name="classic-console-clusters"></a>

You can use the AWS CLI to configure the following cluster parameters:

For information about how to create a cluster using the AWS CLI, see [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) in the *AWS Command Line Interface Reference*\.
+ **Spot Instances** \- For the EC2 launch type, using Spot Instances for your cluster Auto Scaling group
+ **Creating a new VPC on cluster creation** \- You must use existing VPCs and subnets when you create a cluster

### Task definitions<a name="classic-console-task-def"></a>

You can use the JSON editor in the new console, or the AWS CLI to configure the following task definition parameters:

For information about how to use the JSON editor, see [Creating a task definition using the console](create-task-definition.md)\.

For information about how to register the task definition using the AWS CLI, see [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/ecs/register-task-definition.html) in the *AWS Command Line Interface Reference*\.
+ **Docker labels**
+ **AWS App Mesh integration**
+ **FireLens customization**
+ **Elastic inference**
+ **Task placement constraints**
+ **Update a service with the new task definition revision**
+ **Network modes** \- The following network modes are not supported:
  + **host**
  + **none**
+ **Container parameters** The following container parameters are not supported:
  + **Startup and dependency ordering**
  + **Individual container log configuration** Monitoring and logging has replaced this option
  + **Timeouts, ulimits, and network settings**

In addition only the CloudWatch log driver is supported\.

### Tasks<a name="classic-console-tasks"></a>

You can use the EventBridge console or the AWS CLI to configure scheduled tasks:

For information about how to use the EventBridge Scheduler console, see [Scheduled tasks](scheduled_tasks.md)\.
+ **Scheduling tasks** \- You cannot create, or update scheduled tasks\.
+ **Run more like this**

### Services<a name="classic-console-services"></a>

You must use AWS CloudFormation or the AWS Command Line Interface to deploy a service that uses any of the following parameters:

For information about how to create a service using the AWS CLI, see [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html) in the *AWS Command Line Interface Reference*\.

For information about how to create a service using AWS CloudFormation, see [AWS::ECS::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ecs-service.html) in the *AWS CloudFormation User Guide*\.
+ **Service Discovery** \- You can only view your Service Discovery configuration\. 
+ **Step scaling policies for service auto scaling**
+ **Tracking policy with a custom metric** \- You must use AWS CloudFormation or the AWS Command Line Interface to deploy the service
+ **Update Service** \- You cannot update the `awsvpc` network configuration and the health check grace period\.

### Container instances<a name="classic-console-instances"></a>

You can use the AWS CLI to view or edit custom metadata for your container instance\.

For more information, see [Adding an attribute using the classic console](task-placement-constraints.md#add-attribute) and [Filtering by attribute using the console](task-placement-constraints.md#filter-attribute)\.
+ **Instance attributes** \- You cannot view or edit custom metadata for your container instance

## List view enhancements<a name="display-enhancements"></a>

The console provides list views for your resources such as cluster, services, and task definitions\. There are also detail views for items such as networking, container instances, and deployments\.

The information is provided in a table view which allows you to adjust the width of each column\. The console tracks the width you set, so there is no need to adjust the width when you leave and return to the page\.

You can use the gear icon that is in the top\-right to control the page or table layout, including:
+ The number of items per page\.
+ The fields that are visible\.
+ Where applicable, the line wrap option\.
+ Where applicable, the timestamp format\. The default is the absolute time in the local format\. The following options are also available:
  + Absolute time in the ISO format
  + Relative time