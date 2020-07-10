# Using CloudWatch Logs with container instances<a name="using_cloudwatch_logs"></a>

You can configure your container instances to send log information to CloudWatch Logs\. This enables you to view different logs from your container instances in one convenient location\. This topic helps you get started using CloudWatch Logs on your container instances that were launched with the Amazon ECS\-optimized Amazon Linux AMI\.

For information about sending container logs from your tasks to CloudWatch Logs, see [Using the awslogs Log Driver](using_awslogs.md)\. For more information about CloudWatch Logs, see [Monitoring Log Files](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.

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

1. Choose `ecsInstanceRole`\. If the role does not exist, follow the procedures in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) to create the role\.

1. Choose **Permissions**, **Attach policies**\.

1. To narrow the available policies to attach, for **Filter**, type **ECS\-CloudWatchLogs**\.

1. Select the **ECS\-CloudWatchLogs** policy and choose **Attach policy**\.

## Installing and configuring the CloudWatch agent<a name="installing_cwl_agent"></a>

After you have added the `ECS-CloudWatchLogs` policy to your `ecsInstanceRole`, you can install the CloudWatch agent on your container instances\.

For more information, see [Download and configure the CloudWatch agent using the command line](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html) in the *Amazon CloudWatch User Guide*\.

## Viewing CloudWatch Logs<a name="viewing_cwlogs"></a>

After you have given your container instance role the proper permissions to send logs to CloudWatch Logs, and you have configured and started the agent, your container instance should be sending its log data to CloudWatch Logs\. You can view and search these logs in the AWS Management Console\.

**Note**  
New instance launches may take a few minutes to send data to CloudWatch Logs\.

**To view your CloudWatch Logs data**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Logs**, **Log groups**\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cwl-log-groups.png)

1. Choose a log group to view\.

1. Choose a log stream to view\. The streams are identified by the cluster name and container instance ID that sent the logs\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cw_log_stream.png)