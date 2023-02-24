# Using AWS Systems Manager Parameter Store<a name="secrets-app-ssm-paramstore"></a>

Systems Manager Parameter Store provides secure storage and management of secrets\. You can store data such as passwords, database strings, Amazon Elastic Compute Cloud \(Amazon EC2\) instance IDs and Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\.

After you store the secret using Systems Manager Parameter Store, update your application code to retrieve the secret\.

## Considerations<a name="secrets-app-ssm-paramstore-considerations"></a>

Review the following considerations before securing sensitive data in Systems Manager Parameter Store\.
+ Only secrets that store text data are supported\. Secrets that store binary data are not supported\.
+ Use interface VPC endpoints to enhance security controls\.
+ The VPC your task uses must use DNS resolution\.

## Required IAM permissions<a name="secrets-app-ssm-paramstore-iam"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. This allows the container agent to pull the necessary Systems Manager resources\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
For tasks that use the EC2 launch type, you must use the ECS agent configuration variable `ECS_ENABLE_AWSLOGS_EXECUTIONROLE_OVERRIDE=true` to use this feature\. You can add it to the `./etc/ecs/ecs.config` file during container instance creation or you can add it to an existing instance and then restart the ECS agent\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

To provide access to the Systems Manager Parameter Store parameters that you create, manually add the following permissions as a policy to the task execution role\. For information about how to manage permissions, see [Adding and Removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.
+ `ssm:GetParameters` — Required if you are referencing a Systems Manager Parameter Store parameter in a task definition\.
+ `secretsmanager:GetSecretValue` — Required if you are referencing a Secrets Manager secret either directly or if your Systems Manager Parameter Store parameter is referencing a Secrets Manager secret in a task definition\.
+ `kms:Decrypt` — Required only if your secret uses a customer managed key and not the default key\. The ARN for your custom key should be added as a resource\.

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

## Create the secret<a name="secrets-app-ssm-paramstore-create-secret"></a>

You can use the Systems Manager console to create a Systems Manager Parameter Store parameter for your sensitive data\. For more information, see [Create a Systems Manager parameter \(console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-create-console.html) or [Create a Systems Manager parameter \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/param-create-cli.html) in the *AWS Systems Manager User Guide*\.

## Update your application to programmatically retrieve Systems Manager Parameter Store secrets<a name="secrets-app-ssm-paramstore-update-app"></a>

To retrieve the sensitive data stored in the Systems Manager Parameter Store parameter, insert the following code in your application\.

```
import boto3
import os

print("SSM Parameter Store Demo using Python SDK")

    ssm = boto3.client("ssm")
    MY_ENV = os.environ["MY_ENV"]
    
    raw_my_username = ssm.get_parameter(Name=f"/sandbox/{MY_ENV}/username")
    raw_my_password = ssm.get_parameter(Name=f"/sandbox/{MY_ENV}/password", WithDecryption=True)
    
    my_username = raw_my_username['Parameter']['Value']
    my_password = raw_my_password['Parameter']['Value']
    
    print(f"Username: {my_username}")
    print(f"Password: {my_password}")
    
    return f"Successfully fetched credentials on {MY_ENV} environment = {my_username}:{my_password}"
```