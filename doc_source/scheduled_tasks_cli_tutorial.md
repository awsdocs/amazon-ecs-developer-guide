# Creating a scheduled task using the AWS CLI<a name="scheduled_tasks_cli_tutorial"></a>

This topic shows you how to create a scheduled task using the AWS CLI\. The scheduled task creation uses the CloudWatch Events API\. For more information, see [What is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.

Complete the following prerequisites:
+ Set up an AWS account and an *ecsEventsRole* associated with your account\.
+ Install and configure the AWS CLI version 2\. For more information, see [Installing the AWS CLI version 2](https://docs.aws.amazon.com/https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) and [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)\.
+ A registered task definition\. If you haven't yet created and registered a task definition, see [Getting started with Amazon ECS using Fargate](getting-started-fargate.md)\.
+ An Amazon EC2 Linux instance running on your default ECS cluster\. If you need to create these resources, see [Getting started with Amazon ECS using Fargate](getting-started-fargate.md)\.

  To verify the scheduling results, the cluster should not be running a service or task\. From the ECS console, delete the cluster tasks and service before trying the example\.

**To create the scheduled task**

1. Create the CloudWatch Events rule\. This example creates a rule named `MyRule1` that is triggered every day at 12:00pm UTC\. You can change the time so that it is more convenient for verifying the schedule results\. The first time placeholder is minutes and the second placeholder is UTC hours\.

   ```
   aws events put-rule --schedule-expression "cron(0 12 * * ? *)" --name MyRule1
   ```
**Note**  
For other examples of rule expressions, see [Schedule Expressions for Rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

1. Add the details of your Amazon ECS cluster, EC2 instance and task definition as a target for the CloudWatch Events rule\. Specify the cluster and task definition using the full Amazon Resource Name \(ARN\)\. The launch type and network configuration must be defined either in the task definition or the `put-targets` command line\. The launch type defaults to `EC2`\. When using Fargate, the network configuration must be defined as `awsvpc`\.

   In this example, 1 target is defined as the `default` cluster in which to run a Fargate task based on the `first-run-task-definition:1` task definition that does not include network configuration or launch type defintions\. A count of 1 task is scheduled to run according to MyRule1\. An `ecsEventsRole` IAM role is assigned to the target\. The launch type is `FARGATE` and the network configuration is defined as `awsvpc` with a security groups and an enabled public subnet\. The command is run from the ECS instance in the default cluster\. For more information on `put-targets`, see [put\-targets](https://docs.aws.amazon.com/cli/latest/reference/events/put-targets.html)\. The cluster, EC2 instance and task definition must already be created\. Otherwise, you will receive an error\.

   ```
   aws events put-targets --rule "MyRule1" --targets "Id"="1","Arn"="arn:aws:ecs:us-east-1:123456789012:cluster/default","RoleArn"="arn:aws:iam::123456789012:role/ecsEventsRole","EcsParameters"="{"TaskDefinitionArn"= "arn:aws:ecs:us-east-1:123456789012:task-definition/first-run-task-definition:1","TaskCount"= 1}", "LaunchType"="FARGATE","NetworkConfiguration"="{"awsvpcConfiguration"="{"Subnets"="subnet ID","SecurityGroups"="security group ID","AssignPublicIp"="ENABLED"}"}"}"
   ```