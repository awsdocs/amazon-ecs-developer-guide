# HTTP Proxy Configuration<a name="http_proxy_config"></a>

You can configure your Amazon ECS container instances to use an HTTP proxy for both the Amazon ECS container agent and the Docker daemon\. This is useful if your container instances do not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\. The process differs for Linux and Windows instances, so be sure to read the appropriate section below for your application\.


+ [Linux Container Instance Configuration](#linux-proxy)
+ [Windows Container Instance Configuration](#windows-proxy)

## Linux Container Instance Configuration<a name="linux-proxy"></a>

To configure your Amazon ECS Linux container instance to use an HTTP proxy, set the following variables in the `/etc/ecs/ecs.config`, `/etc/init/ecs.override`, and `/etc/sysconfig/docker` files at launch time \(with Amazon EC2 user data\)\. You could also manually edit the configuration file and restart the agent afterwards\.

`/etc/ecs/ecs.config`    
`HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for the ECS agent to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock`  
Set this value to `169.254.169.254,169.254.170.2,/var/run/docker.sock` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

`/etc/init/ecs.override`    
`env HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for `ecs-init` to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`env NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock`  
Set this value to `169.254.169.254,169.254.170.2,/var/run/docker.sock` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

`/etc/sysconfig/docker`    
`export HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for the Docker daemon to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`export NO_PROXY=169.254.169.254`  
Set this value to `169.254.169.254` to filter EC2 instance metadata from the proxy\. 

Setting these environment variables in the above files only affects the Amazon ECS container agent, `ecs-init`, and the Docker daemon\. They do not configure any other services \(such as yum\) to use the proxy\.

**Example Linux HTTP proxy user data script**  
The example user data `cloud-boothook` script below configures the Amazon ECS container agent, `ecs-init`, the Docker daemon, and yum to use an HTTP proxy that you specify\. You can also specify a cluster into which the container instance registers itself\.  
To use this script when you launch a container instance, follow the steps in [Launching an Amazon ECS Container Instance](launch_container_instance.md), and in [[ERROR] BAD/MISSING LINK TEXT](launch_container_instance.md#instance-launch-user-data-step)\. Then, copy and paste the `cloud-boothook` script below into the **User data** field \(be sure to substitute the red example values with your own proxy and cluster information\)\.  

```
#cloud-boothook
# Configure Yum, the Docker daemon, and the ECS agent to use an HTTP proxy

# Specify proxy host, port number, and ECS cluster name to use
PROXY_HOST=10.0.0.131
PROXY_PORT=3128
CLUSTER_NAME=proxy-test

# Set Yum HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_yum_http_proxy ]; then
	echo "proxy=http://$PROXY_HOST:$PROXY_PORT" >> /etc/yum.conf
	echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_yum_http_proxy
fi

# Set Docker HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_docker_http_proxy ]; then
	echo "export HTTP_PROXY=http://$PROXY_HOST:$PROXY_PORT/" >> /etc/sysconfig/docker
	echo "export NO_PROXY=169.254.169.254" >> /etc/sysconfig/docker
	echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_docker_http_proxy
fi

# Set ECS agent HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_ecs-agent_http_proxy ]; then
	echo "ECS_CLUSTER=$CLUSTER_NAME" >> /etc/ecs/ecs.config
	echo "HTTP_PROXY=$PROXY_HOST:$PROXY_PORT" >> /etc/ecs/ecs.config
	echo "NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock" >> /etc/ecs/ecs.config
	echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_ecs-agent_http_proxy
fi

# Set ecs-init HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_ecs-init_http_proxy ]; then
	echo "env HTTP_PROXY=$PROXY_HOST:$PROXY_PORT" >> /etc/init/ecs.override
	echo "env NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock" >> /etc/init/ecs.override
	echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_ecs-init_http_proxy
fi
```

## Windows Container Instance Configuration<a name="windows-proxy"></a>

To configure your Amazon ECS Windows container instance to use an HTTP proxy, set the following variables at launch time \(with Amazon EC2 user data\)\.

`[Environment]::SetEnvironmentVariable("HTTP_PROXY", "http://proxy.mydomain:port", "Machine")`  
Set `HTTP_PROXY` to the hostname \(or IP address\) and port number of an HTTP proxy to use for the ECS agent to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.

`[Environment]::SetEnvironmentVariable("NO_PROXY", "169.254.169.254,169.254.170.2,\\.\pipe\docker_engine", "Machine")`  
Set `NO_PROXY` to `169.254.169.254,169.254.170.2,\\.\pipe\docker_engine` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

**Example Windows HTTP proxy user data script**  
The example user data PowerShell script below configures the Amazon ECS container agent and the Docker daemon to use an HTTP proxy that you specify\. You can also specify a cluster into which the container instance registers itself\.  
To use this script when you launch a container instance, follow the steps in [Step 2: Launching a Windows Container Instance into your Cluster](ECS_Windows_getting_started.md#launch_windows_container_instance)\. When you reach [[ERROR] BAD/MISSING LINK TEXT](ECS_Windows_getting_started.md#windows-instance-launch-user-data-step), copy and paste the PowerShell script below into the **User data** field \(be sure to substitute the red example values with your own proxy and cluster information\)\.  
The `-EnableTaskIAMRole` option is required to enable IAM roles for tasks\. For more information, see [Windows IAM Roles for Tasks](windows_task_IAM_roles.md)\.

```
<powershell>
Import-Module ECSTools

$proxy = "http://proxy.mydomain:port"
[Environment]::SetEnvironmentVariable("HTTP_PROXY", $proxy, "Machine")
[Environment]::SetEnvironmentVariable("NO_PROXY", "169.254.169.254,169.254.170.2,\\.\pipe\docker_engine", "Machine")

Restart-Service Docker
Initialize-ECSAgent -Cluster MyCluster -EnableTaskIAMRole
</powershell>
```