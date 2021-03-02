# Fargate Task Storage<a name="fargate-task-storage"></a>

For Fargate tasks, the following storage types are supported:
+ Amazon EFS volumes for persistent storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.
+ Ephemeral storage for nonpersistent storage\.

When provisioned, each Amazon ECS task on Fargate receives the following ephemeral storage\.

## Fargate tasks using platform version 1\.4\.0 or later<a name="fargate-task-storage-pv14"></a>

For Amazon ECS on Fargate tasks using platform version 1\.4\.0 or later, each task receives 20 GB of ephemeral storage\. The amount of storage is not adjustable\.

For tasks using platform version 1\.4\.0 or later that are launched on May 28, 2020 or later, the ephemeral storage is encrypted with an AES\-256 encryption algorithm using an AWS Fargate\-managed encryption key\.

## Fargate tasks using platform version 1\.3\.0 or earlier<a name="fargate-task-storage-pv13"></a>

For Amazon ECS on Fargate tasks using platform version 1\.3\.0 or earlier, each task receives the following ephemeral storage\.
+ 10 GB of Docker layer storage
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.
**Note**  
The `host` and `sourcePath` parameters are not supported for Fargate tasks\.

## Example task definition<a name="fargate-task-storage-example"></a>

**To provide nonpersistent empty storage for containers in a Fargate task**

In this example, you have two application containers that need to access the same scratch file storage location\.

1. In the task definition `volumes` section, define a volume with the name `application_scratch`\.

   ```
     "volumes": [
       {
         "name": "application_scratch",
         "host": {}
       }
     ]
   ```

1. In the `containerDefinitions` section, create the application container definitions so they mount the nonpersistent storage\.

   ```
     "containerDefinitions": [
       {
         "name": "application1",
         "image": "my-repo/application",
         "cpu": 100,
         "memory": 100,
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "application_scratch",
             "containerPath": "/var/scratch"
           }
         ]
       },
       {
         "name": "application2",
         "image": "my-repo/application",
         "cpu": 100,
         "memory": 100,
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "application_scratch",
             "containerPath": "/var/scratch"
           }
         ]
       }
     ]
   ```