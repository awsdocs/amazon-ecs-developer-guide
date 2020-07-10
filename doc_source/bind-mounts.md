# Bind mounts<a name="bind-mounts"></a>

With bind mounts, a file or directory on the host machine is mounted into a container\. Bind mount host volumes are supported for tasks using the EC2 launch type\.

To use bind mount host volumes with tasks using the EC2 launch type, specify a `host` and optional `sourcePath` value in your task definition\. For more information, see [Using bind mounts](https://docs.docker.com/storage/bind-mounts/)\.

**Note**  
For information on the task storage for Amazon ECS on Fargate tasks, see [Fargate Task Storage](fargate-task-storage.md)\.

Some common use cases for bind mounts are:
+ To provide persistent data volumes for use with containers
+ To define an empty, nonpersistent data volume and mount it on multiple containers on the same container instance
+ To share defined data volumes at different locations on different containers on the same container instance

## Specifying a Bind Mount in your Task Definition<a name="specify-volume-config"></a>

Before your containers can use bind mount host volumes, you must specify the volume and mount point configurations in your task definition\. This section describes the volume configuration for a container\. For tasks that use a bind mount host volume, specify a `host` value and optional `sourcePath` value\.

The following task definition JSON snippet shows the syntax for the `volumes` and `mountPoints` objects for a container:

```
{
   "family": "",
   ...
   "containerDefinitions" : [
      {
         "mountPoints" : [
            {
               "containerPath" : "/path/to/mount_volume",
               "sourceVolume" : "string"
            }
          ],
          "name" : "string"
       }
    ],
    ...
    "volumes" : [
       {
          "host" : {
             "sourcePath" : "string"
          },
          "name" : "string"
       }
    ]
}
```

`name`  
Type: String  
Required: No  
The name of the volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. This name is referenced in the `sourceVolume` parameter of container definition `mountPoints`\.

`host`  
Required: No  
This parameter is specified when using bind mounts\. To use Docker volumes, specify a `dockerVolumeConfiguration` instead\. The contents of the `host` parameter determine whether your bind mount data volume persists on the host container instance and where it is stored\. If the `host` parameter is empty, then the Docker daemon assigns a host path for your data volume, but the data is not guaranteed to persist after the containers associated with it stop running\.  
Bind mount host volumes are supported when using either the EC2 or Fargate launch types\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\.    
`sourcePath`  
Type: String  
Required: No  
When the `host` parameter is used, specify a `sourcePath` to declare the path on the host container instance that is presented to the container\. If this parameter is empty, then the Docker daemon has assigned a host path for you\. If the `host` parameter contains a `sourcePath` file location, then the data volume persists at the specified location on the host container instance until you delete it manually\. If the `sourcePath` value does not exist on the host container instance, the Docker daemon creates it\. If the location does exist, the contents of the source path folder are exported\.

`mountPoints`  
Type: Object Array  
Required: No  
The mount points for data volumes in your container\.   
This parameter maps to `Volumes` in the [Create a container](https://docs.docker.com/engine/api/v1.38/#operation/ContainerCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--volume` option to [https://docs.docker.com/engine/reference/commandline/run/](https://docs.docker.com/engine/reference/commandline/run/)\.  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers cannot mount directories on a different drive, and mount point cannot be across drives\.    
`sourceVolume`  
Type: String  
Required: Yes, when `mountPoints` are used  
The name of the volume to mount\.  
`containerPath`  
Type: String  
Required: Yes, when `mountPoints` are used  
The path on the container to mount the volume at\.  
`readOnly`  
Type: Boolean  
Required: No  
If this value is `true`, the container has read\-only access to the volume\. If this value is `false`, then the container can write to the volume\. The default value is `false`\.

## Examples<a name="bind-mount-examples"></a>

**To provide nonpersistent empty storage for containers using a bind mount**

In some cases, you want containers to share the same empty data volume, but you aren't interested in keeping the data after the task has finished\. For example, you may have two database containers that need to access the same scratch file storage location during a task\. This task can be achieved using either a Docker volume or a bind mount host volume\.

1. In the task definition `volumes` section, define a bind mount with the name `database_scratch`\.
**Note**  
Because the `database_scratch` bind mount does not specify a source path, the Docker daemon manages the bind mount for you\. When no containers reference this bind mount, the Amazon ECS container agent task cleanup service eventually deletes it \(by default, this happens 3 hours after the container exits, but you can configure this duration with the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` agent variable\)\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\. If you need this data to persist, specify a `sourcePath` value for the bind mount\.

   ```
     "volumes": [
       {
         "name": "database_scratch",
         "host": {}
       }
     ]
   ```

1. In the `containerDefinitions` section, create the database container definitions so that they mount the nonpersistent storage\.

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

**To provide persistent storage for containers using a bind mount**

When using bind mounts, if a `sourcePath` value is specified the data persists even after all containers that referenced it have stopped\. Any files that exist at the `sourcePath` are presented to the containers at the `containerPath` value, and any files that are written to the `containerPath` value are written to the `sourcePath` value on the container instance\.
**Important**  
Amazon ECS does not sync your storage across container instances\. Tasks that use persistent storage can be placed on any container instance in your cluster that has available capacity\. If your tasks require persistent storage after stopping and restarting, you should always specify the same container instance at task launch time with the AWS CLI [start\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/start-task.html) command\.

1. In the task definition `volumes` section, define a bind mount with `name` and `sourcePath` values\.

   ```
     "volumes": [
       {
         "name": "webdata",
         "host": {
           "sourcePath": "/ecs/webdata"
         }
       }
     ]
   ```

1. In the `containerDefinitions` section, define a container with `mountPoints` values that reference the name of the defined bind mount and the `containerPath` value to mount the bind mount at on the container\.

   ```
     "containerDefinitions": [
       {
         "name": "web",
         "image": "nginx",
         "cpu": 99,
         "memory": 100,
         "portMappings": [
           {
             "containerPort": 80,
             "hostPort": 80
           }
         ],
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "webdata",
             "containerPath": "/usr/share/nginx/html"
           }
         ]
       }
     ]
   ```

**To mount a defined volume on multiple containers**

You can define a data volume in a task definition and mount that volume at different locations on different containers\. For example, your host container has a website data folder at `/data/webroot`, and you may want to mount that data volume as read\-only on two different web servers that have different document roots\.

1. In the task definition `volumes` section, define a data volume with the name `webroot` and the source path `/data/webroot`\.

   ```
     "volumes": [
       {
         "name": "webroot",
         "host": {
           "sourcePath": "/data/webroot"
         }
       }
     ]
   ```

1. In the `containerDefinitions` section, define a container for each web server with `mountPoints` values that associate the `webroot` volume with the `containerPath` value pointing to the document root for that container\.

   ```
     "containerDefinitions": [
       {
         "name": "web-server-1",
         "image": "my-repo/ubuntu-apache",
         "cpu": 100,
         "memory": 100,
         "portMappings": [
           {
             "containerPort": 80,
             "hostPort": 80
           }
         ],
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "webroot",
             "containerPath": "/var/www/html",
             "readOnly": true
           }
         ]
       },
       {
         "name": "web-server-2",
         "image": "my-repo/sles11-apache",
         "cpu": 100,
         "memory": 100,
         "portMappings": [
           {
             "containerPort": 8080,
             "hostPort": 8080
           }
         ],
         "essential": true,
         "mountPoints": [
           {
             "sourceVolume": "webroot",
             "containerPath": "/srv/www/htdocs",
             "readOnly": true
           }
         ]
       }
     ]
   ```

**To mount volumes from another container using `volumesFrom`**

You can define one or more volumes on a container, and then use the `volumesFrom` parameter in a different container definition \(within the same task\) to mount all of the volumes from the `sourceContainer` at their originally defined mount points\. The `volumesFrom` parameter applies to volumes defined in the task definition, and those that are built into the image with a Dockerfile\.

1. \(Optional\) To share a volume that is built into an image, you need to build the image with the volume declared in a `VOLUME` instruction\. The following example Dockerfile uses an `httpd` image and then adds a volume and mounts it at `dockerfile_volume` in the Apache document root \(which is the folder used by the `httpd` web server\):

   ```
   FROM httpd
   VOLUME ["/usr/local/apache2/htdocs/dockerfile_volume"]
   ```

   You can build an image with this Dockerfile and push it to a repository, such as Docker Hub, and use it in your task definition\. The example `my-repo/httpd_dockerfile_volume` image used in the following steps was built with the above Dockerfile\.

1. Create a task definition that defines your other volumes and mount points for the containers\. In this example `volumes` section, you create an empty volume called `empty`, which the Docker daemon manages\. There is also a host volume defined called `host_etc`, which exports the `/etc` folder on the host container instance\.

   ```
   {
     "family": "test-volumes-from",
     "volumes": [
       {
         "name": "empty",
         "host": {}
       },
       {
         "name": "host_etc",
         "host": {
           "sourcePath": "/etc"
         }
       }
     ],
   ```

   In the container definitions section, create a container that mounts the volumes defined earlier\. In this example, the `web` container \(which uses the image built with a volume in the Dockerfile\) mounts the `empty` and `host_etc` volumes\.

   ```
   "containerDefinitions": [
       {
         "name": "web",
         "image": "my-repo/httpd_dockerfile_volume",
         "cpu": 100,
         "memory": 500,
         "portMappings": [
           {
             "containerPort": 80,
             "hostPort": 80
           }
         ],
         "mountPoints": [
           {
             "sourceVolume": "empty",
             "containerPath": "/usr/local/apache2/htdocs/empty_volume"
           },
           {
             "sourceVolume": "host_etc",
             "containerPath": "/usr/local/apache2/htdocs/host_etc"
           }
         ],
         "essential": true
       },
   ```

   Create another container that uses `volumesFrom` to mount all of the volumes that are associated with the `web` container\. All of the volumes on the `web` container are likewise mounted on the `busybox` container \(including the volume specified in the Dockerfile that was used to build the `my-repo/httpd_dockerfile_volume` image\)\.

   ```
       {
         "name": "busybox",
         "image": "busybox",
         "volumesFrom": [
           {
             "sourceContainer": "web"
           }
         ],
         "cpu": 100,
         "memory": 500,
         "entryPoint": [
           "sh",
           "-c"
         ],
         "command": [
           "echo $(date) > /usr/local/apache2/htdocs/empty_volume/date && echo $(date) > /usr/local/apache2/htdocs/host_etc/date && echo $(date) > /usr/local/apache2/htdocs/dockerfile_volume/date"
         ],
         "essential": false
       }
     ]
   }
   ```

   When this task is run, the two containers mount the volumes, and the `command` in the `busybox` container writes the date and time to a file called `date` in each of the volume folders\. The folders are then visible at the website displayed by the `web` container\.
**Note**  
Because the `busybox` container runs a quick command and then exits, it must be set as `"essential": false` in the container definition\. Otherwise, it stops the entire task when it exits\.