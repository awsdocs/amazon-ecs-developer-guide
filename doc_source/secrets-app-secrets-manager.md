# Using Secrets Manager<a name="secrets-app-secrets-manager"></a>

Use Secrets Manager to protect sensitive data and rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle\.

After you create a Secrets Manager secret, update your application code to retrieve the secret\.

## Considerations<a name="secrets-app-secrets-manager-considerations"></a>

Review the following considerations before securing sensitive data in Secrets Manager\.
+ Only secrets that store text data, which are secrets created with the `SecretString` parameter of the [CreateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html) API, are supported\. Secrets that store binary data, which are secrets created with the SecretBinary parameter of the [CreateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html) API are not supported\.
+ Use interface VPC endpoints to enhance security controls\. You must create the interface VPC endpoints for Secrets Manager\. For information about the VPC endpoint, see [Using Secrets Manager with VPC Endpoints](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) in the *AWS Secrets Manager User Guide*\.
+ The VPC your task uses must use DNS resolution\.

## Required IAM permissions<a name="secrets-app-secrets-manager-iam"></a>

To use this feature, you must have the Amazon ECS task execution role and reference it in your task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

To provide access to the Secrets Manager secrets that you create, manually add the following permission to the task execution role\. For information about how to manage permissions, see [Adding and Removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.
+ `secretsmanager:GetSecretValue`– Required if you are referencing a Secrets Manager secret\.

The following example policy adds the required permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": [
        "arn:aws:secretsmanager:region:aws_account_id:secret:secret_name"
      ]
    }
  ]
}
```

## Create the Secrets Manager secret<a name="secrets-app-secrets-manager-create-secret"></a>

You can use the Secrets Manager console to create a secret for your sensitive data\. For information about how to create secrets, see [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) in the *AWS Secrets Manager User Guide*\.

## Update your application to programmatically retrieve Secrets Manager secrets<a name="secrets-app-secrets-manager-update-app"></a>

You can retrieve secrets with a call to the Secrets Manager APIs directly from your application\. For information about how update your code to request the secret, see [Retrieve secrets from AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets.html) in the *AWS Secrets Manager User Guide*\.

The following code example shows how to get a Secrets Manager secret value\.

```
class SecretsManagerSecret:
    """Encapsulates Secrets Manager functions."""
    def __init__(self, secretsmanager_client):
        """
        :param secretsmanager_client: A Boto3 Secrets Manager client.
        """
        self.secretsmanager_client = secretsmanager_client
        self.name = None

    def get_value(self, stage=None):
        """
        Gets the value of a secret.

        :param stage: The stage of the secret to retrieve. If this is None, the
                      current stage is retrieved.
        :return: The value of the secret. When the secret is a string, the value is
                 contained in the `SecretString` field. When the secret is bytes,
                 it is contained in the `SecretBinary` field.
        """
        if self.name is None:
            raise ValueError

        try:
            kwargs = {'SecretId': self.name}
            if stage is not None:
                kwargs['VersionStage'] = stage
            response = self.secretsmanager_client.get_secret_value(**kwargs)
            logger.info("Got value for secret %s.", self.name)
        except ClientError:
            logger.exception("Couldn't get value for secret %s.", self.name)
            raise
        else:
            return response
```