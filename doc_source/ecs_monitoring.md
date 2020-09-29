# Monitoring Amazon ECS<a name="ecs_monitoring"></a>

You can monitor your Amazon ECS resources using Amazon CloudWatch, which collects and processes raw data from Amazon ECS into readable, near real\-time metrics\. These statistics are recorded for a period of two weeks, so that you can access historical information and gain a better perspective on how your clusters or services are performing\. Amazon ECS metric data is automatically sent to CloudWatch in 1\-minute periods\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon ECS and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. Before you start monitoring Amazon ECS; however, you should create a monitoring plan that includes answers to the following questions:
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

The metrics made available depend on the launch type of the tasks and services in your clusters\. If you are using the Fargate launch type for your services, then CPU and memory utilization metrics are provided to assist in the monitoring of your services\. For the Amazon EC2 launch type, you own and need to monitor the EC2 instances that make your underlying infrastructure\. Additional CPU and memory reservation and utilization metrics are made available at the cluster, service, and task level\.

The next step is to establish a baseline for normal Amazon ECS performance in your environment, by measuring performance at various times and under different load conditions\. As you monitor Amazon ECS, store historical monitoring data so that you can compare it with current performance data, identify normal performance patterns and performance anomalies, and devise methods to address issues\.

To establish a baseline you should, at a minimum, monitor the following items:
+ The CPU and memory and reservation utilization metrics for your Amazon ECS clusters
+ The CPU and memory utilization metrics for your Amazon ECS services

**Topics**
+ [Monitoring tools](monitoring-automated-manual.md)
+ [Amazon ECS CloudWatch metrics](cloudwatch-metrics.md)
+ [Amazon ECS events and EventBridge](cloudwatch_event_stream.md)
+ [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)
+ [Logging Amazon ECS API calls with AWS CloudTrail](logging-using-cloudtrail.md)