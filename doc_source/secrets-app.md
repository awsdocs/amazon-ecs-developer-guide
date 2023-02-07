# Retrieve secrets programmatically through your application<a name="secrets-app"></a>

Instead of hardcoding sensitive information in plain text in your application, you can use Secrets Manager or Systems Manager Parameter Store to store the sensitive data\.

We recommend this method of retrieving sensitive data because if the Secrets Manager secret is subsequently updated, the application automatically retrieves the latest version of the secret\.

**Topics**
+ [Using Secrets Manager](secrets-app-secrets-manager.md)
+ [Using AWS Systems Manager Parameter Store](secrets-app-ssm-paramstore.md)