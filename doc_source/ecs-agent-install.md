# Installing the Amazon ECS Container Agent<a name="ecs-agent-install"></a>

If your container instance was not launched from an AMI that includes the Amazon ECS container agent, you can install it using the following procedure\.

**Note**  
The Amazon ECS container agent is included in the Amazon ECS\-optimized AMI and does not require installation\.

**To install the Amazon ECS container agent on an Amazon Linux EC2 instance**

1. Launch an Amazon Linux instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Install the `ecs-init` package\. For more information about `ecs-init`, see the [source code on GitHub](https://github.com/aws/amazon-ecs-init)\.

   ```
   [ec2-user ~]$ sudo yum install -y ecs-init
   ```

1. Start the Docker daemon\.

   ```
   [ec2-user ~]$ sudo service docker start
   ```

   Output:

   ```
   Starting cgconfig service:                                 [  OK  ]
   Starting docker:	                                   [  OK  ]
   ```

1. Start the `ecs-init` upstart job\.

   ```
   [ec2-user ~]$ sudo start ecs
   ```

   Output:

   ```
   ecs start/running, process 2804
   ```

1. \(Optional\) You can verify that the agent is running and see some information about your new container instance with the agent introspection API\. For more information, see [Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)\.

   ```
   [ec2-user ~]$ curl http://localhost:51678/v1/metadata
   ```

   Output:

   ```
   {
     "Cluster": "default",
     "ContainerInstanceArn": "<container_instance_ARN>",
     "Version": "Amazon ECS Agent - v1.17.3 (159ae5c3)"
   }
   ```

**To install the Amazon ECS container agent on a non\-Amazon Linux EC2 instance**

1. Launch an EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Install Docker on your instance\. Amazon ECS requires a minimum Docker version of 1\.5\.0 \(version 17\.12\.1\-ce is recommended\), and the default Docker versions in many system package managers, such as yum or apt\-get do not meet this minimum requirement\. For information about installing the latest Docker version on your particular Linux distribution, see [https://docs\.docker\.com/engine/installation/](https://docs.docker.com/engine/installation/)\.
**Note**  
The Amazon Linux AMI always includes the recommended version of Docker for use with Amazon ECS\. You can install Docker on Amazon Linux with the sudo yum install docker \-y command\.

1. Check your Docker version to verify that your system meets the minimum version requirement\.

   ```
   ubuntu:~$ sudo docker version
   ```

   Output:

   ```
   Client version: 1.4.1
   Client API version: 1.16
   Go version (client): go1.3.3
   Git commit (client): 5bc2ff8
   OS/Arch (client): linux/amd64
   Server version: 1.4.1
   Server API version: 1.16
   Go version (server): go1.3.3
   Git commit (server): 5bc2ff8
   ```

   In this example, the Docker version is `1.4.1`, which is below the minimum version of 1\.5\.0\. This instance needs to upgrade its Docker version before proceeding\. For information about installing the latest Docker version on your particular Linux distribution, go to [https://docs\.docker\.com/engine/installation/](https://docs.docker.com/engine/installation/)\.

1. Run the following commands on your container instance to allow the port proxy to route traffic using loopback addresses\.

   ```
   ubuntu:~$ sudo sh -c "echo 'net.ipv4.conf.all.route_localnet = 1' >> /etc/sysctl.conf"
   ubuntu:~$ sudo sysctl -p /etc/sysctl.conf
   ```

1. Run the following commands on your container instance to enable IAM roles for tasks\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

   ```
   ubuntu:~$ iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
   ubuntu:~$ iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679
   ```

1. Write the new iptables configuration to your operating system\-specific location\.
   + For Debian/Ubuntu: 

     ```
     sudo sh -c 'iptables-save > /etc/iptables/rules.v4'
     ```
   + For CentOS/RHEL: 

     ```
     sudo sh -c 'iptables-save > /etc/sysconfig/iptables'
     ```

1. Create the `/etc/ecs` directory and create the Amazon ECS container agent configuration file\.

   ```
   ubuntu:~$ sudo mkdir -p /etc/ecs && sudo touch /etc/ecs/ecs.config
   ```

1. Edit the `/etc/ecs/ecs.config` file and add the following contents\. If you do not want your container instance to register with the default cluster, specify your cluster name as the value for `ECS_CLUSTER`\.

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
You can optionally store your agent environment variables in Amazon S3 \(which can be downloaded to your container instances at launch time using Amazon EC2 user data\)\. This is recommended for sensitive information such as authentication credentials for private repositories\. For more information, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3) and [Private Registry Authentication](private-auth.md)\.

1. Pull and run the latest Amazon ECS container agent on your container instance\.
**Note**  
You should use Docker restart policies or a process manager \(such as upstart or systemd\) to treat the container agent as a service or a daemon and ensure that it is restarted after exiting\. For more information, see [Automatically start containers](https://docs.docker.com/engine/admin/host_integration/) and [Restart policies](https://docs.docker.com/engine/reference/run/#restart-policies-restart) in the Docker documentation\. The Amazon ECS\-optimized AMI uses the `ecs-init` RPM for this purpose, and you can view the [source code for this RPM](https://github.com/aws/amazon-ecs-init) on GitHub\. For example systemd unit files for Ubuntu 16\.04 and CentOS 7, see [Example Container Instance User Data Configuration Scripts](example_user_data_scripts.md)\.

   The following example agent run command is broken into separate lines to show each option\. For more information about these and other agent runtime options, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
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