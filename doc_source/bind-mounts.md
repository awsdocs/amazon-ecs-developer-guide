# Bind mounts<a name="bind-mounts"></a>

With bind mounts, a file or directory on a host, such as an Amazon EC2 instance or AWS Fargate, is mounted into a container\. Bind mounts are supported for tasks that are hosted on both Fargate and Amazon EC2 instances\. By default, bind mounts are tied to the lifecycle of the container that uses them\. After all of the containers that use a bind mount are stopped, such as when a task is stopped, the data is removed\. For tasks that are hosted on Amazon EC2 instances, the data can be tied to the lifecycle of the host Amazon EC2 instance by specifying a `host` and optional `sourcePath` value in your task definition\. For more information, see [Using bind mounts](https://docs.docker.com/storage/bind-mounts/) in the Docker documentation\.

The following are common use cases for bind mounts\.
+ To provide an empty data volume to mount in one or more containers\.
+ To mount a host data volume in one or more containers\.
+ To share a data volume from a source container with other containers in the same task\.
+ To expose a path and its contents from a Dockerfile to one or more containers\.

## Considerations when using bind mounts<a name="bind-mount-considerations"></a>

When using bind mounts, consider the following\.
+ For tasks that are hosted on AWS Fargate using platform version `1.4.0` or later \(Linux\) or `1.0.0` or later \(Windows\), by default they receive a minimum of 20 GiB of ephemeral storage for bind mounts\. For Linux tasks, the total amount of ephemeral storage can be increased to a maximum of 200 GiB by specifying the `ephemeralStorage` object in your task definition\.
+ To expose files from a Dockerfile to a data volume when a task is run, the Amazon ECS data plane looks for a `VOLUME` directive\. If the absolute path that's specified in the `VOLUME` directive is the same as the `containerPath` that's specified in the task definition, the data in the `VOLUME` directive path is copied to the data volume\. In the following Dockerfile example, a file that's named `examplefile` in the `/var/log/exported` directory is written to the host and then mounted inside the container\.

  ```
  FROM public.ecr.aws/amazonlinux/amazonlinux:latest
  RUN mkdir -p /var/log/exported
  RUN touch /var/log/exported/examplefile
  VOLUME ["/var/log/exported"]
  ```

  By default, the volume permissions are set to `0755` and the owner as `root`\. You can customize these permissions in the Dockerfile\. The following example defines the owner of the directory as `node`\.

  ```
  FROM public.ecr.aws/amazonlinux/amazonlinux:latest
  RUN yum install -y shadow-utils && yum clean all
  RUN useradd node
  RUN mkdir -p /var/log/exported && chown node:node /var/log/exported
  RUN touch /var/log/exported/examplefile
  USER node
  VOLUME ["/var/log/exported"]
  ```
+ For tasks that are hosted on Amazon EC2 instances, when a `host` and `sourcePath` value aren't specified, the Docker daemon manages the bind mount for you\. When no containers reference this bind mount, the Amazon ECS container agent task cleanup service eventually deletes it\. By default, this happens three hours after the container exits\. However, you can configure this duration with the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` agent variable\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\. If you need this data to persist beyond the lifecycle of the container, specify a `sourcePath` value for the bind mount\.

## Specifying a bind mount in your task definition<a name="specify-volume-config"></a>

For Amazon ECS tasks that are hosted on either Fargate or Amazon EC2 instances, the following task definition JSON snippet shows the syntax for the `volumes`, `mountPoints`, and `ephemeralStorage` objects for a task definition\.

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
          "name" : "string"
       }
    ],
    "ephemeralStorage": {
	   "sizeInGiB": integer
    }
}
```

For Amazon ECS tasks that are hosted on Amazon EC2 instances, you can use the optional `host` parameter and a `sourcePath` when specifying the task volume details\. When it's specified, it ties the bind mount to the lifecycle of the task rather than the container\.

```
"volumes" : [
    {
        "host" : {
            "sourcePath" : "string"
        },
        "name" : "string"
    }
]
```

The following describes each task definition parameter in more detail\.

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

`ephemeralStorage`  
Type: Object  
Required: No  
The amount of ephemeral storage to allocate for the task\. This parameter is used to expand the total amount of ephemeral storage available, beyond the default amount, for tasks hosted on AWS Fargate using platform version `1.4.0` or later \(Linux\) or `1.0.0` or later \(Windows\)\.  
You can use the Copilot CLI, CloudFormation, the AWS SDK or the CLI to specify ephemeral storage for a bind mount\.

## Bind mount examples<a name="bind-mount-examples"></a>

The following examples cover the most common use cases for using a bind mount for your containers\.

**To allocate an increased amount of ephemeral storage space for a Fargate task**

For Amazon ECS tasks that are hosted on Fargate using platform version `1.4.0` or later \(Linux\) or `1.0.0` \(Windows\), you can allocate more than the default amount of ephemeral storage for the containers in your task to use\. This example can be incorporated into the other examples to allocate more ephemeral storage for your Fargate tasks\.
+ In the task definition, define an `ephemeralStorage` object\. The `sizeInGiB` must be an integer between the values of `21` and `200` and is expressed in GiB\.

  ```
  "ephemeralStorage": {
      "sizeInGiB": integer
  }
  ```

**To provide an empty data volume for one or more containers**

In some cases, you want to provide the containers in a task some scratch space\. For example, you might have two database containers that need to access the same scratch file storage location during a task\. This can be achieved using a bind mount\.

1. In the task definition `volumes` section, define a bind mount with the name `database_scratch`\.

   ```
     "volumes": [
       {
         "name": "database_scratch",
       }
     ]
   ```

1. In the `containerDefinitions` section, create the database container definitions\. This is so that they mount the volume\.

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

**To expose a path and its contents in a Dockerfile to a container**

In this example, you have a Dockerfile that writes data that you want to mount inside a container\. This example works for tasks that are hosted on Fargate or Amazon EC2 instances\.

1. Create a Dockerfile\. The following example uses the public Amazon Linux 2 container image and creates a file that's named `examplefile` in the `/var/log/exported` directory that we want to mount inside the container\. The `VOLUME` directive should specify an absolute path\.

   ```
   FROM public.ecr.aws/amazonlinux/amazonlinux:latest
   RUN mkdir -p /var/log/exported
   RUN touch /var/log/exported/examplefile
   VOLUME ["/var/log/exported"]
   ```

   By default, the volume permissions are set to `0755` and the owner as `root`\. These permissions can be changed in the Dockerfile\. In the following example, the owner of the `/var/log/exported` directory is set to `node`\.

   ```
   FROM public.ecr.aws/amazonlinux/amazonlinux:latest
   RUN yum install -y shadow-utils && yum clean all
   RUN useradd node
   RUN mkdir -p /var/log/exported && chown node:node /var/log/exported
   RUN touch /var/log/exported/examplefile
   USER node
   VOLUME ["/var/log/exported"]
   ```

1. In the task definition `volumes` section, define a volume with the name `application_logs`\.

   ```
     "volumes": [
       {
         "name": "application_logs",
       }
     ]
   ```

1. In the `containerDefinitions` section, create the application container definitions\. This is so they mount the storage\. The `containerPath` value must match the absolute path that's specified in the `VOLUME` directive from the Dockerfile\.

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
             "sourceVolume": "application_logs",
             "containerPath": "/var/log/exported"
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
             "sourceVolume": "application_logs",
             "containerPath": "/var/log/exported"
           }
         ]
       }
     ]
   ```

