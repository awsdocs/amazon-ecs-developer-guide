# Using the Amazon ECS Command Line Interface<a name="ECS_CLI"></a>

The Amazon Elastic Container Service \(Amazon ECS\) command line interface \(CLI\) provides high\-level commands to simplify creating, updating, and monitoring clusters and tasks from a local development environment\. The Amazon ECS CLI supports [Docker Compose](https://docs.docker.com/compose/) files \([Version 1](https://docs.docker.com/compose/compose-file/compose-file-v1/), [Version 2](https://docs.docker.com/compose/compose-file/compose-file-v2/), and [Version 3](https://docs.docker.com/compose/compose-file/)\), a popular open\-source specification for defining and running multi\-container applications\. Use the CLI as part of your everyday development and testing cycle as an alternative to the AWS Management Console\.

The latest version of the Amazon ECS CLI is 1\.6\.0\. For release notes, see [Changelog](https://github.com/aws/amazon-ecs-cli/blob/master/CHANGELOG.md)\.

**Note**  
The source code for the Amazon ECS CLI is [available on GitHub](https://github.com/aws/amazon-ecs-cli)\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently support running modified copies of this software\.

**Topics**
+ [Installing the Amazon ECS CLI](ECS_CLI_installation.md)
+ [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)
+ [Migrating Configuration Files](ECS_CLI_migrating_config_files.md)
+ [Tutorial: Creating a Cluster with a Fargate Task Using the ECS CLI](ECS_CLI_tutorial_fargate.md)
+ [Tutorial: Creating a Cluster with an EC2 Task Using the ECS CLI](ECS_CLI_tutorial_EC2.md)
+ [Amazon ECS Command Line Reference](ECS_CLI_reference.md)