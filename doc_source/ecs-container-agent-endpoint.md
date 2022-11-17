# Amazon ECS container agent endpoint<a name="ecs-container-agent-endpoint"></a>

**Important**  
  
If you are using Amazon ECS tasks hosted on AWS Fargate, see [Container agent endpoint](https://docs.aws.amazon.com/AmazonECS/latest/userguide/container-agent-endpoint-fargate.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

The Amazon ECS container agent automatically injects the `ECS_AGENT_URI` environment variable into the containers of Amazon ECS tasks to provide a method to interact with the container agent API endpoint\.

This applies to Amazon ECS tasks launched on Amazon EC2 Linux and Windows instances that are running at least version `1.65.0` of the Amazon ECS container agent\.

**Note**  
You can add support for this feature on Amazon EC2 instances using older versions of the Amazon ECS container agent by updating the agent to the latest version\. For more information, see [Updating the Amazon ECS container agent](ecs-agent-update.md)\.