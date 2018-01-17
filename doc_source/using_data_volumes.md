# Using Data Volumes in Tasks<a name="using_data_volumes"></a>

There are several use cases for using data volumes in Amazon ECS task definitions\. Some common examples are to provide persistent data volumes for use with containers, to define an empty, nonpersistent data volume and mount it on multiple containers on the same container instance, and to share defined data volumes at different locations on different containers on the same container instance\.

**Note**  
Prior to the release of the Amazon ECS\-optimized AMI version 2017\.03\.a, only file systems that were available when the Docker daemon was started are available to Docker containers\. You can use the latest Amazon ECS\-optimized AMI to avoid this limitation, or you can upgrade the `docker` package to the latest version and restart Docker\.

**To provide persistent data volumes for containers**

When a volume is defined with a `sourcePath` value, the data volume persists even after all containers that referenced it have stopped\. Any files that exist at the `sourcePath` are presented to the containers at the `containerPath` value, and any files that are written to the `containerPath` value by running containers that mount the data volume are written to the `sourcePath` value on the container instance\.
**Important**  
Amazon ECS does not sync your data volumes across container instances\. Tasks that use persistent data volumes can be placed on any container instance in your cluster that has available capacity\. If your tasks require persistent data volumes after stopping and restarting, you should always specify the same container instance at task launch time with the AWS CLI [start\-task](http://docs.aws.amazon.com/cli/latest/reference/ecs/start-task.html) command\.

1. In the task definition `volumes` section, define a data volume with `name` and `sourcePath` values\.

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

1. In the `containerDefinitions` section, define a container with `mountPoints` that reference the name of the defined volume and the `containerPath` value to mount the volume at on the container\.

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

**To provide nonpersistent empty data volumes for containers**

In some cases, you want containers to share the same empty data volume, but you aren't interested in keeping the data after the task has finished\. For example, you may have two database containers that need to access the same scratch file storage location during a task\.

1. In the task definition `volumes` section, define a data volume with the name `database_scratch`\.
**Note**  
Because the `database_scratch` volume does not specify a source path, the Docker daemon manages the volume for you\. When no containers reference this volume, the Amazon ECS container agent task cleanup service eventually deletes it \(by default, this happens 3 hours after the container exits, but you can configure this duration with the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` agent variable\)\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\. If you need this data to persist, specify a `sourcePath` value for the volume\.

   ```
     "volumes": [
       {
         "name": "database_scratch",
         "host": {}
       }
     ]
   ```

1. In the `containerDefinitions` section, create the database container definitions so they mount the nonpersistent data volumes\.

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

1. Create a task definition that defines your other volumes and mount points for the containers\. In this example `volumes` section, you create an empty volume called `empty`, which the Docker daemon will manage\. There is also a host volume defined called `host_etc`, which exports the `/etc` folder on the host container instance\.

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

   Create another container that uses `volumesFrom` to mount all of the volumes that are associated with the `web` container\. All of the volumes on the `web` container will likewise be mounted on the `busybox` container \(including the volume specified in the Dockerfile that was used to build the `my-repo/httpd_dockerfile_volume` image\)\.

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

   When this task is run, the two containers mount the volumes, and the `command` in the `busybox` container writes the date and time to a file called `date` in each of the volume folders, which are then visible at the website displayed by the `web` container\.
**Note**  
Because the `busybox` container runs a quick command and then exits, it needs to be set as `"essential": false` in the container definition to prevent it from stopping the entire task when it exits\.