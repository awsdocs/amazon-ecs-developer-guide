# Step Scaling Policies<a name="service-autoscaling-stepscaling"></a>

Although Amazon ECS Service Auto Scaling supports using Application Auto Scaling step scaling policies, we recommend using target tracking scaling policies instead\. For example, if you want to scale your service when CPU utilization falls below or rises above a certain level, create a target tracking scaling policy based on the CPU utilization metric provided by Amazon ECS\. For more information, see [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)\.

With step scaling policies, you create and manage the CloudWatch alarms that trigger the scaling process\. If the target tracking alarms don't work for your use case, you can use step scaling\. You can also use target tracking scaling with step scaling for an advanced scaling policy configuration\. For example, you can configure a more aggressive response when utilization reaches a certain level\. 

## Service Auto Scaling Considerations<a name="auto-scaling-concepts"></a>
+ Metrics are not available until the clusters and services send the metrics to CloudWatch, and you cannot create CloudWatch alarms for metrics that do not exist yet\.
+ The scaling policies that you create for Amazon ECS services support a cooldown period\. This is the number of seconds after a scaling activity completes where previous scaling policy\-related scaling activities can influence future scaling activities\. 
  + For scale\-out policies, while the cooldown period is in effect, the capacity that has been added by the previous scale\-out activity that initiated the cooldown is calculated as part of the desired capacity for the next scale out\. The intention is to continuously \(but not excessively\) scale out\. 
  + For scale\-in policies, the cooldown period is used to block subsequent scale in requests until it has expired\. The intention is to scale in conservatively to protect your application's availability\. However, if another alarm triggers a scale\-out policy during the cooldown period after a scale in, automatic scaling scales out your service immediately\. 
+ The ECS service scheduler respects the desired count at all times, but as long as you have active scaling policies and alarms on a service, Service Auto Scaling could change a desired count that was manually set by you\.
+ If a service's desired count is set below its minimum capacity value, and an alarm triggers a scale\-out activity, Service Auto Scaling scales the desired count up to the minimum capacity value and then continues to scale out as required, based on the scaling policy associated with the alarm\. However, a scale\-in activity does not adjust the desired count, because it is already below the minimum capacity value\.
+ If a service's desired count is set above its maximum capacity value, and an alarm triggers a scale in activity, Service Auto Scaling scales the desired count out to the maximum capacity value and then continues to scale in as required, based on the scaling policy associated with the alarm\. However, a scale\-out activity does not adjust the desired count, because it is already above the maximum capacity value\.
+ During scaling activities, the actual running task count in a service is the value that Service Auto Scaling uses as its starting point, as opposed to the desired count, which is what processing capacity is supposed to be\. This prevents excessive \(runaway\) scaling that could not be satisfied, for example, if there are not enough container instance resources to place the additional tasks\. If the container instance capacity is available later, the pending scaling activity may succeed, and then further scaling activities can continue after the cooldown period\.

## Amazon ECS Console Experience<a name="service-auto-scaling-console"></a>

Service Auto Scaling is disabled by default\. You can enable it by configuring scaling policies from the **Auto Scaling** tab of your services in the AWS Management Console for Amazon ECS\. 

For step\-by\-step guidance for working with scaling policies from the console, see [Creating a service](create-service.md) and [Updating a service](update-service.md)\. For more information about step scaling and a walkthrough, see [Automatic Scaling with Amazon ECS](http://aws.amazon.com/blogs/compute/automatic-scaling-with-amazon-ecs/) in the *AWS Compute Blog*\. For a target tracking walkthrough, see [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)\.

When you configure scaling policies for a service in the Amazon ECS console, your service is automatically registered as a scalable target with Application Auto Scaling, and your scaling policies are automatically in force as soon as they're successfully created\. 

## AWS CLI and SDK Experience<a name="service-auto-scaling-api"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. 

For more information about these specific API operations, see the [Amazon Elastic Container Service API Reference](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/), the [Amazon CloudWatch API Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/), and the [Application Auto Scaling API Reference](https://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/)\. For more information about the AWS CLI commands for these services, see the [ecs](https://docs.aws.amazon.com/cli/latest/reference/ecs), [cloudwatch](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch), and [application\-autoscaling](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling) sections of the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

**To configure scaling policies for your ECS service using the AWS CLI**

1. Register your ECS service as a scalable target using the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command\.

1. Create a scaling policy using the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command\.

1. \[Step scaling\] Create an alarm that triggers the scaling policy using the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command\.

For more information about configuring scaling policies using the AWS CLI, see the [Application Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/application/userguide/)\.