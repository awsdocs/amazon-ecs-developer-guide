# Manually Updating the Amazon ECS Container Agent \(for Non\-Amazon ECS\-Optimized AMIs\)<a name="manually_update_agent"></a>

**To manually update the Amazon ECS container agent \(for non\-Amazon ECS\-optimized AMIs\)**
**Note**  
Agent updates do not apply to Windows container instances\. We recommend that you launch new container instances to update the agent version in your Windows clusters\.

1. Log in to your container instance via SSH\.

1. Check to see if your agent uses the `ECS_DATADIR` environment variable to save its state\.

   ```
   ubuntu:~$ docker inspect ecs-agent | grep ECS_DATADIR
   ```

   Output:

   ```
   "ECS_DATADIR=/data",
   ```
**Important**  
If the previous command does not return the `ECS_DATADIR` environment variable, you must stop any tasks running on this container instance before updating your agent\. Newer agents with the `ECS_DATADIR` environment variable save their state and you can update them while tasks are running without issues\.

1. Stop the Amazon ECS container agent\.

   ```
   ubuntu:~$ docker stop ecs-agent
   ```

1. Delete the agent container\.

   ```
   ubuntu:~$ docker rm ecs-agent
   ```

1. Ensure that the `/etc/ecs` directory and the Amazon ECS container agent configuration file exist at `/etc/ecs/ecs.config`\.

   ```
   ubuntu:~$ sudo mkdir -p /etc/ecs && sudo touch /etc/ecs/ecs.config
   ```

1. Edit the `/etc/ecs/ecs.config` file and ensure that it contains at least the following variable declarations\. If you do not want your container instance to register with the default cluster, specify your cluster name as the value for `ECS_CLUSTER`\.

   ```
   ECS_DATADIR=/data
   ECS_ENABLE_TASK_IAM_ROLE=true
   ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true
   ECS_LOGFILE=/log/ecs-agent.log
   ECS_AVAILABLE_LOGGING_DRIVERS=["json-file","awslogs"]
   ECS_LOGLEVEL=info
   ECS_CLUSTER=default
   ```

   For more information about these and other agent runtime options, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
**Note**  
You can optionally store your agent environment variables in Amazon S3 \(which can be downloaded to your container instances at launch time using Amazon EC2 user data\)\. This is recommended for sensitive information such as authentication credentials for private repositories\. For more information, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3) and [Private registry authentication for tasks](private-auth.md)\.

1. Pull the latest Amazon ECS container agent image from Docker Hub\.

   ```
   ubuntu:~$ docker pull amazon/amazon-ecs-agent:latest
   ```

   Output:

   ```
   Pulling repository amazon/amazon-ecs-agent
   a5a56a5e13dc: Download complete
   511136ea3c5a: Download complete
   9950b5d678a1: Download complete
   c48ddcf21b63: Download complete
   Status: Image is up to date for amazon/amazon-ecs-agent:latest
   ```

1. Run the latest Amazon ECS container agent on your container instance\.
**Note**  
Use Docker restart policies or a process manager \(such as upstart or systemd\) to treat the container agent as a service or a daemon and ensure that it is restarted after exiting\. For more information, see [Automatically start containers](https://docs.docker.com/engine/admin/host_integration/) and [Restart policies](https://docs.docker.com/engine/reference/run/#restart-policies-restart) in the Docker documentation\. The Amazon ECS\-optimized AMI uses the `ecs-init` RPM for this purpose, and you can view the [source code for this RPM](https://github.com/aws/amazon-ecs-init) on GitHub\. For example systemd unit files for Ubuntu 16\.04 and CentOS 7, see [Example Container Instance User Data Configuration Scripts](example_user_data_scripts.md)\.

   The following example of the agent run command is broken into separate lines to show each option\. For more information about these and other agent runtime options, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
**Important**  
Operating systems with SELinux enabled require the `--privileged` option in your docker run command\. In addition, for SELinux\-enabled container instances, we recommend that you add the `:Z` option to the `/log` and `/data` volume mounts\. However, the host mounts for these volumes must exist before you run the command or you receive a `no such file or directory` error\. Take the following action if you experience difficulty running the Amazon ECS agent on an SELinux\-enabled container instance:  
Create the host volume mount points on your container instance\.  

     ```
     ubuntu:~$ sudo mkdir -p /var/log/ecs /var/lib/ecs/data
     ```
Add the `--privileged` option to the docker run command below\.
Append the `:Z` option to the `/log` and `/data` container volume mounts \(for example, `--volume=/var/log/ecs/:/log:Z`\) to the docker run command below\.

   ```
   ubuntu:~$ sudo docker run --name ecs-agent \
   --detach=true \
   --restart=on-failure:10 \
   --volume=/var/run:/var/run \
   --volume=/var/log/ecs/:/log \
   --volume=/var/lib/ecs/data:/data \
   --volume=/etc/ecs:/etc/ecs \
   --net=host \
   --env-file=/etc/ecs/ecs.config \
   amazon/amazon-ecs-agent:latest
   ```
**Note**  
If you receive an `Error response from daemon: Cannot start container` message, you can delete the failed container with the sudo docker rm ecs\-agent command and try running the agent again\. 