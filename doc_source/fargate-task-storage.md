# Fargate Task Storage<a name="fargate-task-storage"></a>

When provisioned, each Fargate task receives the following storage\. Task storage is ephemeral\. After a Fargate task stops, the storage is deleted\.
+ 10 GB of Docker layer storage
+ An additional 4 GB for volume mounts\. This can be mounted and shared among containers using the `volumes`, `mountPoints` and `volumesFrom` parameters in the task definition\.
**Note**  
The `host` and `sourcePath` parameters are not supported for Fargate tasks\.

For more information about Amazon ECS default service limits, see [Amazon ECS Service Limits](service_limits.md)\.

**To provide nonpersistent empty storage for containers in a Fargate task**

In this example, you may have two database containers that need to access the same scratch file storage location during a task\.

1. In the task definition `volumes` section, define a volume with the name `database_scratch`\.

   ```
     "volumes": [
       {
         "name": "database_scratch",
         "host": {}
       }
     ]
   ```

1. In the `containerDefinitions` section, create the database container definitions so they mount the nonpersistent storage\.

   ```
     "containerDefinitions": [
       {
         "name": "database1",
         "image": "my-repo/database",
         "cpu": 100,
         "memory": 100,
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "database_scratch",
             "containerPath": "/var/scratch"
           }
         ]
       },
       {
         "name": "database2",
         "image": "my-repo/database",
         "cpu": 100,
         "memory": 100,
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "database_scratch",
             "containerPath": "/var/scratch"
           }
         ]
       }
     ]
   ```