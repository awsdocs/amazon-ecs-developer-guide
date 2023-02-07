# Using AWS Systems Manager<a name="secrets-envvar-ssm-paramstore"></a>

Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\.

## Considerations<a name="secrets-envvar-ssm-paramstore-considerations"></a>

The following should be considered when using an environment variable to inject an AWS Systems Manager secret into a container\.
+ Sensitive data is injected into your container when the container is initially started\. If the secret is subsequently updated or rotated, the container will not receive the updated value automatically\. You must either launch a new task or if your task is part of a service you can update the service and use the **Force new deployment** option to force the service to launch a fresh task\.
+ For Amazon ECS tasks on AWS Fargate, the following should be considered:
  + To inject the full content of a secret as an environment variable or in a log configuration, you must use platform version `1.3.0` or later\. For information, see [AWS Fargate platform versions](platform_versions.md)\.
  + To inject a specific JSON key or version of a secret as an environment variable or in a log configuration, you must use platform version `1.4.0` or later \(Linux\) or `1.0.0` \(Windows\)\. For information, see [AWS Fargate platform versions](platform_versions.md)\.
+ For Amazon ECS tasks on EC2, the following should be considered:
  + To inject a secret using a specific JSON key or version of a secret, your container instance must have version `1.37.0` or later of the container agent\. However, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.

    To inject the full contents of a secret as an environment variable or to inject a secret in a log configuration, your container instance must have version `1.22.0` or later of the container agent\.
+ When you use a task definition that references AWS Systems Manager secrets to retrieve sensitive data for your containers, if you also use interface VPC endpoints, you must create the interface VPC endpoints for AWS Systems Manager\. For more information, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
+ For Windows tasks that are configured to use the `awslogs` logging driver, you must also set the `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE` environment variable on your container instance\. This can be done with User Data using the following syntax:

  ```
  <powershell>
  [Environment]::SetEnvironmentVariable("ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE", $TRUE, "Machine")
  Initialize-ECSAgent -Cluster <cluster name> -EnableTaskIAMRole -LoggingDrivers '["json-file","awslogs"]'
  </powershell>
  ```

## IAM permissions<a name="secrets-envvar-ssm-paramstore-iam"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary AWS Systems Manager resources\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
For tasks that use the EC2 launch type, you must use the ECS agent configuration variable `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE=true` to use this feature\. You can add it to the `./etc/ecs/ecs.config` file during container instance creation or you can add it to an existing instance and then restart the ECS agent\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

To provide access to the AWS Systems Manager Parameter Store parameters that you create, manually add the following permissions to the task execution role\. For information about how to manage permissions, see [Adding and Removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.
+ `ssm:GetParameters`— Required if you are referencing a Systems Manager Parameter Store parameter in a task definition\.
+ `secretsmanager:GetSecretValue`— Required if you are referencing a Secrets Manager secret either directly or if your Systems Manager Parameter Store parameter is referencing a Secrets Manager secret in a task definition\.
+ `kms:Decrypt`— Required only if your secret uses a custom KMS key and not the default key\. The ARN for your custom key should be added as a resource\.

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

## Create the AWS Systems Manager parameter<a name="secrets-envvar-ssm-paramstore-create-parameter"></a>

You can use the Systems Manager console to create a Systems Manager Parameter Store parameter for your sensitive data\. For more information, see [Create a Systems Manager parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-create-console.html) or [Create a Systems Manager parameter \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/param-create-cli.html) in the *AWS Systems Manager User Guide*\.

## Add the environment variable to the container definition<a name="secrets-envvar-ssm-paramstore-update-container-definition"></a>

Within your container definition, specify `secrets` with the name of the environment variable to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container\.

The following is a snippet of a task definition showing the format when referencing a Systems Manager Parameter Store parameter\. If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or name of the parameter\. If the parameter exists in a different Region, then the full ARN must be specified\.

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

## Create a task definition that references sensitive data<a name="secrets-envvar-ssm-paramstore-taskdefinition"></a>

You can use the Amazon ECS console to create a task definition that references a Systems Manager Parameter Store parameter\.

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

      1. For **Value**, choose **ValueFrom**\. For **Value**, enter the name or full ARN of the AWS Systems Manager Parameter Store parameter that contains the data to present to your log configuration as a log option\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or the name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. \(Optional\) Choose **Add more containers** to add additional containers to the task definition\. Choose **Next** once all containers have been defined\.

1. For **App environment**, **Task size**, and **Container size**, fill out the remaining required fields and any optional fields\.

1. \(Optional\) Expand the **Task roles, network mode** section to specify the following:

   1. For **Task role**, choose the IAM role to assign to the task\. A task IAM role provides permissions for the containers in a task to call AWS APIs\.

1. \(Optional\) The **Storage** section is used to expand the amount of ephemeral storage for tasks hosted on Fargate as well as add a data volume configuration for the task\.

   1. To expand the available ephemeral storage beyond the default value of 20 GiB for your Fargate tasks, for **Amount**, enter a value up to 200 GiB\.

1. For sensitive data referenced in the log configuration for a container, under **Use log collection**, for **Log configuration**, complete the following configuration:

   1. Select the log option, and then under **Key**, choose **Add**\.

   1. For **Key**, enter the name of the log configuration option to set\.

   1.  For **Value**, enter the name or full ARN of the AWS Systems Manager Parameter Store parameter that contains the data to present to your log configuration as a log option\.
**Note**  
If the Systems Manager Parameter Store parameter exists in the same Region as the task you are launching, then you can use either the full ARN or the name of the secret\. If the parameter exists in a different Region, then the full ARN must be specified\.

   1. For **Value type**, choose **ValueFrom**\.

1. Choose **Next** to review the task definition\.

1. On the **Review and create** page, review each task definition section\. Choose **Edit** to make changes\. After the task definition is complete, choose **Create** to register the task definition\.