# Specifying sensitive data using Systems Manager Parameter Store<a name="specifying-sensitive-data-parameters"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\.

**Topics**
+ [Considerations for specifying sensitive data using Systems Manager Parameter Store](#secrets--parameterstore-considerations)
+ [Required IAM permissions for Amazon ECS secrets](#secrets-iam-parameters)
+ [Injecting sensitive data as an environment variable](#secrets-envvar-parameters)
+ [Injecting sensitive data in a log configuration](#secrets-logconfig-parameters)
+ [Creating an AWS Systems Manager Parameter Store parameter](#secrets-create-parameter)
+ [Creating a Task Definition that References sensitive data](#secrets-create-taskdefinition-parameters)

## Considerations for specifying sensitive data using Systems Manager Parameter Store<a name="secrets--parameterstore-considerations"></a>

The following should be considered when specifying sensitive data for containers using Systems Manager Parameter Store parameters\.
+ For tasks that use the Fargate launch type, this feature requires that your task use platform version 1\.3\.0 or later\. For information, see [AWS Fargate platform versions](platform_versions.md)\.
+ For tasks that use the EC2 launch type, this feature requires that your container instance have version 1\.22\.0 or later of the container agent\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.
+ Sensitive data is injected into your container when the container is initially started\. If the secret or Parameter Store parameter is subsequently updated or rotated, the container will not receive the updated value automatically\. You must either launch a new task or if your task is part of a service you can update the service and use the **Force new deployment** option to force the service to launch a fresh task\.
+ For Windows tasks that are configured to use the `awslogs` logging driver, you must also set the `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE` environment variable on your container instance\. This can be done with User Data using the following syntax:

  ```
  <powershell>
  [Environment]::SetEnvironmentVariable("ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE", $TRUE, "Machine")
  Initialize-ECSAgent -Cluster <cluster name> -EnableTaskIAMRole -LoggingDrivers '["json-file","awslogs"]'
  </powershell>
  ```

## Required IAM permissions for Amazon ECS secrets<a name="secrets-iam-parameters"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary AWS Systems Manager resources\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
For tasks that use the EC2 launch type, you must use the ECS agent configuration variable `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE=true` to use this feature\. You can add it to the `./etc/ecs/ecs.config` file during container instance creation or you can add it to an existing instance and then restart the ECS agent\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

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
        "arn:aws:ssm:<region>:<aws_account_id>:parameter/<parameter_name>",
        "arn:aws:secretsmanager:<region>:<aws_account_id>:secret:<secret_name>",
        "arn:aws:kms:<region>:<aws_account_id>:key/<key_id>"
      ]
    }
  ]
}
```

## Injecting sensitive data as an environment variable<a name="secrets-envvar-parameters"></a>

Within your container definition, specify `secrets` with the name of the environment variable to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container\.

The following is a snippet of a task definition showing the format when referencing an Systems Manager Parameter Store parameter\. If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the parameter\. If the parameter exists in a different Region, then the full ARN must be specified\.

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:ssm:region:aws_account_id:parameter/parameter_name"
    }]
  }]
}
```

## Injecting sensitive data in a log configuration<a name="secrets-logconfig-parameters"></a>

Within your container definition, when specifying a `logConfiguration` you can specify `secretOptions` with the name of the log driver option to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container\.

**Important**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the parameter\. If the parameter exists in a different Region, then the full ARN must be specified\.

The following is a snippet of a task definition showing the format when referencing an Systems Manager Parameter Store parameter\.

```
{
  "containerDefinitions": [{
    "logConfiguration": [{
      "logDriver": "fluentd",
      "options": {
        "tag": "fluentd demo"
      },
      "secretOptions": [{
        "name": "fluentd-address",
        "valueFrom": "arn:aws:ssm:region:aws_account_id:parameter:parameter_name"
      }]
    }]
  }]
}
```

## Creating an AWS Systems Manager Parameter Store parameter<a name="secrets-create-parameter"></a>

You can use the AWS Systems Manager console to create a Systems Manager Parameter Store parameter for your sensitive data\. For more information, see [Walkthrough: Create and Use a Parameter in a Command \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-console.html) in the *AWS Systems Manager User Guide*\.

**To create a Parameter Store parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**, **Create parameter**\.

1. For **Name**, type a hierarchy and a parameter name\. For example, type `test/database_password`\.

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

## Creating a Task Definition that References sensitive data<a name="secrets-create-taskdefinition-parameters"></a>

You can use the Amazon ECS console to create a task definition that references a Systems Manager Parameter Store parameter\.

**To create a task definition that specifies a secret**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **task definitions**, **Create new task definition**\.

1. On the **Select launch type compatibility** page, choose the launch type for your tasks and choose **Next step**\.
**Note**  
This step only applies to Regions that currently support Amazon ECS using AWS Fargate\. For more information, see [Amazon ECS on AWS Fargate](AWS_Fargate.md)\.

1. For **task definition Name**, type a name for your task definition\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\.

1. For **Task execution role**, either select your existing task execution role or choose **Create new role** to have one created for you\. This role authorizes Amazon ECS to pull private images for your task\. For more information, see [Required IAM permissions for private registry authentication](private-auth.md#private-auth-iam)\.
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

   1. For sensitive data to inject as environment variables, under **Environment**, for **Environment variables**, complete the following fields:

      1. For **Key**, enter the name of the environment variable to set in the container\. This corresponds to the `name` field in the `secrets` section of a container definition\.

      1. For **Value**, choose **ValueFrom**\. For **Add value**, enter the full ARN of the AWS Systems Manager Parameter Store parameter that contains the data to present to your container as an environment variable\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. For secrets referenced in the log configuration for a container, under **Storage and Logging**, for **Log configuration**, complete the following fields:

      1. Clear the **Auto\-configure CloudWatch Logs** option\.

      1. Under **Log options**, for **Key**, enter the name of the log configuration option to set\.

      1. For **Value**, choose **ValueFrom**\. For **Add value**, enter the name or full ARN of the AWS Systems Manager Parameter Store parameter that contains the data to present to your log configuration as a log option\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or the name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. Fill out the remaining required fields and any optional fields to use in your container definitions\. More container definition parameters are available in the **Advanced container configuration** menu\. For more information, see [Task definition parameters](task_definition_parameters.md)\.

   1. Choose **Add**\.

1. When your containers are added, choose **Create**\.