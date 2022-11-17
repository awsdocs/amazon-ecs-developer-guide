# Amazon ECS container agent<a name="ECS_agent"></a>

The Amazon ECS container agent allows container instances to connect to your cluster\. The Amazon ECS container agent is included in the Amazon ECS\-optimized AMIs, but you can also install it on any Amazon EC2 instance that supports the Amazon ECS specification\. The Amazon ECS container agent is supported on Amazon EC2 instances and external instances \(on\-premises server or VM\)\. For more information about external instances, see [Updating the Amazon ECS agent on an external instance](ecs-anywhere-updates.md#ecs-anywhere-updates-ecsagent)\.

The source code for the Amazon ECS container agent is [available on GitHub](https://github.com/aws/amazon-ecs-agent)\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently support running modified copies of this software\.

**Note**  
For tasks using the Fargate launch type and platform version 1\.3\.0 and prior, the Amazon ECS container agent is installed on the AWS managed infrastructure used for these tasks\. If you are only using tasks with the Fargate launch type, no additional configuration is needed and the content in this topic does not apply\. For tasks using the Fargate and platform version 1\.4\.0 and later \(for Linux \) or 1\.0\.0 or later \(for Windows\), the Fargate container agent is used\. For more information, see [AWS Fargate platform versions](https://docs.aws.amazon.com/AmazonECS/latest/userguide/platform_versions.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

**Topics**
+ [Installing the Amazon ECS container agent](ecs-agent-install.md)
+ [Amazon ECS Linux container agent versions](ecs-agent-versions.md)
+ [Amazon EC2 Windows containers](ECS_Windows.md)
+ [Updating the Amazon ECS container agent](ecs-agent-update.md)
+ [Amazon ECS container agent configuration](ecs-agent-config.md)
+ [Private registry authentication for container instances](private-auth-container-instances.md)
+ [Automated task and image cleanup](automated_image_cleanup.md)
+ [Amazon ECS container metadata file](container-metadata.md)
+ [Amazon ECS task metadata endpoint](task-metadata-endpoint.md)
+ [Amazon ECS container agent endpoint](ecs-container-agent-endpoint.md)
+ [Amazon ECS container agent introspection](ecs-agent-introspection.md)
+ [HTTP proxy configuration](http_proxy_config.md)
+ [Using gMSAs for Windows Containers](windows-gmsa.md)