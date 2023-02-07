# Passing sensitive data to a container<a name="specifying-sensitive-data"></a>

You can safely pass sensitive data, such as credentials to a database, into your container\. To start, first store the sensitive data as a secret in AWS Secrets Manager or as a parameter in AWS Systems Manager Parameter Store\. Then, use one of the following ways to expose the secret to the container\.

**Topics**
+ [Retrieve secrets programmatically through your application](secrets-app.md)
+ [Retrieve secrets through environment variables](secrets-envvar.md)
+ [Retrieve secrets for logging configuration](secrets-logconfig.md)