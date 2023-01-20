# Docker volumes<a name="docker-volumes"></a>

When using Docker volumes, the built\-in `local` driver or a third\-party volume driver can be used\. Docker volumes are managed by Docker and a directory is created in `/var/lib/docker/volumes` on the container instance that contains the volume data\.

To use Docker volumes, specify a `dockerVolumeConfiguration` in your task definition\. For more information, see [Using Volumes](https://docs.docker.com/storage/volumes/)\.

Some common use cases for Docker volumes are the following:
+ To provide persistent data volumes for use with containers
+ To share a defined data volume at different locations on different containers on the same container instance
+ To define an empty, nonpersistent data volume and mount it on multiple containers within the same task
+ To provide a data volume to your task that's managed by a third\-party driver

## Docker volume considerations<a name="docker-volume-considerations"></a>

Consider the following when using Docker volumes:
+ Docker volumes are only supported when using the EC2 launch type or external instances\.
+ Windows containers only support the use of the `local` driver\.
+ If a third\-party driver is used, make sure it's installed and active on the container instance before the container agent is started\. If the third\-party driver isn't active before the agent is started, you can restart the container agent using one of the following commands:
  + For the Amazon ECS\-optimized Amazon Linux 2 AMI:

    ```
    sudo systemctl restart ecs
    ```
  + For the Amazon ECS\-optimized Amazon Linux AMI:

    ```
    sudo stop ecs && sudo start ecs
    ```

## Specifying a Docker volume in your task definition<a name="specify-volume-config"></a>

Before your containers can use data volumes, you must specify the volume and mount point configurations in your task definition\. This section describes the volume configuration for a container\. For tasks that use a Docker volume, specify a `dockerVolumeConfiguration`\. For tasks that use a bind mount host volume, specify a `host` and optional `sourcePath`\.

The following task definition JSON shows the syntax for the `volumes` and `mountPoints` objects for a container\.

```
{
    "containerDefinitions": [
        {
            "mountPoints": [
                {
                    "sourceVolume": "string",
                    "containerPath": "/path/to/mount_volume",
                    "readOnly": boolean
                }
            ]
        }
    ],
    "volumes": [
        {
            "name": "string",
            "dockerVolumeConfiguration": {
                "scope": "string",
                "autoprovision": boolean,
                "driver": "string",
                "driverOpts": {
                    "key": "value"
                },
                "labels": {
                    "key": "value"
                }
            }
        }
    ]
}
```

`name`  
Type: String  
Required: No  
The name of the volume\. Up to 255 letters \(uppercase and lowercase\), numbers, hyphens, and underscores are allowed\. This name is referenced in the `sourceVolume` parameter of container definition `mountPoints`\.

`dockerVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when using Docker volumes\. Docker volumes are only supported when using the EC2 launch type\. Windows containers only support the use of the `local` driver\. To use bind mounts, specify a `host` instead\.    
`scope`  
Type: String  
Valid Values: `task` \| `shared`  
Required: No  
The scope for the Docker volume, which determines its lifecycle\. Docker volumes that are scoped to a `task` are automatically provisioned when the task starts and destroyed when the task stops\. Docker volumes that are scoped as `shared` persist after the task stops\.  
`autoprovision`  
Type: Boolean  
Default value: `false`  
Required: No  
If this value is `true`, the Docker volume is created if it does not already exist\. This field is only used if the `scope` is `shared`\. If the `scope` is `task` then this parameter must either be omitted or set to `false`\.  
`driver`  
Type: String  
Required: No  
The Docker volume driver to use\. The driver value must match the driver name provided by Docker because it is used for task placement\. If the driver was installed using the Docker plugin CLI, use `docker plugin ls` to retrieve the driver name from your container instance\. If the driver was installed using another method, use Docker plugin discovery to retrieve the driver name\. For more information, see [Docker plugin discovery](https://docs.docker.com/engine/extend/plugin_api/#plugin-discovery)\. This parameter maps to `Driver` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--driver` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.  
`driverOpts`  
Type: String  
Required: No  
A map of Docker driver specific options to pass through\. This parameter maps to `DriverOpts` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--opt` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.  
`labels`  
Type: String  
Required: No  
Custom metadata to add to your Docker volume\. This parameter maps to `Labels` in the [Create a volume](https://docs.docker.com/engine/api/v1.38/#operation/VolumeCreate) section of the [Docker Remote API](https://docs.docker.com/engine/api/v1.38/) and the `--label` option to [https://docs.docker.com/engine/reference/commandline/volume_create/](https://docs.docker.com/engine/reference/commandline/volume_create/)\.

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

## Examples<a name="docker-volume-examples"></a>

The following are examples that show the use of Docker volumes\.

**To provide nonpersistent storage for a container using a Docker volume**

In this example, a container uses an empty data volume that is disposed of after the task is finished\. One example use case is that you might have a container that needs to access some scratch file storage location during a task\. This task can be achieved using a Docker volume\.

1. In the task definition `volumes` section, define a data volume with `name` and `DockerVolumeConfiguration` values\. In this example, we specify the scope as `task` so the volume is deleted after the task stops and use the built\-in `local` driver\.

   ```
   "volumes": [
       {
           "name": "scratch",
           "dockerVolumeConfiguration" : {
               "scope": "task",
               "driver": "local",
               "labels": {
                   "scratch": "space"
               }
           }
       }
   ]
   ```

1. In the `containerDefinitions` section, define a container with `mountPoints` values that reference the name of the defined volume and the `containerPath` value to mount the volume at on the container\.

   ```
   "containerDefinitions": [
       {
           "name": "container-1",
           "mountPoints": [
               {
                 "sourceVolume": "scratch",
                 "containerPath": "/var/scratch"
               }
           ]
       }
   ]
   ```

**To provide persistent storage for a container using a Docker volume**

In this example, you want a shared volume for multiple containers to use and you want it to persist after any single task that use it stopped\. The built\-in `local` driver is being used\. This is so the volume is still tied to the lifecycle of the container instance\.

1. In the task definition `volumes` section, define a data volume with `name` and `DockerVolumeConfiguration` values\. In this example, specify a `shared` scope so the volume persists, set autoprovision to `true`\. This is so that the volume is created for use\. Then, also use the built\-in `local` driver\.

   ```
   "volumes": [
       {
           "name": "database",
           "dockerVolumeConfiguration" : {
               "scope": "shared",
               "autoprovision": true,
               "driver": "local",
               "labels": {
                   "database": "database_name"
               }
           }
       }
   ]
   ```

1. In the `containerDefinitions` section, define a container with `mountPoints` values that reference the name of the defined volume and the `containerPath` value to mount the volume at on the container\.

   ```
   "containerDefinitions": [
       {
           "name": "container-1",
           "mountPoints": [
           {
             "sourceVolume": "database",
             "containerPath": "/var/database"
           }
         ]
       },
       {
         "name": "container-2",
         "mountPoints": [
           {
             "sourceVolume": "database",
             "containerPath": "/var/database"
           }
         ]
       }
     ]
   ```

**To provide NFS persistent storage for a container using a Docker volume**

 In this example, a container uses an NFS data volume that is automatically mounted when the task starts and unmounted when the task stops\. This uses the Docker built\-in `local` driver\. One example use case is that you might have a local NFS storage and need to access it from an ECS Anywhere task\. This can be achieved using a Docker volume with NFS driver option\.

1. In the task definition `volumes` section, define a data volume with `name` and `DockerVolumeConfiguration` values\. In this example, specify a `task` scope so the volume is unmounted after the task stops\. Use the `local` driver and configure the `driverOpts` with the `type`, `device`, and `o` options accordingly\. Replace `NFS_SERVER` with the NFS server endpoint\.

   ```
   "volumes": [
          {
              "name": "NFS",
              "dockerVolumeConfiguration" : {
                  "scope": "task",
                  "driver": "local",
                  "driverOpts": {
                      "type": "nfs",
                      "device": "$NFS_SERVER:/mnt/nfs",
                      "o": "addr=$NFS_SERVER"
                  }
              }
          }
      ]
   ```

1. In the `containerDefinitions` section, define a container with `mountPoints` values that reference the name of the defined volume and the `containerPath` value to mount the volume on the container\.

   ```
   "containerDefinitions": [
          {
              "name": "container-1",
              "mountPoints": [
                  {
                    "sourceVolume": "NFS",
                    "containerPath": "/var/nfsmount"
                  }
              ]
          }
      ]
   ```