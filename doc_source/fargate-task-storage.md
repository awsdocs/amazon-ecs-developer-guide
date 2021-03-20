# Fargate task storage<a name="fargate-task-storage"></a>

For Amazon ECS tasks hosted on Fargate, the following storage types are supported:
+ Amazon EFS volumes for persistent storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.
+ Bind mounts for ephemeral storage\. For more information, see [Bind mounts](bind-mounts.md)\.

When provisioned, each Amazon ECS task on Fargate receives the following ephemeral storage for bind mounts\.

## Fargate tasks using platform version 1\.4\.0 or later<a name="fargate-task-storage-pv14"></a>

For Amazon ECS on Fargate tasks using platform version 1\.4\.0 or later, each task receives 20 GB of ephemeral storage\. The amount of storage is not adjustable\.

For tasks using platform version 1\.4\.0 or later that are launched on May 28, 2020 or later, the ephemeral storage is encrypted with an AES\-256 encryption algorithm using an AWS Fargate\-managed encryption key\.

## Fargate tasks using platform version 1\.3\.0 or earlier<a name="fargate-task-storage-pv13"></a>

For Amazon ECS on Fargate tasks using platform version 1\.3\.0 or earlier, each task receives the following ephemeral storage\.
+ 10 GB of Docker layer storage
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.