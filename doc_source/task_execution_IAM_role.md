# Amazon ECS Task Execution IAM Role<a name="task_execution_IAM_role"></a>

The Amazon ECS container agent, and the Fargate agent for your Fargate tasks, make calls to the Amazon ECS API on your behalf\. The agent requires an IAM role for the service to know that the agent belongs to you\. This IAM role is referred to as a task execution IAM role\. You can have multiple task execution roles for different purposes associated with your account\.

The following are common use cases for a task execution IAM role:
+ Your task uses the Fargate launch type and\.\.\.
  + is pulling a container image from Amazon ECR\.
  + uses the `awslogs` or `awsfirelens` log driver\. For more information, see [Using the awslogs Log Driver](using_awslogs.md) and [Custom log routing](using_firelens.md)\.
+ Your tasks uses either the Fargate or EC2 launch type and\.\.\.
  + is using private registry authentication\. For more information, see [Required IAM Permissions for Private Registry Authentication](#task-execution-private-auth)\.
  + the task definition is referencing sensitive data using Secrets Manager secrets or AWS Systems Manager Parameter Store parameters\. For more information, see [Required IAM Permissions for Amazon ECS Secrets](#task-execution-secrets)\.

**Note**  
The task execution role is supported by Amazon ECS container agent version 1\.16\.0 and later\.

Amazon ECS provides the managed policy named `AmazonECSTaskExecutionRolePolicy` which contains the permissions the common use cases described above require\. It may be necessary to add inline policies to your task execution role for special use cases which are outlined below\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:GetAuthorizationToken",
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

An Amazon ECS task execution role is automatically created for you in the Amazon ECS console first\-run experience; however, you should manually attach the managed IAM policy for tasks to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. You can use the following procedure to check and see if your account already has the Amazon ECS task execution role and to attach the managed IAM policy if needed\.<a name="procedure_check_execution_role"></a>

**To check for the `ecsTaskExecutionRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsTaskExecutionRole`\. If the role does not exist, see [Creating the task execution IAM role](#create-task-execution-role)\. If the role does exist, select the role to view the attached policies\.

1. On the **Permissions** tab, ensure that the **AmazonECSTaskExecutionRolePolicy** managed policy is attached to the role\. If the policy is attached, your Amazon ECS task execution role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, type **AmazonECSTaskExecutionRolePolicy**\.

   1. Check the box to the left of the **AmazonECSTaskExecutionRolePolicy** policy and choose **Attach policy**\.

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
           "Service": "ecs-tasks.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

## Creating the task execution IAM role<a name="create-task-execution-role"></a>

If your account does not already have a task execution role, use the following steps to create the role\.

**To create the `ecsTaskExecutionRole` IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. In the **Select type of trusted entity** section, choose **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task**, then choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, search for **AmazonECSTaskExecutionRolePolicy**, select the policy, and then choose **Next: Review**\.

1. For **Role Name**, type `ecsTaskExecutionRole` and choose **Create role**\.

## Required IAM Permissions for Private Registry Authentication<a name="task-execution-private-auth"></a>

The Amazon ECS task execution role is required to use the private registry authentication feature\. This allows the container agent to pull the container image\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

To provide access to the secrets that you create, manually add the following permissions as an inline policy to the task execution role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `secretsmanager:GetSecretValue`
+ `kms:Decrypt`—Required only if your key uses a custom KMS key and not the default key\. The ARN for your custom key should be added as a resource\.

An example inline policy adding the permissions is shown below\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue"
      ],
      "Resource": [
        "arn:aws:secretsmanager:<region>:<aws_account_id>:secret:secret_name",
        "arn:aws:kms:<region>:<aws_account_id>:key/key_id"     
      ]
    }
  ]
}
```

## Required IAM Permissions for Amazon ECS Secrets<a name="task-execution-secrets"></a>

To use the Amazon ECS secrets feature, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary AWS Systems Manager or Secrets Manager resources\. For more information, see [Specifying sensitive data](specifying-sensitive-data.md)\.

To provide access to the AWS Systems Manager Parameter Store parameters that you create, manually add the following permissions as an inline policy to the task execution role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `ssm:GetParameters`—Required if you are referencing a Systems Manager Parameter Store parameter in a task definition\.
+ `secretsmanager:GetSecretValue`—Required if you are referencing a Secrets Manager secret either directly or if your Systems Manager Parameter Store parameter is referencing a Secrets Manager secret in a task definition\.
+ `kms:Decrypt`—Required only if your secret uses a custom KMS key and not the default key\. The ARN for your custom key should be added as a resource\.

The following example inline policy adds the required permissions:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "kms:Decrypt"
      ],
      "Resource": [
        "arn:aws:secretsmanager:<region>:<aws_account_id>:secret:<secret_name>",
        "arn:aws:kms:<region>:<aws_account_id>:key/<key_id>"
      ]
    }
  ]
}
```

## Optional IAM Permissions for Fargate Tasks Pulling Amazon ECR Images over Interface Endpoints<a name="task-execution-ecr-conditionkeys"></a>

When launching tasks that use the Fargate launch type that pull images from Amazon ECR when Amazon ECR is configured to use an interface VPC endpoint, you can restrict the tasks access to a specific VPC or VPC endpoint\. Do this by creating a task execution role for the tasks to use that use IAM condition keys\.

Use the following IAM global condition keys to restrict access to a specific VPC or VPC endpoint\. For more information, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.
+ `aws:SourceVpc`—Restricts access to a specific VPC\.
+ `aws:SourceVpce`—Restricts access to a specific VPC endpoint\.

The following task execution role policy provides an example for adding condition keys:

**Important**  
The `ecr:GetAuthorizationToken` API action cannot have the `aws:sourceVpc` or `aws:sourceVpce` condition keys applied to it because the GetAuthorizationToken API call goes through the elastic network interface owned by AWS Fargate rather than the elastic network interface of the task\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:GetAuthorizationToken",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:sourceVpce": "vpce-xxxxxx",
                    "aws:sourceVpc": "vpc-xxxxx"
                }
            }
        }
    ]
}
```