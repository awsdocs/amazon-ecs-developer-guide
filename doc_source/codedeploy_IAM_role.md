# Amazon ECS CodeDeploy IAM Role<a name="codedeploy_IAM_role"></a>

Before you can use the CodeDeploy blue/green deployment type with Amazon ECS, the CodeDeploy service needs permissions to update your Amazon ECS service on your behalf\. These permissions are provided by the CodeDeploy IAM role \(`ecsCodeDeployRole`\)\.

**Note**  
IAM users also require permissions to use CodeDeploy; these permissions are described in [Blue/Green Deployment Required IAM Permissions](deployment-type-bluegreen.md#deployment-type-bluegreen-IAM)\. 

There are two managed policies provided\. The `AWSCodeDeployRoleForECS` policy, shown below, gives CodeDeploy permission to update any resource using the associated action\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ecs:DescribeServices",
                "ecs:CreateTaskSet",
                "ecs:UpdateServicePrimaryTaskSet",
                "ecs:DeleteTaskSet",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:ModifyListener",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:ModifyRule",
                "lambda:InvokeFunction",
                "cloudwatch:DescribeAlarms",
                "sns:Publish",
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": [
                        "ecs-tasks.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

The `AWSCodeDeployRoleForECSLimited` policy, shown below, gives CodeDeploy more limited permissions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ecs:DescribeServices",
                "ecs:CreateTaskSet",
                "ecs:UpdateServicePrimaryTaskSet",
                "ecs:DeleteTaskSet",
                "cloudwatch:DescribeAlarms"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "sns:Publish"
            ],
            "Resource": "arn:aws:sns:*:*:CodeDeployTopic_*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:ModifyListener",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:ModifyRule"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:*:*:function:CodeDeployHook_*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "s3:ExistingObjectTag/UseWithCodeDeploy": "true"
                }
            },
            "Effect": "Allow"
        },
        {
            "Action": [
                "iam:PassRole"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iam::*:role/ecsTaskExecutionRole",
                "arn:aws:iam::*:role/ECSTaskExecution*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": [
                        "ecs-tasks.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

**To create an IAM role for CodeDeploy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. For **Select type of trusted entity** section, choose **AWS service**\.

1. For **Choose the service that will use this role**, choose **CodeDeploy**\.

1. For **Select your use case**, choose **CodeDeploy \- ECS**, **Next: Permissions**\.

1. Choose **Next: Tags**\.

1. For **Add tags \(optional\)**, you can add optional IAM tags to the role\. Choose **Next:Review** when finished\.

1. For **Role name**, type `ecsCodeDeployRole`, enter an optional description, and then choose **Create role**\.

**To add the required permissions to the Amazon ECS CodeDeploy IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Search the list of roles for `ecsCodeDeployRole`\. If the role does not exist, use the procedure above to create the role\. If the role does exist, select the role to view the attached policies\.

1. In the **Permissions policies** section, ensure that either the **AWSCodeDeployRoleForECS** or **AWSCodeDeployRoleForECSLimited** managed policy is attached to the role\. If the policy is attached, your Amazon ECS CodeDeploy service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, type **AWSCodeDeployRoleForECS** or **AWSCodeDeployRoleForECSLimited**\.

   1. Check the box to the left of the AWS managed policy and choose **Attach policy**\.

1. Choose **Trust Relationships**, **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "codedeploy.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. If the tasks in your Amazon ECS service using the blue/green deployment type require the use of the task execution role or a task role override, then you must add the `iam:PassRole` permission for each task execution role or task role override to the CodeDeploy IAM role as an inline policy\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md) and [IAM Roles for Tasks](task-iam-roles.md)\.

   Follow the substeps below to create an inline policy\.

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. Search the list of roles for `ecsCodeDeployRole`\. If the role does not exist, use the procedure above to create the role\. If the role does exist, select the role to view the attached policies\.

   1. In the **Permissions policies** section, choose **Add inline policy**\.

   1. Choose the **JSON** tab and add the following policy text\.

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
**Note**  
Specify the full ARN of your task execution role or task role override\.

   1. Choose **Review policy**

   1. For **Name**, type a name for the added policy and then choose **Create policy**\.