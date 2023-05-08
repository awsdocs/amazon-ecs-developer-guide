# Private registry authentication for tasks<a name="private-auth"></a>

Private registry authentication for tasks using AWS Secrets Manager enables you to store your credentials securely and then reference them in your task definition\. This provides a way to reference container images that exist in private registries outside of AWS that require authentication in your task definitions\. This feature is supported by tasks hosted on Fargate, Amazon EC2 instances, and external instances using Amazon ECS Anywhere\.

**Important**  
If your task definition references an image that's stored in Amazon ECR, this topic doesn't apply\. For more information, see [Using Amazon ECR Images with Amazon ECS](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ECR_on_ECS.html) in the *Amazon Elastic Container Registry User Guide*\.

For tasks hosted on Amazon EC2 instances, this feature requires version `1.19.0` or later of the container agent\. However, we recommend using the latest container agent version\. For information about how to check your agent version and update to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.

For tasks hosted on Fargate, this feature requires platform version `1.2.0` or later\. For information, see [AWS Fargate platform versions](platform_versions.md)\.

Within your container definition, specify the `repositoryCredentials` object with the details of the secret that you created\. The secret you reference can be from a different AWS Region or a different account than the task using it\.

**Note**  
When using the Amazon ECS API, AWS CLI, or AWS SDK, if the secret exists in the same AWS Region as the task that you're launching then you can use either the full ARN or name of the secret\. If the secret exists in a different account, the full ARN of the secret must be specified\. When using the AWS Management Console, the full ARN of the secret must be specified always\.

The following is a snippet of a task definition that shows the required parameters:

```
"containerDefinitions": [
    {
        "image": "private-repo/private-image",
        "repositoryCredentials": {
            "credentialsParameter": "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name"
        }
    }
]
```

**Note**  
Another method of enabling private registry authentication uses Amazon ECS container agent environment variables to authenticate to private registries\. This method is only supported for tasks hosted on Amazon EC2 instances\. For more information, see [Private registry authentication for container instances](private-auth-container-instances.md)\.

## Required IAM permissions for private registry authentication<a name="private-auth-iam"></a>

The Amazon ECS task execution role is required to use this feature\. This allows the container agent to pull the container image\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

To provide access to the secrets that you create, add the following permissions as an inline policy to the task execution role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `secretsmanager:GetSecretValue`
+ `kms:Decrypt`—Required only if your key uses a custom KMS key and not the default key\. The Amazon Resource Name \(ARN\) for your custom key must be added as a resource\.

The following is an example inline policy that adds the permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:Decrypt",
        "ssm:GetParameters",
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

## Using private registry authentication<a name="private-auth-enable"></a>

**To create a basic secret**

Use AWS Secrets Manager to create a secret for your private registry credentials\.

1. Open the AWS Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Choose **Store a new secret**\.

1. For **Select secret type**, choose **Other type of secrets**\.

1. Select **Plaintext** and enter your private registry credentials using the following format:

   ```
   {
     "username" : "privateRegistryUsername",
     "password" : "privateRegistryPassword"
   }
   ```

1. Choose **Next**\.

1. For **Secret name**, enter an optional path and name, such as **production/MyAwesomeAppSecret** or **development/TestSecret**, and choose **Next**\. You can optionally add a description to help you remember the purpose of this secret later\.

   The secret name must be ASCII letters, digits, or any of the following characters: `/_+=.@-`\.

1. \(Optional\) At this point, you can configure rotation for your secret\. For this procedure, leave it at **Disable automatic rotation** and choose **Next**\.

   For instructions on how to configure rotation on new or existing secrets, see [Rotating Your AWS Secrets Manager Secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html)\.

1. Review your settings, and then choose **Store secret** to save everything that you entered as a new secret in Secrets Manager\.

Register a task definition and under **Private registry**, turn on **Private registry authentication**\. Then, in **Secrets Manager ARN or name**, enter the Amazon Resource Name \(ARN\) of the secret\. For more information, see [Required IAM permissions for private registry authentication](#private-auth-iam)\. For more information, see [Creating a task definition using the console](create-task-definition.md)\.
