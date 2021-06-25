# Bootstrapping container instances with Amazon EC2 user data<a name="bootstrap_container_instance"></a>

When you launch an Amazon ECS container instance, you have the option of passing user data to the instance\. The data can be used to perform common automated configuration tasks and even run scripts when the instance boots\. For Amazon ECS, the most common use cases for user data are to pass configuration information to the Docker daemon and the Amazon ECS container agent\. 

You can pass multiple types of user data to Amazon EC2, including cloud boothooks, shell scripts, and `cloud-init` directives\. For more information about these and other format types, see the [Cloud\-Init documentation](https://cloudinit.readthedocs.io/en/latest/topics/format.html)\. 

You can pass this user data when using the Amazon EC2 launch wizard\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

**Topics**
+ [Amazon ECS container agent](#bootstrap_container_agent)
+ [Docker daemon](#bootstrap_docker_daemon)
+ [Example container instance user data configuration scripts](example_user_data_scripts.md)

## Amazon ECS container agent<a name="bootstrap_container_agent"></a>

The Linux variants of the Amazon ECS\-optimized AMI look for agent configuration data in the `/etc/ecs/ecs.config` file when the container agent starts\. You can specify this configuration data at launch with Amazon EC2 user data\. For more information about available Amazon ECS container agent configuration variables, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

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

## Docker daemon<a name="bootstrap_docker_daemon"></a>

You can specify Docker daemon configuration information with Amazon EC2 user data\. For more information about configuration options, see [the Docker daemon documentation](https://docs.docker.com/engine/reference/commandline/dockerd/)\.

In the example below, the custom options are added to the Docker daemon configuration file, `/etc/docker/daemon.json` which is then specified in the user data when the instance is launched\.

```
#!/bin/bash
cat <<EOF >/etc/docker/daemon.json
{"debug": true}
EOF
systemctl restart docker --no-block
```