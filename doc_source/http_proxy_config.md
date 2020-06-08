# HTTP Proxy Configuration<a name="http_proxy_config"></a>

You can configure your Amazon ECS container instances to use an HTTP proxy for both the Amazon ECS container agent and the Docker daemon\. This is useful if your container instances do not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\. The process differs for Linux and Windows instances, so be sure to read the appropriate section below for your application\.

**Topics**
+ [Amazon Linux Container Instance Configuration](#linux-proxy)
+ [Windows Container Instance Configuration](#windows-proxy)

## Amazon Linux Container Instance Configuration<a name="linux-proxy"></a>

To configure your Amazon ECS Linux container instance to use an HTTP proxy, set the following variables in the relevant files at launch time \(with Amazon EC2 user data\)\. You could also manually edit the configuration file and restart the agent afterwards\.

`/etc/ecs/ecs.config` \(Amazon Linux 2 and Amazon Linux AMI\)    
`HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for the ECS agent to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock`  
Set this value to `169.254.169.254,169.254.170.2,/var/run/docker.sock` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

`/etc/systemd/system/ecs.service.d/http-proxy.conf` \(Amazon Linux 2 only\)    
`Environment="HTTP_PROXY=10.0.0.131:3128/"`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for `ecs-init` to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`Environment="NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock"`  
Set this value to `169.254.169.254,169.254.170.2,/var/run/docker.sock` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

`/etc/init/ecs.override` \(Amazon Linux AMI only\)    
`env HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for `ecs-init` to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`env NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock`  
Set this value to `169.254.169.254,169.254.170.2,/var/run/docker.sock` to filter EC2 instance metadata, IAM roles for tasks, and Docker daemon traffic from the proxy\. 

`/etc/systemd/system/docker.service.d/http-proxy.conf` \(Amazon Linux 2 only\)    
`Environment="HTTP_PROXY=http://10.0.0.131:3128"`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for the Docker daemon to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`Environment="NO_PROXY=169.254.169.254"`  
Set this value to `169.254.169.254` to filter EC2 instance metadata from the proxy\. 

`/etc/sysconfig/docker` \(Amazon Linux AMI and Amazon Linux 2 only\)    
`export HTTP_PROXY=10.0.0.131:3128`  
Set this value to the hostname \(or IP address\) and port number of an HTTP proxy to use for the Docker daemon to connect to the internet\. For example, your container instances may not have external network access through an Amazon VPC internet gateway, NAT gateway, or instance\.  
`export NO_PROXY=169.254.169.254,169.254.170.2`  
Set this value to `169.254.169.254` to filter EC2 instance metadata from the proxy\. 

Setting these environment variables in the above files only affects the Amazon ECS container agent, `ecs-init`, and the Docker daemon\. They do not configure any other services \(such as yum\) to use the proxy\.

**Example Amazon Linux HTTP proxy user data script**  
The example user data `cloud-boothook` script below configures the Amazon ECS container agent, `ecs-init`, the Docker daemon, and yum to use an HTTP proxy that you specify\. You can also specify a cluster into which the container instance registers itself\.  
To use this script when you launch a container instance, follow the steps in [Launching an Amazon ECS Container Instance](launch_container_instance.md), and in [Step 7](launch_container_instance.md#instance-launch-user-data-step)\. Then, copy and paste the `cloud-boothook` script below into the **User data** field \(be sure to substitute the red example values with your own proxy and cluster information\)\.  
The user data script below only supports Amazon Linux 2 and Amazon Linux AMI variants of the Amazon ECS\-optimized AMI\.

```
#cloud-boothook
# Configure Yum, the Docker daemon, and the ECS agent to use an HTTP proxy

# Specify proxy host, port number, and ECS cluster name to use
PROXY_HOST=10.0.0.131
PROXY_PORT=3128
CLUSTER_NAME=proxy-test

if grep -q 'Amazon Linux release 2' /etc/system-release ; then
    OS=AL2
    echo "Setting OS to Amazon Linux 2"
elif grep -q 'Amazon Linux AMI' /etc/system-release ; then
    OS=ALAMI
    echo "Setting OS to Amazon Linux AMI"
else
    echo "This user data script only supports Amazon Linux 2 and Amazon Linux AMI."
fi

# Set Yum HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_yum_http_proxy ]; then
    echo "proxy=http://$PROXY_HOST:$PROXY_PORT" >> /etc/yum.conf
    echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_yum_http_proxy
fi

# Set Docker HTTP proxy (different methods for Amazon Linux 2 and Amazon Linux AMI)
# Amazon Linux 2
if [ $OS == "AL2" ] && [ ! -f /var/lib/cloud/instance/sem/config_docker_http_proxy ]; then
    mkdir /etc/systemd/system/docker.service.d
    cat <<EOF > /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=http://$PROXY_HOST:$PROXY_PORT/"
Environment="HTTPS_PROXY=https://$PROXY_HOST:$PROXY_PORT/"
Environment="NO_PROXY=169.254.169.254,169.254.170.2"
EOF
    systemctl daemon-reload
    if [ "$(systemctl is-active docker)" == "active" ] 
    then 
        systemctl restart docker
    fi 
    echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_docker_http_proxy
fi
# Amazon Linux AMI
if [ $OS == "ALAMI" ] && [ ! -f /var/lib/cloud/instance/sem/config_docker_http_proxy ]; then
    echo "export HTTP_PROXY=http://$PROXY_HOST:$PROXY_PORT/" >> /etc/sysconfig/docker
    echo "export HTTPS_PROXY=https://$PROXY_HOST:$PROXY_PORT/" >> /etc/sysconfig/docker
    echo "export NO_PROXY=169.254.169.254,169.254.170.2" >> /etc/sysconfig/docker
    echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_docker_http_proxy
fi

# Set ECS agent HTTP proxy
if [ ! -f /var/lib/cloud/instance/sem/config_ecs-agent_http_proxy ]; then
    cat <<EOF > /etc/ecs/ecs.config
ECS_CLUSTER=$CLUSTER_NAME
HTTP_PROXY=$PROXY_HOST:$PROXY_PORT
NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock
EOF
    echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_ecs-agent_http_proxy
fi

# Set ecs-init HTTP proxy (different methods for Amazon Linux 2 and Amazon Linux AMI)
# Amazon Linux 2
if [ $OS == "AL2" ] && [ ! -f /var/lib/cloud/instance/sem/config_ecs-init_http_proxy ]; then
    mkdir /etc/systemd/system/ecs.service.d
    cat <<EOF > /etc/systemd/system/ecs.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=$PROXY_HOST:$PROXY_PORT/"
Environment="NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock"
EOF
    systemctl daemon-reload
    if [ "$(systemctl is-active ecs)" == "active" ]; then 
        systemctl restart ecs
    fi 
    echo "$$: $(date +%s.%N | cut -b1-13)" > /var/lib/cloud/instance/sem/config_ecs-init_http_proxy
fi
# Amazon Linux AMI
if [ $OS == "ALAMI" ] && [ ! -f /var/lib/cloud/instance/sem/config_ecs-init_http_proxy ]; then
    cat <<EOF > /etc/init/ecs.override
env HTTP_PROXY=$PROXY_HOST:$PROXY_PORT
env NO_PROXY=169.254.169.254,169.254.170.2,/var/run/docker.sock
EOF
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
To use this script when you launch a container instance, follow the steps in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. Just copy and paste the PowerShell script below into the **User data** field \(be sure to substitute the red example values with your own proxy and cluster information\)\.  
The `-EnableTaskIAMRole` option is required to enable IAM roles for tasks\. For more information, see [Windows IAM roles for tasks](windows_task_IAM_roles.md)\.

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