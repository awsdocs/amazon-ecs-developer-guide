# Service auto scaling<a name="service-auto-scaling"></a>

*Automatic scaling* is the ability to increase or decrease the desired count of tasks in your Amazon ECS service automatically\. Amazon ECS leverages the Application Auto Scaling service to provide this functionality\. For more information, see the [Application Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)\.

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. For more information, see [Service utilization](cloudwatch-metrics.md#service_utilization)\. You can use these and other CloudWatch metrics to scale out your service \(add more tasks\) to deal with high demand at peak times, and to scale in your service \(run fewer tasks\) to reduce costs during periods of low utilization\. 

Amazon ECS Service Auto Scaling supports the following types of automatic scaling:
+ [Target tracking scaling policies](service-autoscaling-targettracking.md)— Increase or decrease the number of tasks that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.
+ [Step scaling policies](service-autoscaling-stepscaling.md)— Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach\. 
+ [Scheduled Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-scheduled-scaling.html)—Increase or decrease the number of tasks that your service runs based on the date and time\. 

## Service auto scaling and deployments<a name="service-auto-scaling-deployments"></a>

Application Auto Scaling turns off scale\-in processes while Amazon ECS deployments are in progress\. However, scale\-out processes continue to occur, unless suspended, during a deployment\. If you want to suspend scale\-out processes while deployments are in progress, take the following steps\.

1. Call the [describe\-scalable\-targets](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scalable-targets.html) command, specifying the resource ID of the service associated with the scalable target in Application Auto Scaling \(Example: `service/default/sample-webapp`\)\. Record the output\. You will need it when you call the next command\.

1. Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command, specifying the resource ID, namespace, and scalable dimension\. Specify `true` for both `DynamicScalingInSuspended` and `DynamicScalingOutSuspended`\.

1. After deployment is complete, you can call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to resume scaling\.

For more information, see [Suspending and resuming scaling for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-suspend-resume-scaling.html)\.

## IAM permissions required for service auto scaling<a name="auto-scaling-IAM"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. 

In addition to the standard IAM permissions for creating and updating services, you must give your users, groups, or roles permissions to interact with Service Auto Scaling settings as shown in the following example policy\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "application-autoscaling:*",
        "ecs:DescribeServices",
        "ecs:UpdateService",
        "cloudwatch:DescribeAlarms",
        "cloudwatch:PutMetricAlarm",
        "cloudwatch:DeleteAlarms",
        "cloudwatch:DescribeAlarmHistory",
        "cloudwatch:DescribeAlarmsForMetric",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "cloudwatch:DisableAlarmActions",
        "cloudwatch:EnableAlarmActions",
        "iam:CreateServiceLinkedRole",
        "sns:CreateTopic",
        "sns:Subscribe",
        "sns:Get*",
        "sns:List*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

The [Create service example](security_iam_id-based-policy-examples.md#IAM_create_service_policies) and [Update service example](security_iam_id-based-policy-examples.md#IAM_update_service_policies) IAM policy examples show the required permissions to use Service Auto Scaling in the AWS Management Console\.

The Application Auto Scaling service also needs permission to describe your Amazon ECS services and CloudWatch alarms, and permissions to modify your service's desired count on your behalf\. The `sns:` permissions are for the notifications that CloudWatch sends to an Amazon SNS topic when a threshold has been exceeded\. If you use automatic scaling for your Amazon ECS services, it creates a service\-linked role named `AWSServiceRoleForApplicationAutoScaling_ECSService`\. This service\-linked role grants Application Auto Scaling permission to describe the alarms for your policies, to monitor the current running task count of the service, and to modify the desired count of the service\. The original managed Amazon ECS role for Application Auto Scaling was `ecsAutoscaleRole`, but it is no longer required\. The service\-linked role is the default role for Application Auto Scaling\. For more information, see [Service\-linked roles for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\.

If you created your Amazon ECS container instance role before CloudWatch metrics are available for Amazon ECS, you might need to add the `ecs:StartTelemetrySession` permission\. For more information, see [Using CloudWatch metrics](cloudwatch-metrics.md#enable_cloudwatch)\.

## Considerations<a name="auto-scaling-concepts"></a>

When using scaling policies, consider the following:
+ Amazon ECS sends metrics in 1\-minute intervals to CloudWatch\. Metrics are not available until the clusters and services send the metrics to CloudWatch, and you cannot create CloudWatch alarms for metrics that do not exist\. 
+ The scaling policies support a cooldown period\. This is the number of seconds to wait for a previous scaling activity to take effect\. 
  + For scale\-out events, the intention is to continuously \(but not excessively\) scale out\. After Service Auto Scaling successfully scales out using a scaling policy, it starts to calculate the cooldown time\. The scaling policy won't increase the desired capacity again unless either a larger scale out is initiated or the cooldown period ends\. While the scale\-out cooldown period is in effect, the capacity added by the initiating scale\-out activity is calculated as part of the desired capacity for the next scale\-out activity\. 
  + For scale\-in events, the intention is to scale in conservatively to protect your application's availability, so scale\-in activities are blocked until the cooldown period has expired\. However, if another alarm initiates a scale\-out activity during the scale\-in cooldown period, Service Auto Scaling scales out the target immediately\. In this case, the scale\-in cooldown period stops and doesn't complete\. 
+ The service scheduler respects the desired count at all times, but as long as you have active scaling policies and alarms on a service, Service Auto Scaling could change a desired count that was manually set by you\.
+ If a service's desired count is set below its minimum capacity value, and an alarm triggers a scale\-out activity, Service Auto Scaling scales the desired count up to the minimum capacity value and then continues to scale out as required, based on the scaling policy associated with the alarm\. However, a scale\-in activity does not adjust the desired count, because it is already below the minimum capacity value\.
+ If a service's desired count is set above its maximum capacity value, and an alarm triggers a scale in activity, Service Auto Scaling scales the desired count out to the maximum capacity value and then continues to scale in as required, based on the scaling policy associated with the alarm\. However, a scale\-out activity does not adjust the desired count, because it is already above the maximum capacity value\.
+ During scaling activities, the actual running task count in a service is the value that Service Auto Scaling uses as its starting point, as opposed to the desired count\. This is what processing capacity is supposed to be\. This prevents excessive \(runaway\) scaling that might not be satisfied, for example, if there aren't enough container instance resources to place the additional tasks\. If the container instance capacity is available later, the pending scaling activity may succeed, and then further scaling activities can continue after the cooldown period\.
+ If you want your task count to scale to zero when there's no work to be done, set a minimum capacity of 0\. With target tracking scaling policies, when actual capacity is 0 and the metric indicates that there is workload demand, Service Auto Scaling waits for one data point to be sent before scaling out\. In this case, it scales out by the minimum possible amount as a starting point and then resumes scaling based on the actual running task count\.
+ Application Auto Scaling turns off scale\-in processes while Amazon ECS deployments are in progress\. However, scale\-out processes continue to occur, unless suspended, during a deployment\. For more information, see [Service auto scaling and deployments](#service-auto-scaling-deployments)\.

## AWS CLI and SDK experience<a name="service-auto-scaling-api"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. 

For more information about these specific API operations, see the [Amazon Elastic Container Service API Reference](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/), the [Amazon CloudWatch API Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/), and the [Application Auto Scaling API Reference](https://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/)\. For more information about the AWS CLI commands for these services, see the [ecs](https://docs.aws.amazon.com/cli/latest/reference/ecs), [cloudwatch](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch), and [application\-autoscaling](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling) sections of the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

**To configure scaling policies for your Amazon ECS service using the AWS CLI**

1. Register your Amazon ECS service as a scalable target using the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command\.

1. Create a scaling policy using the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command\.

1. \[Step scaling\] Create an alarm that triggers the scaling policy using the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command\.

For more information about configuring scaling policies using the AWS CLI, see the [Application Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/application/userguide/)\.