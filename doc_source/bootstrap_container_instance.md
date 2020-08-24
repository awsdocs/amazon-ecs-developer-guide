# Bootstrapping Container Instances with Amazon EC2 User Data<a name="bootstrap_container_instance"></a>

When you launch an Amazon ECS container instance, you have the option of passing user data to the instance\. The data can be used to perform common automated configuration tasks and even run scripts when the instance boots\. For Amazon ECS, the most common use cases for user data are to pass configuration information to the Docker daemon and the Amazon ECS container agent\. 

You can pass multiple types of user data to Amazon EC2, including cloud boothooks, shell scripts, and `cloud-init` directives\. For more information about these and other format types, see the [Cloud\-Init documentation](https://cloudinit.readthedocs.io/en/latest/topics/format.html)\. 

You can pass this user data when using the Amazon EC2 launch wizard\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

**Topics**
+ [Amazon ECS Container Agent](#bootstrap_container_agent)
+ [Docker Daemon](#bootstrap_docker_daemon)
+ [cloud\-init\-per Utility](#cloud-init-per)
+ [Specifying Multiple User Data Blocks Using a MIME Multi Part Archive](#multi-part_user_data)
+ [Example Container Instance User Data Configuration Scripts](example_user_data_scripts.md)

## Amazon ECS Container Agent<a name="bootstrap_container_agent"></a>

The Linux variants of the Amazon ECS\-optimized AMI look for agent configuration data in the `/etc/ecs/ecs.config` file when the container agent starts\. You can specify this configuration data at launch with Amazon EC2 user data\. For more information about available Amazon ECS container agent configuration variables, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

To set only a single agent configuration variable, such as the cluster name, use echo to copy the variable to the configuration file:

```
#!/bin/bash
echo "ECS_CLUSTER=MyCluster" >> /etc/ecs/ecs.config
```

If you have multiple variables to write to `/etc/ecs/ecs.config`, use the following `heredoc` format\. This format writes everything between the lines beginning with cat and `EOF` to the configuration file\.

```
#!/bin/bash
cat <<'EOF' >> /etc/ecs/ecs.config
ECS_CLUSTER=MyCluster
ECS_ENGINE_AUTH_TYPE=docker
ECS_ENGINE_AUTH_DATA={"https://index.docker.io/v1/":{"username":"my_name","password":"my_password","email":"email@example.com"}}
ECS_LOGLEVEL=debug
EOF
```

## Docker Daemon<a name="bootstrap_docker_daemon"></a>

You can specify Docker daemon configuration information with Amazon EC2 user data, but this configuration data must be written before the Docker daemon starts\. The `cloud-boothook` user data format executes earlier in the boot process than a user data shell script\. For more information about configuration options, see [the Docker daemon documentation](https://docs.docker.com/engine/reference/commandline/dockerd/)\.

By default, `cloud-boothook` user data is run at every instance boot, so you must create a mechanism to prevent the boothook from running multiple times\. The cloud\-init\-per utility is provided to control boothook frequency in this manner\. For more information, see [cloud\-init\-per Utility](#cloud-init-per)\.

In the example below, the `--foo bar` option is appended to any existing options in the Docker daemon configuration file, `/etc/sysconfig/docker`\.

```
#cloud-boothook
cloud-init-per once docker_options echo 'OPTIONS="${OPTIONS} --foo bar"' >> /etc/sysconfig/docker
```

To write multiple lines to a file, use the following `heredoc` format to accomplish the same goal:

```
#cloud-boothook
cloud-init-per instance docker_options cat <<'EOF' >> /etc/sysconfig/docker
OPTIONS="${OPTIONS} --foo bar"
HTTP_PROXY=http://proxy.example.com:80/
EOF
```

## cloud\-init\-per Utility<a name="cloud-init-per"></a>

**Note**  
At this time, only ECS Containers based on Amazon Linux AMIs \(**not** Amazon Linux 2 AMIs\) support cloud\-init\-per\.

The cloud\-init\-per utility is provided by the `cloud-init` package to help you create boothook commands for instances that run at a specified frequency\. 

The cloud\-init\-per utility syntax is as follows:

```
cloud-init-per frequency name cmd [ arg1 [ arg2 [ ... ] ]
```

`frequency`  
How often the boothook should run\.   
+ Specify `once` to never run again, even with a new instance ID\.
+ Specify `instance` to run on the first boot for each new instance launch\. For example, if you create an AMI from the instance after the boothook has run, it still runs again on subsequent instances launched from that AMI\.
+ Specify `always` to run at every boot\.

`name`  
The name to include in the semaphore file path that is written when the boothook runs\. The semaphore file is written to `/var/lib/cloud/instances/instance_id/sem/bootper.name.instance`\.

`cmd`  
The command and arguments that the boothook should execute\.

In the example below, the command `echo 'OPTIONS="${OPTIONS} --foo bar"' >> /etc/sysconfig/docker` is executed only once\. A semaphore file is written that contains its name\.

```
#cloud-boothook
cloud-init-per once docker_options echo 'OPTIONS="${OPTIONS} --foo bar"' >> /etc/sysconfig/docker
```

The semaphore file records the exit code of the command and a UNIX timestamp for when it was executed\.

```
[ec2-user ~]$ cat /var/lib/cloud/instances/i-0c7f87d7611b2165e/sem/bootper.docker_options.instance
```

Output:

```
0	1488410363
```

## Specifying Multiple User Data Blocks Using a MIME Multi Part Archive<a name="multi-part_user_data"></a>

You can combine multiple user data blocks together into a single user data block called a MIME multi\-part file\. For example, you might want to combine a cloud boothook that configures the Docker daemon with a user data shell script that writes configuration information for the Amazon ECS container agent\. 

A MIME multi\-part file consists of the following components:
+ The content type and part boundary declaration: `Content-Type: multipart/mixed; boundary="==BOUNDARY=="`
+ The MIME version declaration: `MIME-Version: 1.0`
+ One or more user data blocks, which contain the following components:
  + The opening boundary, which signals the beginning of a user data block: `--==BOUNDARY==`
  + The content type declaration for the block: `Content-Type: text/cloud-boothook; charset="us-ascii"`\. For more information about content types, see the [Cloud\-Init documentation](https://cloudinit.readthedocs.io/en/latest/topics/format.html)\. 
  + The content of the user data, for example, a list of shell commands or `cloud-init` directives
+ The closing boundary, which signals the end of the MIME multi\-part file: `--==BOUNDARY==--`

**Example MIME multi\-part file**  
This example MIME multi\-part file configures the Docker base device size to 20 GiB and configures the Amazon ECS container agent to register the instance into the cluster named `my-ecs-cluster`\.  

```
Content-Type: multipart/mixed; boundary="==BOUNDARY=="
MIME-Version: 1.0

--==BOUNDARY==
Content-Type: text/cloud-boothook; charset="us-ascii"

# Set Docker daemon options
cloud-init-per once docker_options echo 'OPTIONS="${OPTIONS} --foo bar"' >> /etc/sysconfig/docker

--==BOUNDARY==
Content-Type: text/x-shellscript; charset="us-ascii"

#!/bin/bash
# Set any ECS agent configuration options
echo "ECS_CLUSTER=my-ecs-cluster" >> /etc/ecs/ecs.config

--==BOUNDARY==--
```