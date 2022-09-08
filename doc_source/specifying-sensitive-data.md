# Passing sensitive data to a container<a name="specifying-sensitive-data"></a>

You can use Amazon ECS to inject sensitive data into your containers by storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\.

Secrets can be exposed to a container in the following ways:
+ To inject sensitive data into your containers as environment variables, use the `secrets` container definition parameter\.
+ To reference sensitive information in the log configuration of a container, use the `secretOptions` container definition parameter\.

**Topics**
+ [Using Secrets Manager to secure sensitive data](specifying-sensitive-data-secrets.md)
+ [Using Systems Manager Parameter Store to secure sensitive data](specifying-sensitive-data-parameters.md)