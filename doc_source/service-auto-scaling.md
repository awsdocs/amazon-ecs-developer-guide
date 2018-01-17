# Service Auto Scaling<a name="service-auto-scaling"></a>

Your Amazon ECS service can optionally be configured to use Service Auto Scaling to adjust its desired count up or down in response to CloudWatch alarms\. Service Auto Scaling is available in all regions that support Amazon ECS\.

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. You can use these service utilization metrics to scale your service up to deal with high demand at peak times, and to scale your service down to reduce costs during periods of low utilization\. For more information, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\.

You can also use CloudWatch metrics published by other services, or custom metrics that are specific to your application\. For example, a web service could increase the number of tasks based on Elastic Load Balancing metrics such as `SurgeQueueLength`, and a batch job could increase the number of tasks based on Amazon SQS metrics like `ApproximateNumberOfMessagesVisible`\.

You can also use Service Auto Scaling in conjunction with Auto Scaling for Amazon EC2 on your Amazon ECS cluster to scale your cluster, and your service, as a result to the demand\. For more information, see [Tutorial: Scaling Container Instances with CloudWatch Alarms](cloudwatch_alarm_autoscaling.md)\.

## Service Auto Scaling Required IAM Permissions<a name="auto-scaling-IAM"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. IAM users must have the appropriate permissions for these services before they can use Service Auto Scaling in the AWS Management Console or with the AWS CLI or SDKs\. In addition to the standard IAM permissions for creating and updating services, Service Auto Scaling requires the following permissions:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "application-autoscaling:*",
        "cloudwatch:DescribeAlarms",
        "cloudwatch:PutMetricAlarm"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

The [Create Services](IAMPolicyExamples.md#IAM_create_service_policies) and [Update Services](IAMPolicyExamples.md#IAM_update_service_policies) IAM policy examples show the permissions that are required for IAM users to use Service Auto Scaling in the AWS Management Console\.

The Application Auto Scaling service needs permission to describe your ECS services and CloudWatch alarms, as well as permissions to modify your service's desired count on your behalf\. You must create an IAM role \(`ecsAutoscaleRole`\) for your ECS services to provide these permissions and then associate that role with your service before it can use Application Auto Scaling\. If an IAM user has the required permissions to use Service Auto Scaling in the Amazon ECS console, create IAM roles, and attach IAM role policies to them, then that user can create this role automatically as part of the Amazon ECS console create service or update service workflows, and then use the role for any other service later \(in the console or with the CLI/SDKs\)\. You can also create the role by following the procedures in [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

## Service Auto Scaling Concepts<a name="auto-scaling-concepts"></a>

+ The ECS service scheduler respects the desired count at all times, but as long as you have active scaling policies and alarms on a service, Service Auto Scaling could change a desired count that was manually set by you\.

+ If a service's desired count is set below its minimum capacity value, and an alarm triggers a scale out activity, Application Auto Scaling scales the desired count up to the minimum capacity value and then continues to scale out as required, based on the scaling policy associated with the alarm\. However, a scale in activity will not adjust the desired count, because it is already below the minimum capacity value\.

+ If a service's desired count is set above its maximum capacity value, and an alarm triggers a scale in activity, Application Auto Scaling scales the desired count down to the maximum capacity value and then continues to scale in as required, based on the scaling policy associated with the alarm\. However, a scale out activity will not adjust the desired count, because it is already above the maximum capacity value\.

+ During scaling activities, the actual running task count in a service is the value that Service Auto Scaling uses as its starting point, as opposed to the desired count, which is what processing capacity is supposed to be\. This prevents excessive \(runaway\) scaling that could not be satisfied, for example, if there are not enough container instance resources to place the additional tasks\. If the container instance capacity is available later, the pending scaling activity may succeed, and then further scaling activities can continue after the cool down period\.

## Amazon ECS Console Experience<a name="service-auto-scaling-console"></a>

The Amazon ECS console's service creation and service update workflows support Service Auto Scaling\. The ECS console handles the `ecsAutoscaleRole` and policy creation, provided that the IAM user who is using the console has the permissions described in [Service Auto Scaling Required IAM Permissions](#auto-scaling-IAM), and that they can create IAM roles and attach policies to them\.

When you configure a service to use Service Auto Scaling in the console, your service is automatically registered as a scalable target with Application Auto Scaling so that you can configure scaling policies that scale your service up and down\. You can also create and update the scaling policies and CloudWatch alarms that trigger them in the Amazon ECS console\.

To create a new ECS service that uses Service Auto Scaling, see [Creating a Service](create-service.md)\.

To update an existing service to use Service Auto Scaling, see [Updating a Service](update-service.md)\.

## AWS CLI and SDK Experience<a name="service-auto-scaling-api"></a>

You can configure Service Auto Scaling by using the AWS CLI or the AWS SDKs, but you must observe the following considerations\.

+ Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. For more information about these specific API operations, see the [Amazon Elastic Container Service API Reference](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/), the [Amazon CloudWatch API Reference](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/), and the [Application Auto Scaling API Reference](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/)\. For more information about the AWS CLI commands for these services, see the [ecs](http://docs.aws.amazon.com/cli/latest/reference/ecs), [cloudwatch](http://docs.aws.amazon.com/cli/latest/reference/cloudwatch), and [application\-autoscaling](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling) sections of the [AWS Command Line Interface Reference](http://docs.aws.amazon.com/cli/latest/reference/)\.

+ Before your service can use Service Auto Scaling, you must register it as a scalable target with the Application Auto Scaling [RegisterScalableTarget](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/API_RegisterScalableTarget.html) API operation\.

+ After your ECS service is registered as a scalable target, you can create scaling policies with the Application Auto Scaling [PutScalingPolicy](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/API_PutScalingPolicy.html) API operation to specify what should happen when your CloudWatch alarms are triggered\.

+ After you create the scaling policies for your service, you can create the CloudWatch alarms that trigger the scaling events for your service with the CloudWatch [PutMetricAlarm](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html) API operation\.