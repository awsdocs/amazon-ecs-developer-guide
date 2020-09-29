# Service Auto Scaling<a name="service-auto-scaling"></a>

*Automatic scaling* is the ability to increase or decrease the desired count of tasks in your Amazon ECS service automatically\. Amazon ECS leverages the Application Auto Scaling service to provide this functionality\. For more information, see the [Application Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)\.

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. For more information, see [Service utilization](cloudwatch-metrics.md#service_utilization)\. You can use these and other CloudWatch metrics to scale out your service \(add more tasks\) to deal with high demand at peak times, and to scale in your service \(run fewer tasks\) to reduce costs during periods of low utilization\. 

Amazon ECS Service Auto Scaling supports the following types of automatic scaling:
+ [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)—Increase or decrease the number of tasks that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.
+ [Step Scaling Policies](service-autoscaling-stepscaling.md)—Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, that vary based on the size of the alarm breach\. 
+ [Scheduled Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-scheduled-scaling.html)—Increase or decrease the number of tasks that your service runs based on the date and time\. 

**Service Auto Scaling and Deployment**  
As a best practice to prevent scale in processes from behaving like scale out processes, suspend dynamic scaling during deployments\. This will prevent alarms from triggering scale in and scale out processes that depend on the running task count\. Take the following steps to avoid this issue\.  
Call the `describe-scalable-targets` command, specifying the resource ID of the ECS service associated with the scalable target in Application Auto Scaling \(Example: service/default/sample\-webapp\)\. Record the output\. You will need it when you call the next command\.
Call the `register-scalable-target` command, specifying the resource ID, namespace, and scalable dimension\. Specify `true` for both `DynamicScalingInSuspended` and `DynamicScalingOutSuspended`\.
After deployment is complete, you can call the `register-scalable-target` command to resume scaling\.
For more information, see [Suspending and Resuming Scaling for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-suspend-resume-scaling.html)\.

## IAM Permissions Required for Service Auto Scaling<a name="auto-scaling-IAM"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. 

In addition to the standard IAM permissions for creating and updating services, the IAM user that accesses Service Auto Scaling settings must have the appropriate permissions for the services that support dynamic scaling\. IAM users must have permissions to use the actions shown in the following example policy\. 

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
        "cloudwatch:DescribeAlarms",
        "cloudwatch:DescribeAlarmsForMetric",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "cloudwatch:PutMetricAlarm",
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

The [Create Service Example](security_iam_id-based-policy-examples.md#IAM_create_service_policies) and [Update Service Example](security_iam_id-based-policy-examples.md#IAM_update_service_policies) IAM policy examples show the permissions that are required for IAM users to use Service Auto Scaling in the AWS Management Console\.

The Application Auto Scaling service also needs permission to describe your Amazon ECS services and CloudWatch alarms, and permissions to modify your service's desired count on your behalf\. If you enable automatic scaling for your ECS services, it creates a service\-linked role named `AWSServiceRoleForApplicationAutoScaling_ECSService`\. This service\-linked role grants Application Auto Scaling permission to describe the alarms for your policies, to monitor the current running task count of the service, and to modify the desired count of the service\. The original managed Amazon ECS role for Application Auto Scaling was `ecsAutoscaleRole`, but it is no longer required\. The service\-linked role is the default role for Application Auto Scaling\. For more information, see [Service\-Linked Roles](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\. 

Note that if you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, you might need to add the `ecs:StartTelemetrySession` permission\. For more information, see [Enabling CloudWatch metrics](cloudwatch-metrics.md#enable_cloudwatch)\.