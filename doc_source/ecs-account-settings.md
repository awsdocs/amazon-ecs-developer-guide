# Account settings<a name="ecs-account-settings"></a>

You can go into Amazon ECS account settings to opt in or out of specific features\. For each AWS Region, you can opt in to, or opt out of, each account setting at the account\-level or for a specific user or role\.

You might want to opt in or out of specific features if any of the following is relevant to you:
+ A user or role can opt in or opt out specific account settings for their individual account\.
+ A user or role can set the default opt\-in or opt\-out setting for all users on the account\.
+ The root user can opt in to, or opt out of, any specific role or user on the account\. If the account setting for the root user is changed, it sets the default for all the users and roles that no individual account setting was selected for\.

**Note**  
Federated users assume the account setting of the root user and can't have explicit account settings set for them separately\.

The following account settings are available\. The opt\-in and opt\-out option must be selected for each account setting separately\.

**Amazon Resource Names \(ARNs\) and IDs**  
Resource names: `serviceLongArnFormat`, `taskLongArnFormat`, and `containerInstanceLongArnFormat`  
Amazon ECS is introducing a new format for Amazon Resource Names \(ARNs\) and resource IDs for Amazon ECS services, tasks, and container instances\. The opt\-in status for each resource type determines the Amazon Resource Name \(ARN\) format the resource uses\. You must opt in to the new ARN format to use features such as resource tagging for that resource type\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](#ecs-resource-ids)\.  
Only resources launched after opting in receive the new ARN and resource ID format\. All existing resources aren't affected\. For Amazon ECS services and tasks to transition to the new ARN and resource ID formats, you must recreate the service or task\. To transition a container instance to the new ARN and resource ID format, the container instance must be drained and a new container instance that's registered to the cluster\.  
Tasks launched by an Amazon ECS service can only receive the new ARN and resource ID format if the service was created on or after November 16, 2018, and the user who created the service has opted in to the new format for tasks\.

**AWSVPC trunking**  
Resource name: `awsvpcTrunking`  
Amazon ECS supports launching container instances with increased elastic network interface \(ENI\) density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. You can use this configuration to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI quota of ten\. The container instance has a primary network interface, and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. The primary network interface and the trunk network interface don't count against the ENI quota\. Therefore, you can use this configuration to launch ten tasks on the container instance instead of the current two tasks\. For more information, see [Elastic network interface trunking](container-instance-eni.md)\.  
Only resources launched after opting in receive the the increased ENI limits\. All the existing resources aren't affected\. To transition a container instance to the increased ENI quotas, the container instance must be drained and a new container instance registered to the cluster\.

**CloudWatch Container Insights**  
Resource name: `containerInsights`  
CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. The metrics include utilization for resources such as CPU, memory, disk, and network\. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly\. You can also set CloudWatch alarms on metrics that Container Insights collects\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.  
When you opt in to the `containerInsights` account setting, all new clusters have Container Insights enabled by default\. You can disable this setting for specific clusters when you create them\. You can also change this setting by using the UpdateClusterSettings API\.  
For clusters that contain tasks or services using the EC2 launch type, your container instances must run version 1\.29\.0 or later of the Amazon ECS agent to use Container Insights\. For more information, see [Amazon ECS Linux container agent versions](ecs-agent-versions.md)\.

**Dual\-stack VPC IPv6**  
Resource name: `dualStackIPv6`  
Amazon ECS supports providing tasks with an IPv6 address in addition to the primary private IPv4 address\.  
For tasks to receive an IPv6 address, the task must use the `awsvpc` network mode, must be launched in a VPC configured for dual\-stack mode, and the `dualStackIPv6` account setting must be enabled\. For more information about other requirements, see [Using a VPC in dual\-stack mode](task-networking-awsvpc.md#task-networking-vpc-dual-stack)\.  
The `dualStackIPv6` account setting can only be changed using either the Amazon ECS API or the AWS CLI\. For more information, see [Modifying account settings](ecs-modifying-longer-id-settings.md)\.
If you had a running task using the `awsvpc` network mode in an IPv6 enabled subnet between the dates of October 1, 2020 and November 2, 2020, the default `dualStackIPv6` account setting in the Region that the task was running in is `disabled`\. If that condition isn't met, the default `dualStackIPv6` setting in the Region is `enabled`\.

**Fargate FIPS\-140 compliance**  
Resource name: `fargateFIPSMode`  
Fargate supports the Federal Information Processing Standard \(FIPS\-140\) which specifies the security requirements for cryptographic modules that protect sensitive information\. It is the current United States and Canadian government standard, and is applicable to systems that are required to be compliant with Federal Information Security Management Act \(FISMA\) or Federal Risk and Authorization Management Program \(FedRAMP\)\.  
You must turn on FIPS\-140 compliance\. For more information, see [AWS Fargate Federal Information Processing Standard \(FIPS\-140\)](ecs-fips-compliance.md)\.

**Topics**
+ [Amazon Resource Names \(ARNs\) and IDs](#ecs-resource-ids)
+ [ARN and resource ID format timeline](#ecs-resource-arn-timeline)
+ [AWS Fargate Federal Information Processing Standard \(FIPS\-140\) compliance](#fips-setting)
+ [Viewing account settings using the console](ecs-viewing-longer-id-settings.md)
+ [Modifying account settings](ecs-modifying-longer-id-settings.md)
+ [Reverting to the default account settings](ecs-reverting-account.md)
+ [Account setting management using the AWS CLI](account-setting-management-cli.md)

## Amazon Resource Names \(ARNs\) and IDs<a name="ecs-resource-ids"></a>

When Amazon ECS resources are created, each resource is assigned a unique Amazon Resource Name \(ARN\) and resource identifier \(ID\)\. If you use a command line tool or the Amazon ECS API to work with Amazon ECS, resource ARNs or IDs are required for certain commands\. For example, if you use the [stop\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/stop-task.html) AWS CLI command to stop a task, you must specify the task ARN or ID in the command\.

You can opt in to and opt out of the new Amazon Resource Name \(ARN\) and resource ID format on a per\-Region basis\. Currently, any new account created is opted in by default\.

You can opt in or opt out of the new Amazon Resource Name \(ARN\) and resource ID format at any time\. After you opted in, any new resources that you create use the new format\.

**Note**  
A resource ID doesn't change after it's created\. Therefore, opting in or out of the new format doesn't affect your existing resource IDs\.

The following sections describe how ARN and resource ID formats are changing\. For more information about the transition to the new formats, see [Amazon Elastic Container Service FAQ](https://aws.amazon.com/ecs/faqs/)\.

**Amazon Resource Name \(ARN\) format**  
Some resources have a user\-friendly name, such as a service named `production`\. In other cases, you must specify a resource using the Amazon Resource Name \(ARN\) format\. The new ARN format for Amazon ECS tasks, services, and container instances includes the cluster name\. For information about opting in to the new ARN format, see [Modifying account settings](ecs-modifying-longer-id-settings.md)\.

The following table shows both the current format and the new format for each resource type\.


|  Resource type  |  ARN  | 
| --- | --- | 
|  Container instance  |  Current: `arn:aws:ecs:region:aws_account_id:container-instance/container-instance-id` New: `arn:aws:ecs:region:aws_account_id:container-instance/cluster-name/container-instance-id`  | 
|  Amazon ECS service  |  Current: `arn:aws:ecs:region:aws_account_id:service/service-name` New: `arn:aws:ecs:region:aws_account_id:service/cluster-name/service-name`  | 
|  Amazon ECS task  |  Current: `arn:aws:ecs:region:aws_account_id:task/task-id` New: `arn:aws:ecs:region:aws_account_id:task/cluster-name/task-id`  | 

**Resource ID length**  
A resource ID takes the form of a unique combination of letters and numbers\. New resource ID formats include shorter IDs for Amazon ECS tasks and container instances\. The current resource ID format is 36 characters long\. The new IDs are in a 32\-character format that doesn't include any hyphens\. For information about opting in to the new resource ID format, see [Modifying account settings](ecs-modifying-longer-id-settings.md)\.

## ARN and resource ID format timeline<a name="ecs-resource-arn-timeline"></a>

The timeline for the opt\-in and opt\-out periods for the new Amazon Resource Name \(ARN\) and resource ID format for Amazon ECS resources ended on April 1, 2021\. By default, all accounts are opted in to the new format\. All new resources created receive the new format, and you can no longer opt out\.

## AWS Fargate Federal Information Processing Standard \(FIPS\-140\) compliance<a name="fips-setting"></a>

You must turn on Federal Information Processing Standard \(FIPS\-140\) compliance on Fargate\. For more information, see [AWS Fargate Federal Information Processing Standard \(FIPS\-140\)](ecs-fips-compliance.md)\.

 Run `put-account-setting-default` with the `fargateFIPSMode` option set to `enabled`\. For more information, see, [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) in the *Amazon Elastic Container Service API Reference*\. 
+ Example to turn on FIPS\-140 compliance

  ```
  aws ecs put-account-setting-default --name fargateFIPSMode --value enabled
  ```

  Output

  ```
  {
      "setting": {
          "name": "fargateFIPSMode",
          "value": "enabled",
          "principalArn": "arn:aws:iam::123456789012:user"
      }
  }
  ```

You can run `list-account-settings` to view the current FIPS\-140 compliance status\. Use the `effective-settings` option to view the account level settings\.

```
aws ecs list-account-settings --effective-settings
```