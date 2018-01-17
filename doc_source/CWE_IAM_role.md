# CloudWatch Events IAM Role<a name="CWE_IAM_role"></a>

Before you can use Amazon ECS scheduled tasks with CloudWatch Events rules and targets, the CloudWatch Events service needs permission to run Amazon ECS tasks on your behalf\. These permissions are provided by the CloudWatch Events IAM role \(`ecsEventsRole`\)\.

The CloudWatch Events role is created for you in the AWS Management Console when you configure a scheduled task\. For more information, see [Scheduled Tasks \(`cron`\)](scheduled_tasks.md)\.

The `AmazonEC2ContainerServiceEventsRole` policy is shown below\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:RunTask"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```