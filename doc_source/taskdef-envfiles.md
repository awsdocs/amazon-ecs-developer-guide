# Passing environment variables to a container<a name="taskdef-envfiles"></a>

**Important**  
We recommend storing your sensitive data in either AWS Secrets Manager secrets or AWS Systems Manager Parameter Store parameters\. For more information, see [Passing sensitive data to a container](specifying-sensitive-data.md)\.  
Environment variables specified in the task definition are readable by all IAM users and roles that are allowed the `DescribeTaskDefinition` action for the task definition\.  
Environment variable files are objects in Amazon S3 and all Amazon S3 security considerations apply\. See the below section [Required IAM permissions](#taskdef-envfiles-iam)\.

You can pass environment variables to your containers in the following ways:
+ Individually using the `environment` container definition parameter\. This maps to the `--env` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.
+ In bulk, using the `environmentFiles` container definition parameter to list one or more files that contain the environment variables\. The file must be hosted in Amazon S3\. This maps to the `--env-file` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.

By specifying environment variables in a file, you can bulk inject environment variables\. Within your container definition, specify the `environmentFiles` object with a list of Amazon S3 buckets containing your environment variable files\. The files must use an `.env` file extension and there is a limit of ten files to a task definition\.

We don't enforce a size limit on the environment variables, but a large environment variables file might fill up the disk space\. Each task that uses an environment variables file causes a copy of the file to be downloaded to disk\. We remove the file as part of the task cleanup\. 

The following is a snippet of a task definition showing how to specify individual environment variables\.

```
{
    "family": "",
    "containerDefinitions": [
        {
            "name": "",
            "image": "",
            ...
            "environment": [
                {
                    "name": "variable",
                    "value": "value"
                }
            ],
            ...
        }
    ],
    ...
}
```

The following is a snippet of a task definition showing how to specify an environment variable file\.

```
{
    "family": "",
    "containerDefinitions": [
        {
            "name": "",
            "image": "",
            ...
            "environmentFiles": [
                {
                    "value": "arn:aws:s3:::s3_bucket_name/envfile_object_name.env",
                    "type": "s3"
                }
            ],
            ...
        }
    ],
    ...
}
```

## Considerations for specifying environment variable files<a name="taskdef-envfiles-considerations"></a>

Consider the following when specifying an environment variable file in a container definition\.
+ For Amazon ECS tasks on Amazon EC2, your container instances require that the container agent is version `1.39.0` or later to use this feature\. For information about how to check your agent version and update to the latest version, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.
+ For Amazon ECS tasks on AWS Fargate, your tasks must use platform version `1.4.0` or later \(Linux\) to use this feature\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.

  Verify that the variable is supported for the operating system platform\. For more information, see [Container definitions](task_definition_parameters.md#container_definitions) and [Other task definition parameters](task_definition_parameters.md#other_task_definition_params)\.
+ The file must use the `.env` file extension and UTF\-8 encoding\.
+ Each line in an environment file must contain an environment variable in `VARIABLE=VALUE` format\. Spaces or quotation marks are included as part of the values\. Lines beginning with `#` are treated as comments and are ignored\. For more information about the environment variable file syntax, see [Declare default environment variables in file](https://docs.docker.com/compose/env-file/)\.

  The following is the appropriate syntax\.

  ```
  #This is a comment and will be ignored
  VARIABLE=VALUE
  ENVIRONMENT=PRODUCTION
  ```
+ If there are environment variables specified using the `environment` parameter in a container definition, they take precedence over the variables contained within an environment file\.
+ If multiple environment files are specified and they contain the same variable, they're processed in order of entry\. This means that the first value of the variable is used and subsequent values of duplicate variables are ignored\. We recommend that you use unique variable names\.
+ If an environment file is specified as a container override, it's used\. Moreover, any other environment files that are specified in the container definition is ignored\.

## Required IAM permissions<a name="taskdef-envfiles-iam"></a>

The Amazon ECS task execution role is required to use this feature\. This allows the container agent to pull the environment variable file from Amazon S3\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

To provide access to the Amazon S3 objects that you create, manually add the following permissions as an inline policy to the task execution role\. Use the `Resource` parameter to scope the permission to the Amazon S3 buckets that contain the environment variable files\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
+ `s3:GetObject`
+ `s3:GetBucketLocation`

In the following example, the permissions are added to the inline policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::examplebucket/folder_name/env_file_name"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetBucketLocation"
      ],
      "Resource": [
        "arn:aws:s3:::examplebucket"
      ]
    }
  ]
}
```