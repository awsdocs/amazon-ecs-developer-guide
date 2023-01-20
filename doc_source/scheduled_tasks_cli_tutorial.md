# Creating a scheduled task using the AWS CLI<a name="scheduled_tasks_cli_tutorial"></a>

This topic describes how to create a scheduled task using the AWS CLI\. The scheduled task is created using the CloudWatch Events API\. For more information, see [What is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.

Complete the following prerequisites:
+ Set up an AWS account and an *ecsEventsRole* associated with your account\.
+ Install and configure the AWS CLI version 2\. For more information, see [Installing the AWS CLI version 2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) and [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)\.
+ A registered task definition\. If you haven't yet created and registered a task definition, see [Getting started with the console using Linux containers on AWS Fargate](getting-started-fargate.md)\.
+ An Amazon EC2 Linux instance running on your default ECS cluster\. For instructions on how to create these resources\.

  Before you verify the scheduling results, make sure that the cluster isn't running a service or task\. From the ECS console, delete the cluster tasks and service before trying the example\.

**To create a scheduled task \(AWS CLI\)**

1. Create the CloudWatch Events rule\. This example creates a rule named `MyRule1` that's started every day at 12:00pm UTC\. You can change the time so that it's more convenient for verifying the schedule results\. The first time placeholder is minutes and the second placeholder is UTC hours\. For other examples of rule expressions, see [Schedule Expressions for Rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

   ```
   aws events put-rule \
        --schedule-expression "cron(0 12 * * ? *)" --name MyRule1
   ```

1. Add the details of your Amazon ECS cluster and task definition as a target for the CloudWatch Events rule\. Specify the cluster and task definition using the full Amazon Resource Name \(ARN\)\. The launch type and network configuration must be defined either in the task definition or the `put-targets` command line\. When using Fargate, the network configuration must be defined as `awsvpc`\.

   In this example, the target is defined as the `default` cluster in which to run a Fargate task based on the `first-run-task-definition:1` task definition\. A count of one task is scheduled to run according to `MyRule1`\. An `ecsEventsRole` IAM role is assigned to the target\. The launch type is `FARGATE` and the network configuration is defined as `awsvpc` with a security groups and a public subnet\. The command is run from the ECS instance in the default cluster\. For more information about `put-targets`, see [put\-targets](https://docs.aws.amazon.com/cli/latest/reference/events/put-targets.html)\. The cluster and task definition must already be created\. Otherwise, you receive an error\.

   Create a local file named `scheduledtask.json` with the following contents:

   ```
   [{
           "Id": "1",
           "Arn": "arn:aws:ecs:us-east-1:123456789012:cluster/default",
           "RoleArn": "arn:aws:iam::123456789012:role/ecsEventsRole",
           "EcsParameters": {
                   "TaskDefinitionArn": "arn:aws:ecs:us-east-1:123456789012:task-definition/first-run-task-definition:1",
                   "TaskCount": 1,
                   "LaunchType": "FARGATE",
                   "NetworkConfiguration": {
                           "awsvpcConfiguration": {
                                   "Subnets": ["subnet1"],
                                   "SecurityGroups": ["secgroup1"],
                                   "AssignPublicIp": "ENABLED"
                           }
                   },
                   "PlatformVersion": "LATEST"
           }
   }]
   ```

   Use the following command to create the target:

   ```
   aws events put-targets \
        --rule "MyRule1" \
        --targets file://scheduledtask.json
   ```