# Container Instance AMIs<a name="container_instance_AMIs"></a>

The basic Amazon Elastic Container Service \(Amazon ECS\) container instance specification consists of the following:

Required

+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.

+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.

+ A Docker daemon running at least version 1\.5\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS agent version that you are using\. For more information, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.

Recommended

+ An initialization and nanny process to run and monitor the Amazon ECS agent\. The Amazon ECS\-optimized AMI uses the `ecs-init` upstart process\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

The Amazon ECS\-optimized AMI is preconfigured with these requirements and recommendations\. We recommend that you use the Amazon ECS\-optimized AMI for your container instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\.