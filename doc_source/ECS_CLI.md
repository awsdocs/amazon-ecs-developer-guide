# Using the Amazon ECS Command Line Interface<a name="ECS_CLI"></a>

The Amazon Elastic Container Service \(Amazon ECS\) command line interface \(CLI\) provides high\-level commands to simplify creating, updating, and monitoring clusters and tasks from a local development environment\. The Amazon ECS CLI supports Docker Compose files, a popular open\-source specification for defining and running multi\-container applications\. Use the ECS CLI as part of your everyday development and testing cycle as an alternative to the AWS Management Console\.

**Important**  
At this time, the latest version of the Amazon ECS CLI only supports the major versions of [Docker Compose file syntax](https://docs.docker.com/compose/compose-file/#versioning) versions 1, 2, and 3\. The version specified in the compose file must be the string `"1"`, `"1.0"`, `"2"`, `"2.0"`, `"3"`, or `"3.0"`\. Docker Compose minor versions are not supported\.

The latest version of the Amazon ECS CLI is 1\.17\.0\. For release notes, see [Changelog](https://github.com/aws/amazon-ecs-cli/blob/master/CHANGELOG.md)\.

**Note**  
The source code for the Amazon ECS CLI is [available on GitHub](https://github.com/aws/amazon-ecs-cli)\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently support running modified copies of this software\.

Learn how to use high\-level, application\-first commands to model, create, release and manage containerized applications from a local development environment at [Getting started with AWS Copilot by deploying an Amazon ECS application](getting-started-aws-copilot-cli.md)\.

**Topics**
+ [Installing the Amazon ECS CLI](ECS_CLI_installation.md)
+ [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)
+ [Migrating Configuration Files](ECS_CLI_migrating_config_files.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the Amazon ECS CLI](ecs-cli-tutorial-fargate.md)
+ [Tutorial: Creating a Cluster with an EC2 Task Using the Amazon ECS CLI](ecs-cli-tutorial-ec2.md)
+ [Tutorial: Creating an Amazon ECS Service That Uses Service Discovery Using the Amazon ECS CLI](ecs-cli-tutorial-servicediscovery.md)
+ [Amazon ECS Command Line Reference](ECS_CLI_reference.md)