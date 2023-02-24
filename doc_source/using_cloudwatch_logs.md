# Monitoring your container instances<a name="using_cloudwatch_logs"></a>

You can configure your container instances to send log information to CloudWatch Logs\. This allows you to view different logs from your container instances in one convenient location\. This topic helps you get started using CloudWatch Logs on your container instances that were launched with the Amazon ECS\-optimized Amazon Linux AMI\.

For information about sending container logs from your tasks to CloudWatch Logs, see [Using the awslogs log driver](using_awslogs.md)\. For more information about CloudWatch Logs, see [Monitoring Log Files](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.

**Topics**
+ [CloudWatch Logs IAM Policy](#cwl_iam_policy)
+ [Installing and configuring the CloudWatch agent](#installing_cwl_agent)
+ [Viewing CloudWatch Logs](#viewing_cwlogs)

## CloudWatch Logs IAM Policy<a name="cwl_iam_policy"></a>

Before your container instances can send log data to CloudWatch Logs, you must create an IAM policy to allow your container instances to use the CloudWatch Logs APIs, and then you must attach that policy to `ecsInstanceRole`\.

**To create the `ECS-CloudWatchLogs` IAM policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. Choose **Create policy**, **JSON**\.

1. Enter the following policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "logs:DescribeLogStreams"
               ],
               "Resource": [
                   "arn:aws:logs:*:*:*"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, enter `ECS-CloudWatchLogs` for the **Name** and choose **Create policy**\.

**To attach the `ECS-CloudWatchLogs` policy to `ecsInstanceRole`**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Choose `ecsInstanceRole`\. If the role does not exist, follow the procedures in [Amazon ECS container instance IAM role](instance_IAM_role.md) to create the role\.

1. In the navigation pane, choose **Policies**\. 

1. Choose **ECS\-CloudWatchLogs**\.

1. Choose **Policy actions**, **Attach**\.

1. To narrow the available policies to attach, for **Filter**, type **ecsInstance**\.

1. Select the **ecsInstance** role and choose **Attach policy**\.

## Installing and configuring the CloudWatch agent<a name="installing_cwl_agent"></a>

After you have added the `ECS-CloudWatchLogs` policy to your `ecsInstanceRole`, you can install the CloudWatch agent on your container instances\.

For more information, see [Download and configure the CloudWatch agent using the command line](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html) in the *Amazon CloudWatch User Guide*\.

## Viewing CloudWatch Logs<a name="viewing_cwlogs"></a>

After you have given your container instance role the proper permissions to send logs to CloudWatch Logs, and you have configured and started the agent, your container instance should be sending its log data to CloudWatch Logs\. For information about how to view the logs, see [View log data sent to CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#ViewingLogData) in the *Amazon CloudWatch Logs User Guide*\.

**Note**  
New instance launches may take a few minutes to send data to CloudWatch Logs\.