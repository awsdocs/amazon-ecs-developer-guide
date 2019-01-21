# Specifying Sensitive Data<a name="specifying-sensitive-data"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\.

For tasks that use the Fargate launch type, the only supported method is referencing an Systems Manager Parameter Store parameter\. This feature also requires that your task use platform version 1\.3\.0 or later\. For information, see [AWS Fargate Platform Versions](platform_versions.md)\.

For tasks that use the EC2 launch type, both the Secrets Manager secret and Systems Manager Parameter Store parameter methods described are supported\. This feature requires that your container instance have version 1\.22\.0 or later of the container agent\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

**Note**  
This feature is not available in the GovCloud \(US\-East\) region\.

Within your container definition, specify `secrets` with the name of the environment variable to set in the container and the full ARN of either the Secrets Manager secret or Systems Manager Parameter Store parameter containing the sensitive data to present to the container\. The parameter that you reference can be from a different Region than the container using it, but must be from within the same account\.

**Important**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the parameter\. If the parameter exists in a different Region, then the full ARN must be specified\.

The following is a snippet of a task definition showing the format when referencing an Secrets Manager secret\.

```
"containerDefinitions": [
    {
        "secrets": [
            {
                "name": "environment_variable_name",
                "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name-AbCdEf"
            }
        ]
    }
]
```

The following is a snippet of a task definition showing the format when referencing an Systems Manager Parameter Store parameter\.

```
"containerDefinitions": [
    {
        "secrets": [
            {
                "name": "environment_variable_name",
                "valueFrom": "arn:aws:ssm:region:aws_account_id:parameter/parameter_name"
            }
        ]
    }
]
```

