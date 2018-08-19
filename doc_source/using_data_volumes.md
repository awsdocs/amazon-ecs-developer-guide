# Using Data Volumes in Tasks<a name="using_data_volumes"></a>

There are several use cases for using data volumes in Amazon ECS task definitions\. Some common examples are to provide persistent data volumes for use with a container, to define an empty, nonpersistent data volume and mount it on multiple containers, to share defined data volumes at different locations on different containers on the same container instance, and to provide a data volume to your task that is managed by a third party volume driver\. The lifecycle of the volume can be tied to either a specific task or to the lifecycle of a specific container instance\. 

The following are the types of data volumes that can be used:
+ Docker volumes — A Docker\-managed volume that is created under `/var/lib/docker/volumes` on the container instance\. Docker volume drivers \(also referred to as plugins\) are used to integrate the volumes with external storage systems, such as Amazon EBS\. The built\-in `local` volume driver or a third\-party volume driver can be used\. Docker volumes are only supported when using the EC2 launch type\. Windows containers only support the use of the `local` driver\. To use Docker volumes, specify a `dockerVolumeConfiguration` in your task definition\. For more information, see [Using volumes](https://docs.docker.com/storage/volumes/)\.
+ Bind mounts — A file or directory on the host machine is mounted into a container\. Bind mount host volumes are supported when using either the EC2 or Fargate launch types\. To use bind mount host volumes, specify a `host` and optional `sourcePath` value in your task definition\. For more information, see [Using bind mounts](https://docs.docker.com/storage/bind-mounts/)\.

**Note**  
Prior to the release of the Amazon ECS\-optimized AMI version 2017\.03\.a, only file systems that were available when the Docker daemon was started are available to Docker containers\. You can use the latest Amazon ECS\-optimized AMI to avoid this limitation, or you can upgrade the `docker` package to the latest version and restart Docker\.

**Topics**
+ [Docker Volumes](docker-volumes.md)
+ [Bind Mounts](bind-mounts.md)