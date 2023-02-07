# Retrieve secrets through environment variables<a name="secrets-envvar"></a>

Instead of hardcoding sensitive information in plain text in your application, you can use Secrets Manager or AWS Systems Manager Parameter Store to store the sensitive data\. Then, you can create an environment variable in the container definition and enter the ARN of the Secrets Manager or AWS Systems Manager secret as the value\. This allows your container to retrieve sensitive data at runtime\.

**Topics**
+ [Using Secrets Manager](secrets-envvar-secrets-manager.md)
+ [Using AWS Systems Manager](secrets-envvar-ssm-paramstore.md)