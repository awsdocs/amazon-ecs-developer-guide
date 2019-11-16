# Account Settings<a name="ecs-account-settings"></a>

Amazon ECS provides the following account settings, which enable you to opt in or out of specific features\.

**Amazon Resource Names \(ARNs\) and IDS**  
Resource names: `serviceLongArnFormat`, `taskLongArnFormat`, and `containerInstanceLongArnFormat`  
Amazon ECS is introducing a new format for Amazon Resource Names \(ARNs\) and resource IDs for Amazon ECS services, tasks, and container instances\. You must opt in to the new format for each resource type to use features such as resource tagging for that resource type\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](#ecs-resource-ids)\.

**AWSVPC Trunking**  
Resource name: `awsvpcTrunking`  
Amazon ECS supports launching container instances with increased elastic network interface \(ENI\) density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. This configuration allows you to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI limit of ten\. The container instance will have a primary network interface and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. Neither the primary network interface or the trunk network interface counts against the ENI limit, so this configuration allows you to launch ten tasks on the container instance instead of the current two tasks\. For more information, see [Elastic Network Interface Trunking](container-instance-eni.md)\.

**CloudWatch Container Insights**  
Resource name: `containerInsights`  
CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. The metrics include utilization for resources such as CPU, memory, disk, and network\. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly\. You can also set CloudWatch alarms on metrics that Container Insights collects\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.  
When the `containerInsights` account setting is opted in, all new clusters created after opting in will have Container Insights enabled unless you disable it during cluster creation\. Individual clusters can either be enabled or disabled during creation or by using the UpdateClusterSettings API\.  
For clusters containing tasks or services using the EC2 launch type, your container instances must be running version 1\.29\.0 or later of the Amazon ECS agent to use Container Insights\. For more information, see [Amazon ECS Container Agent Versions](container_agent_versions.md)\.

For each Region, you can opt in to or opt out of each account setting at the account level or for a specific IAM user or role\. The available account settings to opt in to or out of include the new ARN and resource ID format and the awsvpc trunking feature\.

The following are supported scenarios:
+ An IAM user or role can opt in or opt out for their individual user account\.
+ An IAM user or role can set the default opt in or opt out setting for all users on the account\.
+ The root user has the ability to opt in to or opt out of any specific IAM role or user on the account\. If the account setting for the root user is changed, it sets the default for all the IAM users and roles for which no individual account setting has been selected\.

The opt in and opt out option must be selected for each account setting separately\. The ARN and resource ID format of a resource is defined by the opt\-in status of the IAM user or role that created the resource\.

Only resources launched after opting in receive the new ARN and resource ID format or the increased ENI limits\. All existing resources are not affected\. In order for Amazon ECS services and tasks to transition to the new ARN and resource ID formats, the service or task must be re\-created\. To transition a container instance to the new ARN and resource ID format or the increased ENI limits, the container instance must be drained and a new container instance registered to the cluster\.

**Note**  
Tasks launched by an Amazon ECS service can only receive the new ARN and resource ID format if the service was created on or after November 16, 2018, and the IAM user who created the service has opted in to the new format for tasks\.

**Topics**
+ [Amazon Resource Names \(ARNs\) and IDs](#ecs-resource-ids)
+ [Viewing Account Settings](ecs-viewing-longer-id-settings.md)
+ [Modifying Account Settings](ecs-modifying-longer-id-settings.md)

## Amazon Resource Names \(ARNs\) and IDs<a name="ecs-resource-ids"></a>

When Amazon ECS resources are created, each resource is assigned a unique Amazon Resource Name \(ARN\) and resource identifier \(ID\)\. If you are using a command line tool or the Amazon ECS API to work with Amazon ECS, resource ARNs or IDs are required for certain commands\. For example, if you are using the [stop\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/stop-task.html) AWS CLI command to stop a task, you must specify the task ARN or ID in the command\.

The ability to opt in to and opt out of the new Amazon Resource Name \(ARN\) and resource IDs is provided on a per\-Region basis\. Any new accounts created are opted out by default\.

You can opt in or opt out of the new Amazon Resource Name \(ARN\) and resource ID format at any time\. After you have opted in, any new resources that you create use the new format\.

**Note**  
A resource ID does not change after it's created\. Therefore, opting in or out of the new format does not affect your existing resource IDs\.

 The following sections describe how ARN and resource ID formats are changing\. For more information on the transition to the new formats, see [Amazon Elastic Container Service FAQ](https://aws.amazon.com/ecs/faqs/)\.

**Amazon Resource Name \(ARN\) Format**  
Some resources have a friendly name, such as a service named `production`\. In other cases, you must specify a resource using the Amazon Resource Name \(ARN\) format\. The new ARN format for Amazon ECS tasks, services, and container instances includes the cluster name\. For details about opting in to the new ARN format, see [Modifying Account Settings](ecs-modifying-longer-id-settings.md)\.

**Note**  
The new ARN format is not available in the GovCloud \(US\-East\) region\.

The following table shows both the current \(old\) format and the new format for each resource type\.


|  Resource Type  |  ARN  | 
| --- | --- | 
|  Container instance  |  Old: `arn:aws:ecs:region:aws_account_id:container-instance/container-instance-id` New: `arn:aws:ecs:region:aws_account_id:container-instance/cluster-name/container-instance-id`  | 
|  Amazon ECS service  |  Old: `arn:aws:ecs:region:aws_account_id:service/service-name` New: `arn:aws:ecs:region:aws_account_id:service/cluster-name/service-name`  | 
|  Amazon ECS task  |  Old: `arn:aws:ecs:region:aws_account_id:task/task-id` New: `arn:aws:ecs:region:aws_account_id:task/cluster-name/task-id`  | 

**Resource ID Length**  
A resource ID takes the form of a unique combination of letters and numbers\. New resource ID formats include shorter IDs for Amazon ECS tasks and container instances\. The old resource ID format was 36 characters long\. The new IDs are in a 32\-character format that does not include any hyphens\. For details about opting in to the new resource ID format, see [Modifying Account Settings](ecs-modifying-longer-id-settings.md)\.

**Note**  
The new resource ID format is not available in the GovCloud \(US\-East\) region\.