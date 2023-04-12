# Amazon ECS CloudWatch Events IAM Role<a name="CWE_IAM_role"></a>

Before you can use Amazon ECS scheduled tasks with CloudWatch Events rules and targets, the CloudWatch Events service needs permissions to run Amazon ECS tasks on your behalf\. These permissions are provided by the CloudWatch Events IAM role \(`ecsEventsRole`\)\.

The CloudWatch Events role is automatically created for you in the AWS Management Console when you configure a scheduled task\. For more information, see [Scheduled tasks](scheduled_tasks.md)\.

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
		},
		{
			"Effect": "Allow",
			"Action": "ecs:TagResource",
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"ecs:CreateAction": [
						"RunTask"
					]
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

## Checking for the Amazon ECS CloudWatch Events \(`ecsEventsRole`\) in the IAM console<a name="cw-iam-role-verify"></a>

The Amazon ECS instance role is automatically created for you when completing the Amazon ECS console first\-run experience\. However, you can manually create the role and attach the managed IAM policy for container instances to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. Use the following procedure to check and see if your account already has the Amazon ECS container instance IAM role and to attach the managed IAM policy if needed\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. In the search box, enter `ecsEventsRole`\. If the role does exist, choose the role to view the attached policies\.

1. On the **Permissions** tab, verify that the **AmazonEC2ContainerServiceEventsRole** is attached to the role\.

   1. Choose **Add Permissions**, **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, enter **AmazonEC2ContainerServiceEventsRole**\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceEventsRole** policy, and then choose **Attach policy**\.

1. Choose **Trust relationships**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, choose **Edit trust policy**, copy the policy into the **Policy Document** window and choose **Update policy**\.

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

## Creating the Amazon ECS CloudWatch Events \(`ecsEventsRole`\) role<a name="cw-iam-role-create"></a>

**To create an IAM role for CloudWatch Events**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. In the **Trusted entity type** section, choose **AWS service**, **Elastic Container Service**\.

1. For **Use case**, choose **Elastic Container Service Task**, and then choose **Next**\.

1. In the **Attach permissions policy** section, do the following:

   1. In the search box, enter `AmazonEC2ContainerServiceEventsRole`, and then select the policy\.

   1. Under **Set permissions boundary \- optional**, choose **Create role without a permissions boundary**\.

   1. Choose **Next**\.

1. Under **Role details**, do the following: 

   1. For **Role name**, enter `ecsEventsRole` \.

   1. For **Add tags \(optional\)**, enter any custom tags to associate with the policy\.

1. Choose **Create role**\.

1. Search the list of roles for `ecsEventsRole` and select the role\.

1. Replace the existing trust relationship with the following text\. On the ** Trust relationships** tab, choose **Edit trust policy**, copy the policy into the **Policy Document** window, and then choose **Update policy**\.

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

## Attaching a policy to the `ecsEventsRole` role<a name="cw-iam-role-attach"></a>

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

1. For **Name**, enter `AmazonECSEventsTaskExecutionRole`, optionally enter a description, and then choose **Create policy**\.

1. In the navigation pane, choose **Roles**\.

1. Search the list of roles for `ecsEventsRole`, and then select the role to view the attached policies\.

1. Choose **Attach policy**\.

1. In the **Attach policy** section, select the **AmazonECSEventsTaskExecutionRole** policy, and then choose **Attach policy**\.