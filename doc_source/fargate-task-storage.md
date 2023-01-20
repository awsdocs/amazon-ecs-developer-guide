# Fargate task storage<a name="fargate-task-storage"></a>

When provisioned, each Amazon ECS task that are hosted on AWS Fargate receives the following ephemeral storage for bind mounts\. This can be mounted and shared among containers that use the `volumes`, `mountPoints`, and `volumesFrom` parameters in the task definition\.

## Fargate tasks using Windows platform version 1\.0\.0 or later<a name="fargate-task-storage-pvws1"></a>

By default, Amazon ECS tasks that are hosted on Fargate using platform version `1.0.0` or later receive a minimum of 20 GiB of ephemeral storage\. 

You cannot configure the ephemeral storage for Windows containers on Fargate\.

## Fargate tasks using Linux platform version 1\.4\.0 or later<a name="fargate-task-storage-pv14"></a>

By default, Amazon ECS tasks that are hosted on Fargate using platform version `1.4.0` or later receive a minimum of 20 GiB of ephemeral storage\. The total amount of ephemeral storage can be increased, up to a maximum of 200 GiB\. You can do this by specifying the `ephemeralStorage` parameter in your task definition\.

The pulled, compressed, and the uncompressed container image for the task is stored on the ephemeral storage\. To determine the total amount of ephemeral storage that your task has to use, you must subtract the amount of storage that your container image uses from the total amount of ephemeral storage your task is allocated\.

For tasks that use platform version `1.4.0` or later that are launched on May 28, 2020 or later, the ephemeral storage is encrypted with an AES\-256 encryption algorithm\. The algorithm uses an AWS owned encryption key\.

For tasks that use platform version `1.4.0` or later that are launched on November 18, 2022 or later, the ephemeral storage usage is reported through the task metadata endpoint\. Your applications in your tasks can query the task metadata endpoint version 4 to get their ephemeral storage reserved size and the amount used\. Each task can only query the usage of that task\.

 Additionally, the ephemeral storage reserved size and the amount used are sent to Amazon CloudWatch Container Insights if you turn on Container Insights\.

**Note**  
Fargate reserves space on disk\. It is only used by Fargate\. You aren't billed for it\. It isn't shown in these metrics\. However, you can see this additional storage in other tools such as `df`\.

## Fargate tasks using Linux platform version 1\.3\.0 or earlier<a name="fargate-task-storage-pv13"></a>

For Amazon ECS on Fargate tasks using platform version `1.3.0` or earlier, each task receives the following ephemeral storage\.
+ 10 GB of Docker layer storage
**Note**  
This amount includes both compressed and uncompressed container image artifacts\.
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints`, and `volumesFrom` parameters in the task definition\.