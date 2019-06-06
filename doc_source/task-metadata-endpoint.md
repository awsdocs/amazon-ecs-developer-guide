# Amazon ECS Task Metadata Endpoint<a name="task-metadata-endpoint"></a>

The Amazon ECS container agent provides a method to retrieve various task metadata and [Docker stats](https://docs.docker.com/engine/api/v1.30/#operation/ContainerStats)\. This is referred to as the task metadata endpoint\.  The following versions are available:
+ Task metadata endpoint version 3 – Available for tasks that use the Fargate launch type on platform version v1\.3\.0 or later and tasks that use the EC2 launch type and are launched on Amazon EC2 infrastructure running at least version 1\.21\.0 of the Amazon ECS container agent\. For more information, see [Task Metadata Endpoint version 3](task-metadata-endpoint-v3.md)\.
+ Task metadata endpoint version 2 – Available for tasks that use the Fargate launch type on platform version v1\.1\.0 or later and tasks that use the EC2 launch type that also use the `awsvpc` network mode and are launched on Amazon EC2 infrastructure running at least version 1\.17\.0 of the Amazon ECS container agent\. For more information, see [Task Metadata Endpoint version 2](task-metadata-endpoint-v2.md)\.



For information about a sample Go application that queries the metadata and stats API endpoints, see [https://github\.com/aws/amazon\-ecs\-agent/blob/2bf4348a0ff89e23be4e82a6c5ff28edf777092c/misc/taskmetadata\-validator/taskmetadata\-validator\.go](https://github.com/aws/amazon-ecs-agent/blob/2bf4348a0ff89e23be4e82a6c5ff28edf777092c/misc/taskmetadata-validator/taskmetadata-validator.go)\.

**Topics**
+ [Task Metadata Endpoint version 3](task-metadata-endpoint-v3.md)
+ [Task Metadata Endpoint version 2](task-metadata-endpoint-v2.md)