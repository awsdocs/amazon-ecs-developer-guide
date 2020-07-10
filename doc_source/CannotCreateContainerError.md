# `CannotCreateContainerError: API error (500): devmapper`<a name="CannotCreateContainerError"></a>

The following Docker error indicates that the thin pool storage on your container instance is full, and that the Docker daemon cannot create new containers:

```
CannotCreateContainerError: API error (500): devmapper: Thin Pool has 4350 free data blocks which is less than minimum required 4454 free data blocks. Create more free space in thin pool or use dm.min_free_space option to change behavior 
```

By default, Amazon ECS\-optimized Amazon Linux AMIs from version `2015.09.d` and later launch with an 8\-GiB volume for the operating system that is attached at `/dev/xvda` and mounted as the root of the file system\. There is an additional 22\-GiB volume that is attached at `/dev/xvdcz` that Docker uses for image and metadata storage\. If this storage space is filled up, the Docker daemon cannot create new containers\.

The easiest way to add storage to your container instances is to terminate the existing instances and launch new ones with larger data storage volumes\. However, if you are unable to do this, you can add storage to the volume group that Docker uses and extend its logical volume by following the procedures in [AMI storage configuration](ecs-ami-storage-config.md)\.

If your container instance storage is filling up too quickly, there are a few actions that you can take to reduce this effect:
+ \(Amazon ECS container agent 1\.8\.0 and later\) Reduce the amount of time that stopped or exited containers remain on your container instances\. The `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` agent configuration variable sets the time duration to wait from when a task is stopped until the Docker container is removed \(by default, this value is 3 hours\)\. This removes the Docker container data\. If this value is set too low, you may not be able to inspect your stopped containers or view the logs before they are removed\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
+ Remove non\-running containers and unused images from your container instances\. You can use the following example commands to manually remove stopped containers and unused images\. Deleted containers cannot be inspected later, and deleted images must be pulled again before starting new containers from them\.

  To remove non\-running containers, execute the following command on your container instance:

  ```
  docker rm $(docker ps -aq)
  ```

  To remove unused images, execute the following command on your container instance:

  ```
  docker rmi $(docker images -q)
  ```
+ Remove unused data blocks within containers\. You can use the following command to run fstrim on any running container and discard any data blocks that are unused by the container file system\.

  ```
  sudo sh -c "docker ps -q | xargs docker inspect --format='{{ .State.Pid }}' | xargs -IZ fstrim /proc/Z/root/"
  ```