# Service Auto Scaling<a name="service-auto-scaling"></a>

Your Amazon ECS service can optionally be configured to use Service Auto Scaling to adjust its desired count up or down automatically\. Service Auto Scaling leverages the Application Auto Scaling service to provide this functionality\. Service Auto Scaling is available in all regions that support Amazon ECS\. For more information, see the [Application Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)\.

Amazon ECS Service Auto Scaling supports the following types of scaling policies:
+ [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)—Increase or decrease the number of tasks that your service runs based on a target value for a specific CloudWatch metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.
+ [Step Scaling Policies](service-autoscaling-stepscaling.md)—Increase or decrease the number of tasks that your service runs in response to CloudWatch alarms\. Step scaling is based on a set of scaling adjustments, known as step adjustments, which vary based on the size of the alarm breach\.

## Service Auto Scaling Required IAM Permissions<a name="auto-scaling-IAM"></a>

Service Auto Scaling is made possible by a combination of the Amazon ECS, CloudWatch, and Application Auto Scaling APIs\. Services are created and updated with Amazon ECS, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. IAM users must have the appropriate permissions for these services before they can interact with scaling policies in the AWS Management Console, AWS CLI or SDKs\. In addition to the standard IAM permissions for creating and updating services, Service Auto Scaling requires the following permissions:

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

The [Create Service Example](security_iam_id-based-policy-examples.md#IAM_create_service_policies) and [Update Service Example](security_iam_id-based-policy-examples.md#IAM_update_service_policies) IAM policy examples show the permissions that are required for IAM users to use Service Auto Scaling in the AWS Management Console\.

The Application Auto Scaling service needs permission to describe your Amazon ECS services and CloudWatch alarms, as well as permissions to modify your service's desired count on your behalf\. You must create an IAM role \(`ecsAutoscaleRole`\) for your ECS services to provide these permissions and then associate that role with your service before it can use Application Auto Scaling\. If an IAM user has the required permissions to use Service Auto Scaling in the Amazon ECS console, create IAM roles, and attach IAM role policies to them, then that user can create this role automatically as part of the Amazon ECS console [create service](create-service.md#create-service.title) or [update service](update-service.md) workflows, and then use the role for any other service later \(in the console or with the CLI or SDKs\)\. You can also create the role by following the procedures in [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

Alternatively, Application Auto Scaling can also utilize a service\-linked role \(`AWSServiceRoleForApplicationAutoScaling_ECSService`\) that performs automatic scaling actions on your behalf\. If you have the required permissions, this role is created automatically for you, with the required IAM policies attached, when you get started with the Application Auto Scaling service\. For more information, see [Service\-Linked Roles for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\. You or an administrator must have either the `iam:CreateServiceLinkedRole` or `AdministratorAccess` permissions to create this service\-linked role\.