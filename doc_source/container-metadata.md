# Amazon ECS Container Metadata File<a name="container-metadata"></a>

Beginning with version 1\.15\.0 of the Amazon ECS container agent, various container metadata is available within ECS task containers\. By enabling this feature, you can query the information about a task, container, and container instance from within the container or from the container instance by reading the metadata file for each container\. The metadata file is created on the host instance and mounted in the container as a Docker volume\.

The container metadata file location is set to the `ECS_CONTAINER_METADATA_FILE` environment variable inside the container\. You can read the file contents from inside the container with the following command:
+ **For Linux instances:**

  ```
  cat $ECS_CONTAINER_METADATA_FILE
  ```
+ **For Windows instances \(PowerShell\):**

  ```
  Get-Content -path $env:ECS_CONTAINER_METADATA_FILE
  ```

The container metadata file is cleaned up on the host instance when the container is cleaned up\. You can adjust when this happens with the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` container agent variable\. For more information, see [Automated Task and Image Cleanup](automated_image_cleanup.md)\.

**Topics**
+ [Enabling Container Metadata](#enable-metadata)
+ [Container Metadata File Locations](#metadata-file-locations)
+ [Container Metadata File Format](#metadata-file-format)

## Enabling Container Metadata<a name="enable-metadata"></a>

This feature is disabled by default\. You can enable container metadata at the container instance level by setting the `ECS_ENABLE_CONTAINER_METADATA` container agent variable to `true`\. You can set this variable in the `/etc/ecs/ecs.config` configuration file and restart the agent\. You can also set it as a Docker environment variable at run time when the agent container is started\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

**Note**  
The minimum Amazon ECS container agent version to support this feature is 1\.15\.0\.

If the `ECS_ENABLE_CONTAINER_METADATA` is set to `true` when the agent starts, metadata files are created for any future containers started by ECS\.

**Note**  
The Amazon ECS container agent cannot create metadata files for containers that were created before the `ECS_ENABLE_CONTAINER_METADATA` container agent variable was set to `true`\. To ensure that all containers receive metadata files, you should set this agent variable at container instance launch time\.

## Container Metadata File Locations<a name="metadata-file-locations"></a>

By default, the container metadata file is written to the following host and container paths\.
+ **For Linux instances:**
  + Host path: `/var/lib/ecs/data/metadata/task_id/container_name/ecs-container-metadata.json`
**Note**  
The Linux host path assumes that the default data directory mount path \(`/var/lib/ecs/data`\) is used when the agent is started\. If you are not using the Amazon ECS\-optimized AMI \(or the `ecs-init` package to start and maintain the container agent\), be sure to set the `ECS_HOST_DATA_DIR` agent configuration variable to the host path where the container agent's state file is located\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
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

`MetadataFileStatus`  
The status of the metadata file\. When the status is `READY`, the metadata file is current and complete\. If the file is not ready yet \(for example, the moment the task is started\), a truncated version of the file format is available\. To avoid a likely race condition where the container has started, but the metadata has not yet been written, you can parse the metadata file and wait for this parameter to be set to `READY` before depending on the metadata\. This is usually available in less than 1 second from when the container starts\.

**Example Amazon ECS container metadata file \(`READY`\)**  
The following example shows a container metadata file in the `READY` status\.  

```
{
	"Cluster": "default",
	"ContainerInstanceARN": "arn:aws:ecs:us-west-2:012345678910:container-instance/1f73d099-b914-411c-a9ff-81633b7741dd",
	"TaskARN": "arn:aws:ecs:us-west-2:012345678910:task/2b88376d-aba3-4950-9ddf-bcb0f388a40c",
	"ContainerID": "98e44444008169587b826b4cd76c6732e5899747e753af1e19a35db64f9e9c32",
	"ContainerName": "metadata",
	"DockerContainerName": "/ecs-metadata-7-metadata-f0edfbd6d09fdef20800",
	"ImageID": "sha256:c24f66af34b4d76558f7743109e2476b6325fcf6cc167c6e1e07cd121a22b341",
	"ImageName": "httpd:2.4",
	"PortMappings": [
		{
			"ContainerPort": 80,
			"HostPort": 80,
			"BindIp": "",
			"Protocol": "tcp"
		}
	],
	"Networks": [
		{
			"NetworkMode": "bridge",
			"IPv4Addresses": [
				"172.17.0.2"
			]
		}
	],
	"MetadataFileStatus": "READY"
}
```

**Example Incomplete Amazon ECS container metadata file \(not yet `READY`\)**  
The following example shows a container metadata file that has not yet reached the `READY` status\. The information in the file is limited to a few parameters that are known from the task definition\. The container metadata file should be ready within 1 second after the container starts\.  

```
{
    "Cluster": "default",
    "ContainerInstanceARN": "arn:aws:ecs:us-west-2:012345678910:container-instance/1f73d099-b914-411c-a9ff-81633b7741dd",
    "TaskARN": "arn:aws:ecs:us-west-2:012345678910:task/d90675f8-1a98-444b-805b-3d9cabb6fcd4",
    "ContainerName": "metadata"
}
```