**To provide an empty data volume for a container that's tied to the lifecycle of the host Amazon EC2 instance**

For tasks that are hosted on Amazon EC2 instances, you can use bind mounts and have the data tied to the lifecycle of the host Amazon EC2 instance\. You can do this by using the `host` parameter and specifying a `sourcePath` value\. Any files that exist at the `sourcePath` are presented to the containers at the `containerPath` value\. Any files that are written to the `containerPath` value are written to the `sourcePath` value on the host Amazon EC2 instance\.
**Important**  
Amazon ECS doesn't sync your storage across Amazon EC2 instances\. Tasks that use persistent storage can be placed on any Amazon EC2 instance in your cluster that has available capacity\. If your tasks require persistent storage after stopping and restarting, always specify the same Amazon EC2 instance at task launch time with the AWS CLI [start\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/start-task.html) command\. You can also use Amazon EFS volumes for persistent storage\. For more information, see [Amazon EFS volumes](efs-volumes.md)\.

1. In the task definition `volumes` section, define a bind mount with `name` and `sourcePath` values\. In the following example, the host Amazon EC2 instance contains data at `/ecs/webdata` that you want to mount inside the container\.

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

1. In the `containerDefinitions` section, define a container with a `mountPoints` value that references the name of the bind mount and the `containerPath` value to mount the bind mount at on the container\.

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

**To mount a defined volume on multiple containers at different locations**

You can define a data volume in a task definition and mount that volume at different locations on different containers\. For example, your host container has a website data folder at `/data/webroot`\. You might want to mount that data volume as read\-only on two different web servers that have different document roots\.

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

For tasks hosted on Amazon EC2 instances, you can define one or more volumes on a container, and then use the `volumesFrom` parameter in a different container definition within the same task to mount all of the volumes from the `sourceContainer` at their originally defined mount points\. The `volumesFrom` parameter applies to volumes defined in the task definition, and those that are built into the image with a Dockerfile\.

1. \(Optional\) To share a volume that is built into an image, use the `VOLUME` instruction in the Dockerfile\. The following example Dockerfile uses an `httpd` image, and then adds a volume and mounts it at `dockerfile_volume` in the Apache document root\. It is the folder used by the `httpd` web server\.

   ```
   FROM httpd
   VOLUME ["/usr/local/apache2/htdocs/dockerfile_volume"]
   ```

   You can build an image with this Dockerfile and push it to a repository, such as Docker Hub, and use it in your task definition\. The example `my-repo/httpd_dockerfile_volume` image that's used in the following steps was built with the above Dockerfile\.

1. Create a task definition that defines your other volumes and mount points for the containers\. In this example `volumes` section, you create an empty volume called `empty`, which the Docker daemon manages\. There's also a host volume defined that's called `host_etc`\. It exports the `/etc` folder on the host container instance\.

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

   In the container definitions section, create a container that mounts the volumes defined earlier\. In this example, the `web` container mounts the `empty` and `host_etc` volumes\. This is the container that uses the image built with a volume in the Dockerfile\.

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

   Create another container that uses `volumesFrom` to mount all of the volumes that are associated with the `web` container\. All of the volumes on the `web` container are likewise mounted on the `busybox` container\. This includes the volume that's specified in the Dockerfile that was used to build the `my-repo/httpd_dockerfile_volume` image\.

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

   When this task is run, the two containers mount the volumes, and the `command` in the `busybox` container writes the date and time to a file\. This file is called `date` in each of the volume folders\. The folders are then visible at the website displayed by the `web` container\.
**Note**  
Because the `busybox` container runs a quick command and then exits, it must be set as `"essential": false` in the container definition\. Otherwise, it stops the entire task when it exits\.