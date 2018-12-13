# Specifying Sensitive Data<a name="specifying-sensitive-data"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\.

You can also reference an AWS Secrets Manager secret in your parameter\. For more information, see [Referencing AWS Secrets Manager Secrets from Parameter Store Parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html) in the *AWS Systems Manager User Guide*\.

For tasks using the EC2 launch type, this feature requires version 1\.22\.0 or later of the container agent\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

**Important**  
This feature is not yet supported for tasks using the Fargate launch type\.

**Note**  
This feature is not available in the GovCloud \(US\-East\) region\.

Within your container definition, specify `secrets` with the name of the environment variable to set in the container and the name or ARN of the AWS Systems Manager Parameter Store parameter containing the sensitive data to present to the container\. The parameter that you reference can be from a different Region than the container using it, but must be from within the same account\.

**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

The following is a snippet of a task definition showing the required parameters:

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

## Required IAM Permissions for Amazon ECS Secrets<a name="secrets-iam"></a>

To use this feature, you must have the Amazon ECS task execution role\. This allows the container agent to pull the necessary AWS Systems Manager and Secrets Manager resources\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

To provide access to the AWS Systems Manager Parameter Store parameters that you create, manually add the following permissions as an inline policy to the task execution role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `ssm:GetParameters`—Always required when referencing an Systems Manager Parameter Store parameter in a task definition\.
+ `secretsmanager:GetSecretValue`—Required only if your parameter is referencing a Secrets Manager secret\.
+ `kms:Decrypt`—Required only if your key uses a custom KMS key and not the default key\. The ARN for your custom key should be added as a resource\.

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
        "arn:aws:kms:region:aws_account_id:key:key_id"
      ]
    }
  ]
}
```

## Creating an AWS Systems Manager Parameter Store Parameter<a name="secrets-create-parameter"></a>

You can use the AWS Systems Manager console to create a Systems Manager Parameter Store parameter for your sensitive data\.

**To create a Parameter Store parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**, **Create parameter**\.

1. For **Name**, type a hierarchy and a parameter name\. For example, type `test/database_password`\.
**Note**  
If you are referencing an AWS Secrets Manager secret in your parameter, the parameter name must begin with the following reserved path: `/aws/reference/secretsmanager/`\. For more information, see [Referencing AWS Secrets Manager Secrets from Parameter Store Parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html) in the *AWS Systems Manager User Guide*\.

1. For **Description**, type an optional description\.

1. For **Type**, choose **String**, **StringList**, or **SecureString**\.
**Note**  
If you choose **SecureString**, the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses `alias/aws/ssm`, which is the default KMS CMK for Systems Manager\. To avoid using this key, tchoose a custom key\. For more information, see [Use Secure String Parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-about.html) in the *AWS Systems Manager User Guide*\.
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

## Creating a Task Definition that References an AWS Systems Manager Parameter Store Parameter<a name="secrets-create-taskdefinition"></a>

You can use the Amazon ECS console to create a task definition that references a Systems Manager Parameter Store parameter\.

**To create a task definition that specifies a Parameter Store parameter**

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

      1. For **Value**, choose **ValueFrom**\. For **Add value**, enter the AWS Systems Manager Parameter Store parameter name or ARN that contains the data to present to your container as an environment variable\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. Fill out the remaining required fields and any optional fields to use in your container definitions\. More container definition parameters are available in the **Advanced container configuration** menu\. For more information, see [Task Definition Parameters](task_definition_parameters.md)\.

   1. Choose **Add**\.

1. When your containers are added, choose **Create**\.