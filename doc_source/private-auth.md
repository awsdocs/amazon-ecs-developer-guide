# Private registry authentication for tasks<a name="private-auth"></a>

Private registry authentication for tasks using AWS Secrets Manager enables you to store your credentials securely and then reference them in your container definition\. This allows your tasks to use images from private repositories\. This feature is supported by tasks using both the Fargate or EC2 launch types\.

**Important**  
If your task definition references an image stored in Amazon ECR, this topic does not apply\. For more information, see [Using Amazon ECR Images with Amazon ECS](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ECR_on_ECS.html) in the *Amazon Elastic Container Registry User Guide*\.

For tasks using the EC2 launch type, this feature requires version 1\.19\.0 or later of the container agent; however, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

For tasks using the Fargate launch type, this feature requires platform version 1\.2\.0 or later\. For information, see [AWS Fargate platform versions](platform_versions.md)\.

Within your container definition, specify `repositoryCredentials` with the full ARN of the secret that you created\. The secret you reference can be from a different Region than the task using it, but must be from within the same account\.

**Note**  
When using the Amazon ECS API, AWS CLI, or AWS SDK, if the secret exists in the same Region as the task you are launching then you can use either the full ARN or name of the secret\. When using the AWS Management Console, the full ARN of the secret must be specified\.

The following is a snippet of a task definition showing the required parameters:

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
Another method of enabling private registry authentication uses Amazon ECS container agent environment variables to authenticate to private registries\. This method is only supported for tasks using the EC2 launch type\. For more information, see [Private Registry Authentication for Container Instances](private-auth-container-instances.md)\.

## Required IAM permissions for private registry authentication<a name="private-auth-iam"></a>

The Amazon ECS task execution role is required to use this feature\. This allows the container agent to pull the container image\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

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

## Enabling private registry authentication<a name="private-auth-enable"></a>

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

1. For **Secret name**, type an optional path and name, such as **production/MyAwesomeAppSecret** or **development/TestSecret**, and choose **Next**\. You can optionally add a description to help you remember the purpose of this secret later\.

   The secret name must be ASCII letters, digits, or any of the following characters: /\_\+=\.@\-

1. \(Optional\) At this point, you can configure rotation for your secret\. For this procedure, leave it at **Disable automatic rotation** and choose **Next**\.

   For information about how to configure rotation on new or existing secrets, see [Rotating Your AWS Secrets Manager Secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html)\.

1. Review your settings, and then choose **Store secret** to save everything you entered as a new secret in Secrets Manager\.

**To create a task definition that uses private registry authentication**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**\.

1. On the **task definitions** page, choose **Create new task definition**\.

1. On the **Select launch type compatibility** page, choose the launch type for your tasks and then **Next step**\.
**Note**  
This step only applies to regions that currently support Amazon ECS using AWS Fargate\. For more information, see [Amazon ECS on AWS Fargate](AWS_Fargate.md)\.

1. For **task definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. For **Task execution role**, either select your existing task execution role or choose **Create new role** to have one created for you\. This role authorizes Amazon ECS to pull private images for your task\. For more information, see [Required IAM permissions for private registry authentication](#private-auth-iam)\.
**Important**  
If the **Task execution role** field does not appear, choose **Configure via JSON** and manually add the `executionRoleArn` field to specify your task execution role\. The following shows the syntax:  

   ```
   "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole"
   ```

1. For each container to create in your task definition, complete the following steps:

   1. In the **Container Definitions** section, choose **Add container**\.

   1. For **Container name**, type a name for your container\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. For **Image**, type the image name or path to your private image\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. Select the **Private repository authentication** option\.

   1. For **Secrets manager ARN**, enter the full Amazon Resource Name \(ARN\) of the secret that you created earlier\. The value must be between 20 and 2048 characters\.

   1. Fill out the remaining required fields and any optional fields to use in your container definitions\. More container definition parameters are available in the **Advanced container configuration** menu\. For more information, see [Task definition parameters](task_definition_parameters.md)\.

   1. Choose **Add**\.

1. When your containers are added, choose **Create**\.