# Using Secrets Manager<a name="secrets-envvar-secrets-manager"></a>

When you inject a secret as an environment variable, you can specify the full contents of a secret, a specific JSON key within a secret, or a specific version of a secret to inject\. This helps you control the sensitive data exposed to your container\. For more information about secret versioning, see [Key Terms and Concepts for AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/terms-concepts.html#term_secret) in the *AWS Secrets Manager User Guide*\.

## Considerations<a name="secrets-envvar-secrets-manager-considerations"></a>

The following should be considered when using an environment variable to inject an Secrets Manager secret into a container\.
+ Sensitive data is injected into your container when the container is initially started\. If the secret is subsequently updated or rotated, the container will not receive the updated value automatically\. You must either launch a new task or if your task is part of a service you can update the service and use the **Force new deployment** option to force the service to launch a fresh task\.
+ For Amazon ECS tasks on AWS Fargate, the following should be considered:
  + To inject the full content of a secret as an environment variable or in a log configuration, you must use platform version `1.3.0` or later\. For information, see [AWS Fargate platform versions](platform_versions.md)\.
  + To inject a specific JSON key or version of a secret as an environment variable or in a log configuration, you must use platform version `1.4.0` or later \(Linux\) or `1.0.0` \(Windows\)\. For information, see [AWS Fargate platform versions](platform_versions.md)\.
+ For Amazon ECS tasks on EC2, the following should be considered:
  + To inject a secret using a specific JSON key or version of a secret, your container instance must have version `1.37.0` or later of the container agent\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.

    To inject the full contents of a secret as an environment variable or to inject a secret in a log configuration, your container instance must have version `1.22.0` or later of the container agent\.
+ When using a task definition that references Secrets Manager secrets to retrieve sensitive data for your containers, if you are also using interface VPC endpoints, you must create the interface VPC endpoints for Secrets Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
+ For Windows tasks that are configured to use the `awslogs` logging driver, you must also set the `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE` environment variable on your container instance\. This can be done with User Data with the following syntax:

  ```
  <powershell>
  [Environment]::SetEnvironmentVariable("ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE", $TRUE, "Machine")
  Initialize-ECSAgent -Cluster <cluster name> -EnableTaskIAMRole -LoggingDrivers '["json-file","awslogs"]'
  </powershell>
  ```

## IAM permissions<a name="secrets-envvar-secrets-manager-iam"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

To provide access to the Secrets Manager secrets that you create, manually add the following permissions as an inline policy to the task execution role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `secretsmanager:GetSecretValue`–Required if you are referencing a Secrets Manager secret\.
+ `kms:Decrypt`–Required only if your secret uses a customer managed key and not the default key\. The ARN for your customer managed key should be added as a resource\.

The following example policy adds the required permissions:

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

## Create the AWS Secrets Manager secret<a name="secrets-envvar-secrets-manager-create-secret"></a>

You can use the Secrets Manager console to create a secret for your sensitive data\. For more information, see [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) in the *AWS Secrets Manager User Guide*\.

## Add the environment variable to the container definition<a name="secrets-envvar-secrets-manager-update-container-definition"></a>

Within your container definition, you can specify the following:
+ The `secrets` object containing the name of the environment variable to set in the container
+ The Amazon Resource Name \(ARN\) of the Secrets Manager secret
+ Additional parameters that contain the sensitive data to present to the container

The following example shows the full syntax that must be specified for the Secrets Manager secret\.

```
arn:aws:secretsmanager:region:aws_account_id:secret:secret-name:json-key:version-stage:version-id
```

The following section describes the additional parameters\. These parameters are optional, but if you do not use them, you must include the colons `:` to use the default values\. Examples are provided below for more context\.

`json-key`  
Specifies the name of the key in a key\-value pair with the value that you want to set as the environment variable value\. Only values in JSON format are supported\. If you do not specify a JSON key, then the full contents of the secret is used\.

`version-stage`  
Specifies the staging label of the version of a secret that you want to use\. If a version staging label is specified, you cannot specify a version ID\. If no version stage is specified, the default behavior is to retrieve the secret with the `AWSCURRENT` staging label\.  
Staging labels are used to keep track of different versions of a secret when they are either updated or rotated\. Each version of a secret has one or more staging labels and an ID\. For more information, see [Key Terms and Concepts for AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/terms-concepts.html#term_secret) in the *AWS Secrets Manager User Guide*\.

`version-id`  
Specifies the unique identifier of the version of a secret that you want to use\. If a version ID is specified, you cannot specify a version staging label\. If no version ID is specified, the default behavior is to retrieve the secret with the `AWSCURRENT` staging label\.  
Version IDs are used to keep track of different versions of a secret when they are either updated or rotated\. Each version of a secret has an ID\. For more information, see [Key Terms and Concepts for AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/terms-concepts.html#term_secret) in the *AWS Secrets Manager User Guide*\.

### Example container definitions<a name="secrets-examples"></a>

The following examples show ways in which you can reference Secrets Manager secrets in your container definitions\.

**Example referencing a full secret**  
The following is a snippet of a task definition showing the format when referencing the full text of a Secrets Manager secret\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name-AbCdEf"
    }]
  }]
}
```
To access the value of this secret from within the container you would need to call the `$environment_variable_name`\.

**Example referencing a specific key within a secret**  
The following shows an example output from a [get\-secret\-value](https://docs.aws.amazon.com/cli/latest/reference/secretsmanager/get-secret-value.html) command that displays the contents of a secret along with the version staging label and version ID associated with it\.  

```
{
    "ARN": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf",
    "Name": "appauthexample",
    "VersionId": "871d9eca-18aa-46a9-8785-981ddEXAMPLE",
    "SecretString": "{\"username1\":\"password1\",\"username2\":\"password2\",\"username3\":\"password3\"}",
    "VersionStages": [
        "AWSCURRENT"
    ],
    "CreatedDate": 1581968848.921
}
```
Reference a specific key from the previous output in a container definition by specifying the key name at the end of the ARN\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf:username1::"
    }]
  }]
}
```

