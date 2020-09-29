# Monitoring tools<a name="monitoring-automated-manual"></a>

AWS provides various tools that you can use to monitor Amazon ECS\. You can configure some of these tools to do the monitoring for you, while some of the tools require manual intervention\. We recommend that you automate monitoring tasks as much as possible\.

## Automated monitoring tools<a name="monitoring-automated_tools"></a>

You can use the following automated monitoring tools to watch Amazon ECS and report when something is wrong:
+ Amazon CloudWatch alarms – Watch a single metric over a time period that you specify, and perform one or more actions based on the value of the metric relative to a given threshold over a number of time periods\. The action is a notification sent to an Amazon Simple Notification Service \(Amazon SNS\) topic or Amazon EC2 Auto Scaling policy\. CloudWatch alarms do not invoke actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\. For more information, see [Amazon ECS CloudWatch metrics](cloudwatch-metrics.md)\.

  For clusters with tasks or services using the EC2 launch type, you can use CloudWatch alarms to scale in and scale out the container instances based on CloudWatch metrics, such as cluster memory reservation\. For more information, see [Tutorial: Scaling container instances with CloudWatch alarms](cloudwatch_alarm_autoscaling.md)\.
+ Amazon CloudWatch Logs – Monitor, store, and access the log files from the containers in your Amazon ECS tasks by specifying the `awslogs` log driver in your task definitions\. This is the only supported method for accessing logs for tasks using the Fargate launch type, but also works with tasks using the EC2 launch type\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.

  You can also monitor, store, and access the operating system and Amazon ECS container agent log files from your Amazon ECS container instances\. This method for accessing logs can be used for containers using the EC2 launch type\. For more information, see [Using CloudWatch Logs with container instances](using_cloudwatch_logs.md)\. 
+ Amazon CloudWatch Events – Match events and route them to one or more target functions or streams to make changes, capture state information, and take corrective action\. For more information, see [Amazon ECS events and EventBridge](cloudwatch_event_stream.md) in this guide and [What Is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.
+ AWS CloudTrail log monitoring – Share log files between accounts, monitor CloudTrail log files in real time by sending them to CloudWatch Logs, write log processing applications in Java, and validate that your log files have not changed after delivery by CloudTrail\. For more information, see [Logging Amazon ECS API calls with AWS CloudTrail](logging-using-cloudtrail.md) in this guide, and [Working with CloudTrail Log Files](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-working-with-log-files.html) in the *AWS CloudTrail User Guide*\. 

## Manual monitoring tools<a name="monitoring-manual-tools"></a>

Another important part of monitoring Amazon ECS involves manually monitoring those items that the CloudWatch alarms don't cover\. The CloudWatch, Trusted Advisor, and other AWS console dashboards provide an at\-a\-glance view of the state of your AWS environment\. We recommend that you also check the log files on your container instances and the containers in your tasks\.
+ CloudWatch home page: 
  + Current alarms and status
  + Graphs of alarms and resources
  + Service health status

  In addition, you can use CloudWatch to do the following: 
  + Create [customized dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) to monitor the services you care about\.
  + Graph metric data to troubleshoot issues and discover trends\.
  + Search and browse all your AWS resource metrics\.
  + Create and edit alarms to be notified of problems\.
+ AWS Trusted Advisor can help you monitor your AWS resources to improve performance, reliability, security, and cost effectiveness\. Four Trusted Advisor checks are available to all users; more than 50 checks are available to users with a Business or Enterprise support plan\. For more information, see [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/)\.