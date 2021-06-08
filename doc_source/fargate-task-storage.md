# Fargate task storage<a name="fargate-task-storage"></a>

When provisioned, each Amazon ECS task hosted on AWS Fargate receives the following ephemeral storage for bind mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.

## Fargate tasks using platform version 1\.4\.0 or later<a name="fargate-task-storage-pv14"></a>

By default, Amazon ECS tasks hosted on Fargate using platform version `1.4.0` or later receive a minimum of 20 GiB of ephemeral storage\. The total amount of ephemeral storage can be increased, up to a maximum of 200 GiB, by specifying the `ephemeralStorage` parameter in your task definition\.

The pulled, compressed, and the uncompressed container image for the task is stored on the ephemeral storage\. To determine the total amount of ephemeral storage your task has to use, you must subtract the amount of storage your container image uses from the total amount of ephemeral storage your task is allocated\.

For tasks using platform version `1.4.0` or later that are launched on May 28, 2020 or later, the ephemeral storage is encrypted with an AES\-256 encryption algorithm using an AWS owned encryption key\.

## Fargate tasks using platform version 1\.3\.0 or earlier<a name="fargate-task-storage-pv13"></a>

For Amazon ECS on Fargate tasks using platform version `1.3.0` or earlier, each task receives the following ephemeral storage\.
+ 10 GB of Docker layer storage
**Note**  
This amount includes both compressed and uncompressed container image artifacts\.
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.