# Amazon ECS CloudWatch Events IAM Role<a name="CWE_IAM_role"></a>

Before you can use Amazon ECS scheduled tasks with CloudWatch Events rules and targets, the CloudWatch Events service needs permissions to run Amazon ECS tasks on your behalf\. These permissions are provided by the CloudWatch Events IAM role \(`ecsEventsRole`\)\.

The CloudWatch Events role is automatically created for you in the AWS Management Console when you configure a scheduled task\. For more information, see [Scheduled tasks \(`cron`\)](scheduled_tasks.md)\.

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
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "ecs-tasks.amazonaws.com"
                }
            }
        }
    ]
}
```

If your scheduled tasks require the use of the task execution role, a task role, or a task role override, then you must add `iam:PassRole` permissions for each task execution role, task role, or task role override to the CloudWatch Events IAM role\. For more information about the task execution role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Note**  
Specify the full ARN of your task execution role or task role override\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "arn:aws:iam::<aws_account_id>:role/<ecsTaskExecutionRole_or_TaskRole_name>"
            ]
        }
    ]
}
```

You can use the following procedure to check that your account already has the CloudWatch Events IAM role, and manually create it if needed\.

**To check for the CloudWatch Events IAM role in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Search the list of roles for `ecsEventsRole`\. If the role does not exist, use the next procedure to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose **Permissions**\.

1. In the **Permissions policies** section, ensure that the **AmazonEC2ContainerServiceEventsRole** managed policy is attached to the role\. If the policy is attached, your Amazon ECS service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, type `AmazonEC2ContainerServiceEventsRole`\.

   1. Select the box to the left of the **AmazonEC2ContainerServiceEventsRole** policy and choose **Attach policy**\.

1. Choose **Trust relationships**, **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "events.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To create an IAM role for CloudWatch Events**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. In the **Select type of trusted entity** section, choose **Elastic Container Service**\. For **Select your use case** choose **Elastic Container Service Task**\. Choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, select the **AmazonEC2ContainerServiceEventsRole** policy and choose **Next: Tags**\.

1. In the **Add tags \(optional\)** section, enter any tags you would like to associate with the role and choose **Next: Review**\.

1. For **Role name**, type `ecsEventsRole` to name the role, optionally enter a description, and then choose **Create role**\.

1. Review your role information and choose **Create Role**\.

1. Search the list of roles for `ecsEventsRole` and select the role you just created\.

1. Choose **Trust relationships**, **Edit trust relationship**\.

1. Replace the existing trust relationship with the following text in the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "events.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To add permissions for the task execution role to the CloudWatch Events IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, **Create policy**\.

1. Choose **JSON**, paste the following policy, and then choose **Review policy**:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": [
                   "arn:aws:iam::<aws_account_id>:role/<ecsTaskExecutionRole_or_TaskRole_name>"
               ]
           }
       ]
   }
   ```

1. For **Name**, type `AmazonECSEventsTaskExecutionRole`, optionally enter a description, and then choose **Create policy**\.

1. In the navigation pane, choose **Roles**\.

1. Search the list of roles for `ecsEventsRole` and select the role to view the attached policies\.

1. Choose **Attach policy**\.

1. In the **Attach policy** section, select the **AmazonECSEventsTaskExecutionRole** policy and choose **Attach policy**\.