**Example referencing a specific secret version**  
The following shows an example output from a [describe\-secret](https://docs.aws.amazon.com/cli/latest/reference/secretsmanager/describe-secret.html) command that displays the unencrypted contents of a secret along with the metadata for all versions of the secret\.  

```
{
    "ARN": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf",
    "Name": "appauthexample",
    "Description": "Example of a secret containing application authorization data.",
    "RotationEnabled": false,
    "LastChangedDate": 1581968848.926,
    "LastAccessedDate": 1581897600.0,
    "Tags": [],
    "VersionIdsToStages": {
        "871d9eca-18aa-46a9-8785-981ddEXAMPLE": [
            "AWSCURRENT"
        ],
        "9d4cb84b-ad69-40c0-a0ab-cead3EXAMPLE": [
            "AWSPREVIOUS"
        ]
    }
}
```
Reference a specific version staging label from the previous output in a container definition by specifying the key name at the end of the ARN\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf::AWSPREVIOUS:"
    }]
  }]
}
```
Reference a specific version ID from the previous output in a container definition by specifying the key name at the end of the ARN\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf:::9d4cb84b-ad69-40c0-a0ab-cead3EXAMPLE"
    }]
  }]
}
```

**Example referencing a specific key and version staging label of a secret**  
The following shows how to reference both a specific key within a secret and a specific version staging label\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf:username1:AWSPREVIOUS:"
    }]
  }]
}
```
To specify a specific key and version ID, use the following syntax\.  

```
{
  "containerDefinitions": [{
    "secrets": [{
      "name": "environment_variable_name",
      "valueFrom": "arn:aws:secretsmanager:region:aws_account_id:secret:appauthexample-AbCdEf:username1::9d4cb84b-ad69-40c0-a0ab-cead3EXAMPLE"
    }]
  }]
}
```

## Create a task definition that references sensitive data<a name="secrets-envvar-secrets-manager-task-definition"></a>

You can use the Amazon ECS console to create a task definition that references a Secrets Manager secret\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**

1. Choose **Create new task definition**, **Create new task definition**\.

1. For **Task definition family**, specify a unique name for the task definition\.

1. For each container to define in your task definition, complete the following steps\.

   1. For **Name**, enter a name for the container\.

   1. For **Image URI**, enter the image to use to start a container\. Images in the Amazon ECR Public Gallery registry may be specified using the Amazon ECR Public registry name only\. For example, if `public.ecr.aws/ecs/amazon-ecs-agent:latest` is specified, the Amazon Linux container hosted on Amazon ECR Public Gallery is used\. For all other repositories, specify the repository using either the `repository-url/image:tag` or `repository-url/image@digest` formats\.

   1. For **Essential container**, if your task definition has two or more containers defined, you may specify whether the container should be considered essential\. If a container is marked as *essential*, if that container stops then the task is stopped\. Each task definition must contain at least one essential container\.

   1. A port mapping allows the container to access ports on the host to send or receive traffic\. Under **Port mappings**, do one of the following: 
      + When you use the **awsvpc** network mode, for **Container port** and **Protocol**, choose the port mapping to use for the container\.
      + When you use the **bridge** network mode, for **Container port** and **Protocol**, choose the port mapping to use for the container\. You select the **bridge** network mode on the next page\. After you select it, choose **Previous**, and then for **Host port**, specify the port number on the container instance to reserve for your container\.

      Choose **Add more port mappings** to specify additional container port mappings\.

   1. For sensitive data to inject as environment variables, under **Environment**, for **Environment variables**, complete the following fields:

      1. For **Key**, enter the name of the environment variable to set in the container\. This corresponds to the `name` field in the `secrets` section of a container definition\.

      1. For **Value**, choose **ValueFrom**\. For **Add value**, enter the ARN of the Secrets Manager secret that contains the data to present to your container as an environment variable\.

   1. \(Optional\) Choose **Add more containers** to add additional containers to the task definition\. Choose **Next** once all containers have been defined\.

1. For **App environment**, **Task size**, and **Container size**, fill out the remaining required fields and any optional fields\.

1. \(Optional\) Expand the **Task roles, network mode** section to specify the following:

   1. For **Task role**, choose the IAM role to assign to the task\. A task IAM role provides permissions for the containers in a task to call AWS APIs\.

1. \(Optional\) The **Storage** section is used to expand the amount of ephemeral storage for tasks hosted on Fargate as well as add a data volume configuration for the task\.

   1. To expand the available ephemeral storage beyond the default value of 20 GiB for your Fargate tasks, for **Amount**, enter a value up to 200 GiB\.

1. For sensitive data referenced in the log configuration for a container, under **Use log collection**, for **Log configuration**, complete the following configuration:

   1. Select the log option, and then under **Key**, choose **Add**\.

   1. For **Key**, enter the name of the log configuration option to set\.

   1.  For **Value**, enter the full ARN of the Secrets Manager secret that contains the data to present to your log configuration as a log option\.

   1. For **Value type**, choose **ValueFrom**\.

1. Choose **Next** to review the task definition\.

1. On the **Review and create** page, review each task definition section\. Choose **Edit** to make changes\. After the task definition is complete, choose **Create** to register the task definition\.