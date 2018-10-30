# Creating a Scheduled Task Using the AWS CLI<a name="scheduled_tasks_cli_tutorial"></a>

This topic shows you how to create a scheduled task using the AWS CLI\. The scheduled task creation uses the CloudWatch Events API\. For more information, see [What is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.

Complete the following prerequisites:
+ Set up an AWS account\.
+ Install and configure the AWS CLI\. For more information, see [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html)\.

**To create the scheduled task**

1. Create the CloudWatch Events rule\. This example creates a rule named `MyRule1` that is triggered every day at 12:00pm UTC\.

   ```
   aws events put-rule --schedule-expression "cron(0 12 * * ? *)" --name MyRule1
   ```
**Note**  
For other examples of rule expressions, see [Schedule Expressions for Rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html) in the *Amazon CloudWatch Events User Guide*\.

1. Add the details of your ECS cluster and task definition as a target for the CloudWatch Events rule\. Specify the cluster and task definition using the full ARN\.

   This example defines the target for `MyRule1` as the `first-run-task-definition:1` task definition in the `default` cluster and assigns the `ecsEventsRole` IAM role to it\. It requests that `1` task be scheduled\. The cluster and task definition must already be created\. Otherwise, you receive an error\.

   ```
   aws events put-targets --rule "MyRule1" --targets "Id"="1","Arn"="arn:aws:ecs:us-east-1:123456789012:cluster/default","RoleArn"="arn:aws:iam::123456789012:role/ecsEventsRole","EcsParameters"="{"TaskDefinitionArn"= "arn:aws:ecs:us-east-1:123456789012:task-definition/first-run-task-definition:1","TaskCount"= 1}"
   ```