**Topics**
+ [Required IAM Permissions for Amazon ECS Secrets](#secrets-iam)
+ [Creating an AWS Secrets Manager Secret](#secrets-create-secret)
+ [Creating an AWS Systems Manager Parameter Store Parameter](#secrets-create-parameter)
+ [Creating a Task Definition that References a Secret](#secrets-create-taskdefinition)

## Required IAM Permissions for Amazon ECS Secrets<a name="secrets-iam"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary AWS Systems Manager or Secrets Manager resources\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

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
        "ssm:GetParameters",
        "secretsmanager:GetSecretValue",
        "kms:Decrypt"
      ],
      "Resource": [
        "arn:aws:ssm:region:aws_account_id:parameter/parameter_name",
        "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name",
        "arn:aws:kms:region:aws_account_id:key/key_id"
      ]
    }
  ]
}
```

## Creating an AWS Secrets Manager Secret<a name="secrets-create-secret"></a>

You can use the Secrets Manager console to create a secret for your sensitive data\. For more information, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

**To create a basic secret**

Use Secrets Manager to create a secret for your sensitive data\.

1. Open the Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Choose **Store a new secret**\.

1. For **Select secret type**, choose **Other type of secrets**\.

1. Specify the details of your custom secret as **Key** and **Value** pairs\. For example, you can specify a key of UserName, and then supply the appropriate user name as its value\. Add a second key with the name of Password and the password text as its value\. You could also add entries for Database name, Server address, TCP port, and so on\. You can add as many pairs as you need to store the information you require\.

   Alternatively, you can choose the **Plaintext** tab and enter the secret value in any way you like\.

1. Choose the AWS KMS encryption key that you want to use to encrypt the protected text in the secret\. If you don't choose one, Secrets Manager checks to see if there's a default key for the account, and uses it if it exists\. If a default key doesn't exist, Secrets Manager creates one for you automatically\. You can also choose **Add new key** to create a custom CMK specifically for this secret\. To create your own AWS KMS CMK, you must have permissions to create CMKs in your account\.

1. Choose **Next**\.

1. For **Secret name**, type an optional path and name, such as **production/MyAwesomeAppSecret** or **development/TestSecret**, and choose **Next**\. You can optionally add a description to help you remember the purpose of this secret later\.

   The secret name must be ASCII letters, digits, or any of the following characters: /\_\+=\.@\-

1. \(Optional\) At this point, you can configure rotation for your secret\. For this procedure, leave it at **Disable automatic rotation** and choose **Next**\.

   For information about how to configure rotation on new or existing secrets, see [Rotating Your AWS Secrets Manager Secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html)\.

1. Review your settings, and then choose **Store secret** to save everything you entered as a new secret in Secrets Manager\.

## Creating an AWS Systems Manager Parameter Store Parameter<a name="secrets-create-parameter"></a>

You can use the AWS Systems Manager console to create a Systems Manager Parameter Store parameter for your sensitive data\. For more information, see [Walkthrough: Create and Use a Parameter in a Command \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *AWS Systems Manager User Guide*\.

**To create a Parameter Store parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**, **Create parameter**\.

1. For **Name**, type a hierarchy and a parameter name\. For example, type `test/database_password`\.
**Note**  
If you are referencing an AWS Secrets Manager secret in your parameter, the parameter name must begin with the following reserved path: `/aws/reference/secretsmanager/`\. For more information, see [Referencing AWS Secrets Manager Secrets from Parameter Store Parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html) in the *AWS Systems Manager User Guide*\.

1. For **Description**, type an optional description\.

1. For **Type**, choose **String**, **StringList**, or **SecureString**\.
**Note**  
If you choose **SecureString**, the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses `alias/aws/ssm`, which is the default KMS CMK for Systems Manager\. To avoid using this key, choose a custom key\. For more information, see [Use Secure String Parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-about.html) in the *AWS Systems Manager User Guide*\.
When you create a secure string parameter in the console by using the `key-id` parameter with either a custom KMS CMK alias name or an alias ARN, you must specify the prefix `alias/` before the alias\. The following is an ARN example:  

     ```
     arn:aws:kms:us-east-2:123456789012:alias/MyAliasName
     ```
The following is an alias name example:  

     ```
     alias/MyAliasName
     ```

1. For **Value**, type a value\. For example, `MyFirstParameter`\. If you chose **SecureString**, the value is masked as you type\.

1. Choose **Create parameter**\.

## Creating a Task Definition that References a Secret<a name="secrets-create-taskdefinition"></a>

You can use the Amazon ECS console to create a task definition that references either a Secrets Manager secret or a Systems Manager Parameter Store parameter\.

**To create a task definition that specifies a secret**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. On the **Select launch type compatibility** page, choose the launch type for your tasks and choose **Next step**\.
**Note**  
This step only applies to Regions that currently support Amazon ECS using AWS Fargate\. For more information, see [AWS Fargate on Amazon ECS](AWS_Fargate.md)\.

1. For **Task Definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. For **Task execution role**, either select your existing task execution role or choose **Create new role** to have one created for you\. This role authorizes Amazon ECS to pull private images for your task\. For more information, see [Private Registry Authentication Required IAM Permissions](private-auth.md#private-auth-iam)\.
**Important**  
If the **Task execution role** field does not appear, choose **Configure via JSON** and manually add the `executionRoleArn` field to specify your task execution role\. The following code shows the syntax:  

   ```
   "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole"
   ```

1. For each container to create in your task definition, complete the following steps:

   1. Under **Container Definitions**, choose **Add container**\.

   1. For **Container name**, type a name for your container\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. For **Image**, type the image name or path to your private image\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

   1. Expand **Advanced container configuration**\.

   1. Under **Environment**, for **Environment variables**, complete the following fields:

      1. For **Key**, enter the name of the environment variable to set in the container\. This corresponds to the `name` field in the `secrets` section of a container definition\.

      1. For **Value**, choose **ValueFrom**\. For **Add value**, enter the full ARN or the Secrets Manager secret or the name or full ARN of the AWS Systems Manager Parameter Store parameter that contains the data to present to your container as an environment variable\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. Fill out the remaining required fields and any optional fields to use in your container definitions\. More container definition parameters are available in the **Advanced container configuration** menu\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

   1. Choose **Add**\.

1. When your containers are added, choose **Create**\.