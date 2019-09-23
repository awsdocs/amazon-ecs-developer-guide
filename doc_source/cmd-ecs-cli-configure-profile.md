# ecs\-cli configure profile<a name="cmd-ecs-cli-configure-profile"></a>

Configures your AWS credentials in a named Amazon ECS profile, which is stored in the `~/.ecs/credentials` file\. If multiple profiles are created, you can change the profile used by default with the ecs\-cli configure profile default command\. For more information, see [ecs\-cli configure profile default](cmd-ecs-cli-configure-profile-default.md)\.

**Important**  
Some features described might only be available with the latest version of the Amazon ECS CLI\. For more information about obtaining the latest version, see [Installing the Amazon ECS CLI](ECS_CLI_installation.md)\.

You can configure your AWS credentials in several ways:
+ You can set the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` environment variables\. When you run ecs\-cli configure profile, the values of those variables are stored in the Amazon ECS CLI configuration file\.
+ You can pass credentials directly on the command line with the `--access-key`, `--secret-key`, and `--session-token` options\. 
+ You can provide the name of a new profile with the `--profile-name` flag\. If a profile name is not provided, then the profile is named `default`\.
+ The first profile configured is set as the default profile\. The Amazon ECS CLI uses credentials specified in this profile unless the `--ecs-profile` flag is used\.

## Working with Multiple Profiles<a name="ECS_CLI_multiple_profiles"></a>

The following should be noted when using multiple profiles:
+ Multiple profiles may be configured, but one is always the default\. This profile is used when an Amazon ECS CLI command is run that requires credentials\.
+ The first profile that is created is set as the default profile\.
+ To change the default profile, use the `ecs-cli configure profile default` command\. For more information, see [ecs\-cli configure profile default](cmd-ecs-cli-configure-profile-default.md)\.
+ A non\-default profile can be referenced in a command using the `--ecs-profile` flag\.

## Syntax<a name="cmd-ecs-cli-configure-profile-syntax"></a>

ecs\-cli configure profile \-\-profile\-name *profile\_name* \-\-access\-key *aws\_access\_key\_id* \-\-secret\-key *aws\_secret\_access\_key* \[\-\-session\-token *token*\] \[\-\-help\] 

## Options<a name="cmd-ecs-cli-configure-profile-options"></a>


| Name | Description | 
| --- | --- | 
|  `--profile-name profile_name`  |  Specifies the name of this ECS profile\. This is the name that can be referenced in commands using the `--ecs-profile` flag\. If this option is omitted, then the name is set to `default`\. Type: String Required: Yes  | 
|  `--access-key aws_access_key_id`  |  Specifies the AWS access key to use\. If the `AWS_ACCESS_KEY_ID` environment variable is set when ecs\-cli configure profile is run, then the AWS access key ID is set to the value of that environment variable\. Type: String Required: Yes  | 
|  `--secret-key aws_secret_access_key`  |  Specifies the AWS secret key to use\. If the `AWS_SECRET_ACCESS_KEY` environment variable is set when ecs\-cli configure profile is run, then the AWS secret access key is set to the value of that environment variable\. Type: String Required: Yes  | 
| `--session-token token` |  Specifies the AWS session token to use\. If the `AWS_SESSION_TOKEN` environment variable if it is set when ecs\-cli configure profile is run, then the AWS session token is set to the value of that environment variable\. For more information about using a session token for temporary access, see [Requesting Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_request.html)\. Type: String Required: No  | 
|  `--help, -h`  |  Shows the help text for the specified command\. Required: No  | 

## Examples<a name="cmd-ecs-cli-configure-profile-examples"></a>

### Example 1<a name="cmd-ecs-cli-configure-profile-example-1"></a>

This example configures the Amazon ECS CLI to create and use a profile named `default` with a set of access keys\.

```
ecs-cli configure profile --profile-name default --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY
```

Output:

```
INFO[0000] Saved ECS CLI profile configuration default.
```

### Example 2<a name="cmd-ecs-cli-configure-profile-example-2"></a>

This example configures the Amazon ECS CLI to create and use a profile named `default` with a set of access keys and an AWS session token\.

```
ecs-cli configure profile --profile-name default --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY --session-token $AWS_SESSION_TOKEN
```

Output:

```
INFO[0000] Saved ECS CLI profile configuration default.
```