# Example container instance user data configuration scripts<a name="example_user_data_scripts"></a>

The following example user data scripts configure an Amazon ECS container instance at launch\.

## Ubuntu container instance with `systemd`<a name="ubuntu-systemd-userdata"></a>

This example user data script configures an Ubuntu 16\.04 instance to:
+ Install Docker\.
+ Create the required iptables rules for IAM roles for tasks\.
+ Create the required directories for the Amazon ECS container agent\.
+ Write the Amazon ECS container agent configuration file\.
+ Write the systemd unit file to monitor the agent\.
+ Enable and start the systemd unit\.

You can use this script for your own container instances, provided that they are launched from an Ubuntu 16\.04 AMI\. Be sure to replace the `ECS_CLUSTER=default` line in the configuration file to specify your own cluster name, if you are not using the `default` cluster\. For more information about launching container instances, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

```
#!/bin/bash
# Install Docker
apt-get update -y && apt-get install -y docker.io
echo iptables-persistent iptables-persistent/autosave_v4 boolean true | debconf-set-selections
apt-get update -y && apt-get install -y docker.io
echo iptables-persistent iptables-persistent/autosave_v6 boolean true | debconf-set-selections
apt-get -y install iptables-persistent

# Set iptables rules
echo 'net.ipv4.conf.all.route_localnet = 1' >> /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679

# Write iptables rules to persist after reboot
iptables-save > /etc/iptables/rules.v4

# Create directories for ECS agent
mkdir -p /var/log/ecs /var/lib/ecs/data /etc/ecs

# Write ECS config file
cat << EOF > /etc/ecs/ecs.config
ECS_DATADIR=/data
ECS_ENABLE_TASK_IAM_ROLE=true
ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true
ECS_LOGFILE=/log/ecs-agent.log
ECS_AVAILABLE_LOGGING_DRIVERS=["json-file","awslogs"]
ECS_LOGLEVEL=info
ECS_CLUSTER=default
EOF

# Write systemd unit file
cat << EOF > /etc/systemd/system/docker-container@ecs-agent.service
[Unit]
Description=Docker Container %I
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStartPre=-/usr/bin/docker rm -f %i 
ExecStart=/usr/bin/docker run --name %i \
--restart=on-failure:10 \
--volume=/var/run:/var/run \
--volume=/var/log/ecs/:/log \
--volume=/var/lib/ecs/data:/data \
--volume=/etc/ecs:/etc/ecs \
--net=host \
--env-file=/etc/ecs/ecs.config \
amazon/amazon-ecs-agent:latest
ExecStop=/usr/bin/docker stop %i

[Install]
WantedBy=default.target
EOF

systemctl enable docker-container@ecs-agent.service
systemctl start docker-container@ecs-agent.service
```

## CentOS container instance with `systemd` and `SELinux`<a name="centos-systemd-userdata"></a>

This example user data script configures a CentOS 7 instance with `SELinux` enabled to:
+ Install Docker\.
+ Create the required iptables rules for IAM roles for tasks\.
+ Create the required directories for the Amazon ECS container agent\.
+ Write the Amazon ECS container agent configuration file\.
+ Write the systemd unit file to monitor the agent\.
+ Enable and start the systemd unit\.

**Note**  
The docker run command in the systemd unit file below contains the required modifications for `SELinux`, including the `--privileged` flag, and the `:Z` suffixes to the volume mounts\.

You can use this script for your own container instances \(provided that they are launched from an CentOS 7 AMI\), Be sure to replace the `ECS_CLUSTER=default` line in the configuration file to specify your own cluster name \(if you are not using the `default` cluster\)\. For more information about launching container instances, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

```
#!/bin/bash
# Install Docker
yum install -y docker

# Set iptables rules
echo 'net.ipv4.conf.all.route_localnet = 1' >> /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679

# Write iptables rules to persist after reboot
iptables-save > /etc/sysconfig/iptables

# Create directories for ECS agent
mkdir -p /var/log/ecs /var/lib/ecs/data /etc/ecs

# Write ECS config file
cat << EOF > /etc/ecs/ecs.config
ECS_DATADIR=/data
ECS_ENABLE_TASK_IAM_ROLE=true
ECS_ENABLE_TASK_IAM_ROLE_NETWORK_HOST=true
ECS_LOGFILE=/log/ecs-agent.log
ECS_AVAILABLE_LOGGING_DRIVERS=["json-file","awslogs"]
ECS_LOGLEVEL=info
ECS_CLUSTER=default
EOF

# Write systemd unit file
cat << EOF > /etc/systemd/system/docker-container@ecs-agent.service
[Unit]
Description=Docker Container %I
Requires=docker.service
After=cloud-final.service

[Service]
Restart=always
ExecStartPre=-/usr/bin/docker rm -f %i 
ExecStart=/usr/bin/docker run --name %i \
--privileged \
--restart=on-failure:10 \
--volume=/var/run:/var/run \
--volume=/var/log/ecs/:/log:Z \
--volume=/var/lib/ecs/data:/data:Z \
--volume=/etc/ecs:/etc/ecs \
--net=host \
--env-file=/etc/ecs/ecs.config \
amazon/amazon-ecs-agent:latest
ExecStop=/usr/bin/docker stop %i

[Install]
WantedBy=default.target
EOF

systemctl enable docker-container@ecs-agent.service
systemctl start docker-container@ecs-agent.service
```