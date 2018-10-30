# Configuring the Amazon ECS CLI<a name="ECS_CLI_Configuration"></a>

The Amazon ECS CLI requires some basic configuration information before you can use it, such as your AWS credentials, the AWS Region in which to create your cluster, and the name of the Amazon ECS cluster to use\. Configuration information is stored in the `~/.ecs` directory on macOS and Linux systems and in `C:\Users\<username>\AppData\local\ecs` on Windows systems\.

**To configure the Amazon ECS CLI**

1. Set up a CLI profile with the following command, substituting *`profile_name`* with your desired profile name, `$AWS_ACCESS_KEY_ID` and `$AWS_SECRET_ACCESS_KEY` environment variables with your AWS credentials\.

   ```
   ecs-cli configure profile --profile-name profile_name --access-key $AWS_ACCESS_KEY_ID --secret-key $AWS_SECRET_ACCESS_KEY
   ```

1. Complete the configuration with the following command, substituting *`launch_type`* with the task launch type you want to use by default, *`region_name`* with your desired AWS region, *`cluster_name`* with the name of an existing Amazon ECS cluster or a new cluster to use, and `configuration_name` for the name you'd like to give this configuration\.

   ```
   ecs-cli configure --cluster cluster_name --default-launch-type launch_type --region region_name --config-name configuration_name
   ```

After you have installed and configured the CLI, you can try the [Tutorial: Creating a Cluster with a Fargate Task Using the Amazon ECS CLI](ecs-cli-tutorial-fargate.md)\. For more information, see the [Amazon ECS Command Line Reference](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Profiles<a name="ECS_CLI_profiles"></a>

The Amazon ECS CLI supports the configuring of multiple sets of AWS credentials as named *profiles* using the ecs\-cli configure profile command\. A default profile can be set by using the ecs\-cli configure profile default command\. These profiles can then be referenced when you run Amazon ECS CLI commands that require credentials using the `--ecs-profile` flag otherwise the default profile is used\.

For more information, see the [Amazon ECS Command Line Reference](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Cluster Configurations<a name="ECS_CLI_cluster_configurations"></a>

A cluster configuration is a set of fields that describes an Amazon ECS cluster including the name of the cluster and the region\. A default cluster configuration can be set by using the ecs\-cli configure default command\. The Amazon ECS CLI supports the configuring of multiple named cluster configurations using the `--config-name` option\.

For more information, see the [Amazon ECS Command Line Reference](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Order of Precedence<a name="ECS_CLI_order"></a>

There are multiple methods for passing both the credentials and the region in an Amazon ECS CLI command\. The following is the order of precedence for each of these\.

The order of precedence for credentials is:

1. Amazon ECS CLI profile flags:

   1. ECS profile \(`--ecs-profile`\)

   1. AWS profile \(`--aws-profile`\)

1. Environment variables:

   1. `ECS_PROFILE`

   1. `AWS_PROFILE`

   1. `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN`

1. ECS config‐attempts to fetch credentials from the default ECS profile\.

1. Default AWS profile—Attempts to use credentials \(`aws_access_key_id`, `aws_secret_access_key`\) or `assume_role` \(`role_arn`, `source_profile`\) from the AWS profile name\.

   1. `AWS_DEFAULT_PROFILE` environment variable \(defaults to `default`\)\.

1. EC2 instance role

The order of precedence for Region is:

1. Amazon ECS CLI flags:

   1. Region flag \(`--region`\)

   1. Cluster config flag \(`--cluster-config`\)

1. ECS config‐attempts to fetch the Region from the default ECS profile\.

1. Environment variables—Attempts to fetch the region from the following environment variables:

   1. `AWS_REGION`

   1. `AWS_DEFAULT_REGION`

1. AWS profile‐attempts to use the region from the AWS profile name:

   1. `AWS_PROFILE` environment variable

   1. `AWS_DEFAULT_PROFILE` environment variable \(defaults to `default`\)