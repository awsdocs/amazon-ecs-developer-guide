# Logging and Monitoring in Amazon Elastic Container Service<a name="ecs-logging-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon Elastic Container Service and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides several tools for monitoring your Amazon ECS resources and responding to potential incidents:

**Amazon CloudWatch Alarms**  
Watch a single metric over a time period that you specify, and perform one or more actions based on the value of the metric relative to a given threshold over a number of time periods\. The action is a notification sent to an Amazon Simple Notification Service \(Amazon SNS\) topic or Amazon EC2 Auto Scaling policy\. CloudWatch alarms do not invoke actions simply because they are in a particular state; the state must have changed and been maintained for a specified number of periods\. For more information, see [Amazon ECS CloudWatch metrics](cloudwatch-metrics.md)\.  
For clusters with tasks or services using the EC2 launch type, you can use CloudWatch alarms to scale in and scale out the container instances based on CloudWatch metrics, such as cluster memory reservation\. For more information, see [Tutorial: Scaling container instances with CloudWatch alarms](cloudwatch_alarm_autoscaling.md)\.

**Amazon CloudWatch Logs**  
Monitor, store, and access the log files from the containers in your Amazon ECS tasks by specifying the `awslogs` log driver in your task definitions\. This is the only supported method for accessing logs for tasks using the Fargate launch type, but also works with tasks using the EC2 launch type\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.  
You can also monitor, store, and access the operating system and Amazon ECS container agent log files from your Amazon ECS container instances\. This method for accessing logs can be used for containers using the EC2 launch type\. For more information, see [Using CloudWatch Logs with container instances](using_cloudwatch_logs.md)\.

**Amazon CloudWatch Events**  
Match events and route them to one or more target functions or streams to make changes, capture state information, and take corrective action\. For more information, see [Amazon ECS events and EventBridge](cloudwatch_event_stream.md) in this guide and [What Is Amazon CloudWatch Events?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) in the *Amazon CloudWatch Events User Guide*\.

**AWS CloudTrail Logs**  
CloudTrail provides a record of actions taken by a user, role, or an AWS service in Amazon ECS\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon ECS, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Logging Amazon ECS API calls with AWS CloudTrail](logging-using-cloudtrail.md)\.

**AWS Trusted Advisor**  
Trusted Advisor draws upon best practices learned from serving hundreds of thousands of AWS customers\. Trusted Advisor inspects your AWS environment and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps\. All AWS customers have access to five Trusted Advisor checks\. Customers with a Business or Enterprise support plan can view all Trusted Advisor checks\.   
For more information, see [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html#trusted-advisor) in the *AWS Support User Guide*\.

Another important part of monitoring Amazon ECS involves manually monitoring those items that the CloudWatch alarms don't cover\. The CloudWatch, Trusted Advisor, and other AWS console dashboards provide an at\-a\-glance view of the state of your AWS environment\. We recommend that you also check the log files on your container instances and the containers in your tasks\.