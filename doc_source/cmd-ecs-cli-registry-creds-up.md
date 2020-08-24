# ecs\-cli registry\-creds up<a name="cmd-ecs-cli-registry-creds-up"></a>

Generates AWS Secrets Manager secrets and an IAM task execution role for use in an Amazon ECS task definition\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

## Syntax<a name="cmd-ecs-cli-registry-creds-up-syntax"></a>

ecs\-cli registry\-creds up *\./creds\_input\_file\.yml* \-\-role\-name *value* \[\-\-update\-existing\-secrets\] \[\-\-no\-role\] \[\-\-no\-output\-value\] \[\-\-output\-dir *value*\] \[\-\-tags *key1=value1,key2=value2*\] \[\-\-help\] 

## Options<a name="cmd-ecs-cli-registry-creds-up-options"></a>


| Name | Description | 
| --- | --- | 
|  `./creds_input_file.yml`  |  Specifies the values related to private registry authentication\. For more information, see [Using Private Registry Authentication](#cmd-ecs-cli-registry-creds-up-privregauth)\. Required: Yes  | 
|  `--role-name value`  |  The name to use for the new task execution role\. If the role already exists, new policies are attached to the existing role\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.  We recommend creating a new task execution role specific to each application to avoid granting permissions to your secrets for applications that do not need them\.  Required: Yes  | 
|  `--update-existing-secrets`  |  Specifies whether existing secrets should be updated with new credential values\. Required: No  | 
|  `--no-role`  |  If specified, no task execution role is created\. Required: No  | 
|  `--no-output-file`  |  If specified, no output file for use with `compose` is created\. Required: No  | 
|  `--output-dir value`  |  The directory where the output file should be created\. If none specified, the file is created in the current working directory\. Required: No  | 
|  `--tags key1=value1,key2=value2`  |  Specifies the metadata to apply to your AWS resources\. Each tag consists of a key and an optional value\. Tags use the following format: `key1=value1,key2=value2,key3=value3`\. For more information, see [Tagging Resources](#cmd-ecs-cli-registry-creds-up-tags)\. Type: Key value pairs Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Using Private Registry Authentication<a name="cmd-ecs-cli-registry-creds-up-privregauth"></a>

When using the `ecs-cli registry-creds up` command to manage your private registry authentication credentials, there are certain fields that are specified using an input file\. You must specify a file name or path to an input file when using this command\.

Currently, the file supports the follow schema:

```
version: 1
registry_credentials:
  registry_name:
    secrets_manager_arn: string
    username: string
    password: string
    kms_key_id: string
    container_names:
      - string
```

The following are descriptions for each of these fields\.

registry\_name  
Used as the secret name when creating a new secret or updating an existing secret\. The secret name must be ASCII letters, digits, or any of the following characters: /\_\+=\.@\-\. The Amazon ECS CLI adds a prefix to the secret name to indicate that it was created by the CLI\. For more information, see [CreateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html)\.  
Required: No

secrets\_manager\_arn  
The full ARN of an existing secret\. Used to specify or update an existing secret\. Must be in the following format:  

```
arn:aws:secretsmanager:region:aws_account_id:secret:secret_name
```
Required: No

username  
Specifies the user name for the private registry\. We recommend using environment variables for the user name to ensure that no sensitive information is stored in the input file\. When using environment variables, use the format `${VAR_NAME}`\.  
Required: No

password  
Specifies the password for the private registry\. We recommend using environment variables for the password to ensure that no sensitive information is stored in the input file\. When using environment variables, use the format `${VAR_NAME}`\.  
Required: No

kms\_key\_id  
Specifies the ARN, Key ID, or alias of the AWS KMS customer master key \(CMK\) to be used to encrypt the secret\. For more information, see [CreateSecret](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_CreateSecret.html)\.  
Required: No

container\_names  
Corresponds to a service name in a Docker compose file\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md) or [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.  
Required: No

## Tagging Resources<a name="cmd-ecs-cli-registry-creds-up-tags"></a>

The Amazon ECS CLI supports adding metadata in the form of resource tags to your AWS resources\. Each tag consists of a key and an optional value\. Resource tags can be used for cost allocation, automation, and access control\. For more information, see [Tagging your Amazon ECS resources](ecs-using-tags.md)\.

When using the `ecs-cli registry-creds up` command, using the `--tags` flag enables you to add metadata tags to the Secrets Manager secrets and then IAM roles\.

**Note**  
Existing Secrets Manager secrets within your account will be tagged, but IAM roles can only be tagged during creation\. If you're using an existing IAM role, new tags can't be added\.

## Examples<a name="cmd-ecs-cli-registry-creds-up-examples"></a>

### Create a Secret with Private Registry Authentication Credentials<a name="cmd-ecs-cli-registry-creds-up-example-1"></a>

This example creates a secret with the private registry credentials specified in the `creds_input.yml` input file\.

Create a private registry credentials file, named `creds_input.yml` that contains the user name and password for the private registry as well as the name of the container that will use the private registry credentials\. We recommend using environment variables for the credentials to ensure that no sensitive information is stored in the input file\. The container name in this file corresponds to the service name `database` in the Docker compose file\.

```
version: '1'
registry_credentials:
  dockerhub: 
    username: ${MY_REPO_USERNAME}
    password: ${MY_REPO_PASSWORD}
    container_names:
      - database
```

**Important**  
We recommend using environment variables for the password to ensure that no sensitive information is stored in the input file\. If your input file contains sensitive information, make sure that you delete it after use\.

Create the secret\. This command creates a secret using the name from the input file, in this example it is `dockerhub`\. The Amazon ECS CLI adds a prefix to the secret name to indicate that it was created by the CLI\. You also specify the name of your task execution role\.

```
ecs-cli registry-creds up ./creds_input.yml --role-name secretsTaskExecutionRole
```

Output:

```
INFO[0000] Processing credentials for registry dockerhub...
INFO[0000] New credential secret created: arn:aws:secretsmanager:region:aws_account_id:secret:amazon-ecs-cli-setup-dockerhub-VeDqXm
INFO[0000] Creating resources for task execution role ecsTaskExecutionRole...
INFO[0000] Created new task execution role arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole
INFO[0000] Created new task execution role policy arn:aws:iam::aws_account_id:policy/amazon-ecs-cli-setup-bugBashRole-policy-20181023T210805Z
INFO[0000] Attached AWS managed policy arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy to role ecsTaskExecutionRole
INFO[0001] Attached new policy arn:aws:iam::aws_account_id:policy/amazon-ecs-cli-setup-bugBashRole-policy-20181023T210805Z to role ecsTaskExecutionRole
INFO[0001] Writing registry credential output to new file C:\Users\brandejo\regcreds\regCredTest\ecs-registry-creds_20181023T210805Z.yml
```

An output file is created by this command that contains the task execution role name, the ARN of the secret that was created, and the container name\. This file is specified using the `--registry-creds` option when using either the ecs\-cli compose or ecs\-cli compose service commands\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md) or [ecs\-cli compose service](cmd-ecs-cli-compose-service.md)\.

The following is an example output file:

```
version: "1"
registry_credential_outputs:
  task_execution_role: secretsTaskExecutionRole
  container_credentials:
    dockerhub:
      credentials_parameter: arn:aws:secretsmanager:region:aws_account_id:secret:amazon-ecs-cli-setup-dockerhub-bbHiEk
      container_names:
      - database
```

### Create a Secret with Private Registry Authentication Credentials That Use a KMS Key<a name="cmd-ecs-cli-registry-creds-up-example-2"></a>

This example creates a secret with the private registry credentials that are encrypted using a KMS key specified in the `creds_input.yml` input file\.

Create a private registry credentials file, named `creds_input.yml` that contains the user name and password for the private registry as well as the name of the container that will use the private registry credentials\. We recommend using environment variables for the credentials to ensure that no sensitive information is stored in the input file\. The specified KMS key ARN encrypts the values when storing the secret\. The container name in this file corresponds to the service name `database` in the Docker compose file\.

```
version: '1'
registry_credentials:
  dockerhub: 
    username: ${MY_REPO_USERNAME}
    password: ${MY_REPO_PASSWORD}
    kms_key_id: kmsKeyARN
    container_names:
      - database
```

**Important**  
We recommend using environment variables for the password to ensure that no sensitive information is stored in the input file\. If your input file contains sensitive information, make sure that you delete it after use\.

### Create Multiple Secrets For Multiple Private Registries<a name="cmd-ecs-cli-registry-creds-up-example-2"></a>

This example creates multiple secrets with the private registry credentials for multiple registries\.

Create a private registry credentials file, named `creds_input.yml` that contains the credentials from two different private registries\. Each set of credentials are used to create its own secret\. This example also shows two different containers using one secret\.

```
version: '1'
registry_credentials:
  dockerhub: 
    username: ${MY_REPO_USERNAME}
    password: ${MY_REPO_PASSWORD}
    container_names:
      - prod
      - dev
  quay.io: 
    username: ${MY_REPO_USERNAME}
    password: ${MY_REPO_PASSWORD}
    container_names:
      - database
```

**Important**  
We recommend using environment variables for the password to ensure that no sensitive information is stored in the input file\. If your input file contains sensitive information, make sure that you delete it after use\.