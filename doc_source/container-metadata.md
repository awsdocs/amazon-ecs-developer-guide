# Amazon ECS Container Metadata File<a name="container-metadata"></a>

Beginning with version 1\.15\.0 of the Amazon ECS container agent, various container metadata is available within your containers or the host container instance\. By enabling this feature, you can query the information about a task, container, and container instance from within the container or the host container instance\. The metadata file is created on the host instance and mounted in the container as a Docker volume\.

The container metadata file is cleaned up on the host instance when the container is cleaned up\. You can adjust when this happens with the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` container agent variable\. For more information, see [Automated Task and Image Cleanup](automated_image_cleanup.md)\.

**Topics**
+ [Enabling Container Metadata](#enable-metadata)
+ [Container Metadata File Locations](#metadata-file-locations)
+ [Container Metadata File Format](#metadata-file-format)

## Enabling Container Metadata<a name="enable-metadata"></a>

This feature is disabled by default\. You can enable container metadata at the container instance level by setting the `ECS_ENABLE_CONTAINER_METADATA` container agent variable to `true`\. You can set this variable in the `/etc/ecs/ecs.config` configuration file and restart the agent\. You can also set it as a Docker environment variable at runtime when the agent container is started\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

If the `ECS_ENABLE_CONTAINER_METADATA` is set to `true` when the agent starts, metadata files are created for any containers created from that point forward\. The Amazon ECS container agent cannot create metadata files for containers that were created before the `ECS_ENABLE_CONTAINER_METADATA` container agent variable was set to `true`\. To ensure that all containers receive metadata files, you should set this agent variable at container instance launch\. The following is an example user data script that will set this variable as well as register your container instance with your cluster\.

```
#!/bin/bash
cat <<'EOF' >> /etc/ecs/ecs.config
ECS_CLUSTER=your_cluster_name
ECS_ENABLE_CONTAINER_METADATA=true
EOF
```

## Container Metadata File Locations<a name="metadata-file-locations"></a>

By default, the container metadata file is written to the following host and container paths\.
+ **For Linux instances:**
  + Host path: `/var/lib/ecs/data/metadata/cluster_name/task_id/container_name/ecs-container-metadata.json`
**Note**  
The Linux host path assumes that the default data directory mount path \(`/var/lib/ecs/data`\) is used when the agent is started\. If you are not using an Amazon ECS\-optimized AMI \(or the `ecs-init` package to start and maintain the container agent\), be sure to set the `ECS_HOST_DATA_DIR` agent configuration variable to the host path where the container agent's state file is located\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
  + Container path: `/opt/ecs/metadata/random_ID/ecs-container-metadata.json`
+ **For Windows instances:**
  + Host path: `C:\ProgramData\Amazon\ECS\data\metadata\task_id\container_name\ecs-container-metadata.json`
  + Container path: `C:\ProgramData\Amazon\ECS\metadata\random_ID\ecs-container-metadata.json`

However, for easy access, the container metadata file location is set to the `ECS_CONTAINER_METADATA_FILE` environment variable inside the container\. You can read the file contents from inside the container with the following command:
+ **For Linux instances:**

  ```
  cat $ECS_CONTAINER_METADATA_FILE
  ```
+ **For Windows instances \(PowerShell\):**

  ```
  Get-Content -path $env:ECS_CONTAINER_METADATA_FILE
  ```

## Container Metadata File Format<a name="metadata-file-format"></a>

The following information is stored in the container metadata JSON file\.

`Cluster`  
The name of the cluster that the container's task is running on\.

`ContainerInstanceARN`  
The full Amazon Resource Name \(ARN\) of the host container instance\.

`TaskARN`  
The full Amazon Resource Name \(ARN\) of the task that the container belongs to\.

`TaskDefinitionFamily`  
The name of the task definition family the container is using\.

`TaskDefinitionRevision`  
The task definition revision the container is using\.

`ContainerID`  
The Docker container ID \(and not the Amazon ECS container ID\) for the container\.

`ContainerName`  
The container name from the Amazon ECS task definition for the container\.

`DockerContainerName`  
The container name that the Docker daemon uses for the container \(for example, the name that shows up in docker ps command output\)\.

`ImageID`  
The SHA digest for the Docker image used to start the container\.

`ImageName`  
The image name and tag for the Docker image used to start the container\.

`PortMappings`  
Any port mappings associated with the container\.    
`ContainerPort`  
The port on the container that is exposed\.  
`HostPort`  
The port on the host container instance that is exposed\.  
`BindIp`  
The bind IP address that is assigned to the container by Docker\. This IP address is only applied with the `bridge` network mode, and it is only accessible from the container instance\.  
`Protocol`  
The network protocol used for the port mapping\.

`Networks`  
The network mode and IP address for the container\.    
`NetworkMode`  
The network mode for the task to which the container belongs\.  
`IPv4Addresses`  
The IP addresses associated with the container\.  
If your task is using the `awsvpc` network mode, the IP address of the container will not be returned\. In this case, you can retrieve the IP address by reading the /etc/hosts file with the following command:  

```
cat /etc/hosts | tail -1 | awk {'print $1'}
```

`MetadataFileStatus`  
The status of the metadata file\. When the status is `READY`, the metadata file is current and complete\. If the file is not ready yet \(for example, the moment the task is started\), a truncated version of the file format is available\. To avoid a likely race condition where the container has started, but the metadata has not yet been written, you can parse the metadata file and wait for this parameter to be set to `READY` before depending on the metadata\. This is usually available in less than 1 second from when the container starts\.

`AvailabilityZone`  
The Availability Zone the host container instance resides in\.

`HostPrivateIPv4Address`  
The private IP address for the task the container belongs to\.

`HostPublicIPv4Address`  
The public IP address for the task the container belongs to\.

**Example Amazon ECS container metadata file \(`READY`\)**  
The following example shows a container metadata file in the `READY` status\.  

```
{
    "Cluster": "default",
    "ContainerInstanceARN": "arn:aws:ecs:us-west-2:012345678910:container-instance/default/1f73d099-b914-411c-a9ff-81633b7741dd",
    "TaskARN": "arn:aws:ecs:us-west-2:012345678910:task/default/2b88376d-aba3-4950-9ddf-bcb0f388a40c",
    "TaskDefinitionFamily": "console-sample-app-static",
    "TaskDefinitionRevision": "1",
    "ContainerID": "aec2557997f4eed9b280c2efd7afccdcedfda4ac399f7480cae870cfc7e163fd",
    "ContainerName": "simple-app",
    "DockerContainerName": "/ecs-console-sample-app-static-1-simple-app-e4e8e495e8baa5de1a00",
    "ImageID": "sha256:2ae34abc2ed0a22e280d17e13f9c01aaf725688b09b7a1525d1a2750e2c0d1de",
    "ImageName": "httpd:2.4",
    "PortMappings": [
        {
            "ContainerPort": 80,
            "HostPort": 80,
            "BindIp": "0.0.0.0",
            "Protocol": "tcp"
        }
    ],
    "Networks": [
        {
            "NetworkMode": "bridge",
            "IPv4Addresses": [
                "192.0.2.0"
            ]
        }
    ],
    "MetadataFileStatus": "READY",
    "AvailabilityZone": "us-east-1b",
    "HostPrivateIPv4Address": "192.0.2.0",
    "HostPublicIPv4Address": "203.0.113.0"
}
```

**Example Incomplete Amazon ECS container metadata file \(not yet `READY`\)**  
The following example shows a container metadata file that has not yet reached the `READY` status\. The information in the file is limited to a few parameters that are known from the task definition\. The container metadata file should be ready within 1 second after the container starts\.  

```
{
    "Cluster": "default",
    "ContainerInstanceARN": "arn:aws:ecs:us-west-2:012345678910:container-instance/default/1f73d099-b914-411c-a9ff-81633b7741dd",
    "TaskARN": "arn:aws:ecs:us-west-2:012345678910:task/default/d90675f8-1a98-444b-805b-3d9cabb6fcd4",
    "ContainerName": "metadata"
}
```