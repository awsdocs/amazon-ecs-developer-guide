# Installing the Amazon ECS container agent<a name="ecs-agent-install"></a>

If your container instance was not launched using an Amazon ECS\-optimized AMI, you can install the Amazon ECS container agent manually using one of the following procedures\. The Amazon ECS container agent is included in the Amazon ECS\-optimized AMIs and does not require installation\.
+ For Amazon Linux 2 instances, you can install the agent using the `amazon-linux-extras` command\. For more information, see [Installing the Amazon ECS container agent on an Amazon Linux 2 EC2 instance](#ecs-agent-install-al2)\.
+ For Amazon Linux AMI instances, you can install the agent using the Amazon YUM repo\. For more information, see [Installing the Amazon ECS container agent on an Amazon Linux AMI EC2 instance](#ecs-agent-install-amazonlinux)\.
+ For non\-Amazon Linux instances, you can either download the agent from one of the regional S3 buckets or from Amazon Elastic Container Registry Public\. If you download from one of the regional S3 buckets, you can optionally verify the validity of the container agent file using the PGP signature\. For more information, see [Installing the Amazon ECS container agent on a non\-Amazon Linux EC2 instance](#ecs-agent-install-nonamazonlinux)\.

**Note**  
The `systemd` units for both Amazon ECS and Docker services have a directive to wait for `cloud-init` to finish before starting both services\. The `cloud-init` process is not considered finished until your Amazon EC2 user data has finished running\. Therefore, starting Amazon ECS or Docker via Amazon EC2 user data may cause a deadlock\. To start the container agent using Amazon EC2 user data you can use `systemctl enable --now --no-block ecs.service`\.

## Installing the Amazon ECS container agent on an Amazon Linux 2 EC2 instance<a name="ecs-agent-install-al2"></a>

To install the Amazon ECS container agent on an Amazon Linux 2 EC2 instance using the `amazon-linux-extras` command, use the following steps\.

**To install the Amazon ECS container agent on an Amazon Linux 2 EC2 instance**

1. Launch an Amazon Linux 2 EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Disable the `docker` Amazon Linux extra repository\. The `ecs` Amazon Linux extra repository ships with its own version of Docker, so the `docker` extra must be turned off to avoid any potential future conflicts\. This ensures that you are always using the Docker version that Amazon ECS intends for you to use with a particular version of the container agent\.

   ```
   [ec2-user ~]$ sudo amazon-linux-extras disable docker
   ```

1. Install and enable the `ecs` Amazon Linux extra repository\.

   ```
   [ec2-user ~]$ sudo amazon-linux-extras install -y ecs; sudo systemctl enable --now ecs
   ```

1. \(Optional\) You can verify that the agent is running and see some information about your new container instance with the agent introspection API\. For more information, see [Amazon ECS container agent introspection](ecs-agent-introspection.md)\.

   ```
   [ec2-user ~]$ curl -s http://localhost:51678/v1/metadata | python -mjson.tool
   ```
**Note**  
If you get no response, ensure that you associated the Amazon ECS container instance IAM role when launching the instance\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

## Installing the Amazon ECS container agent on an Amazon Linux AMI EC2 instance<a name="ecs-agent-install-amazonlinux"></a>

To install the Amazon ECS container agent on an Amazon Linux AMI EC2 instance using the Amazon YUM repo, use the following steps\.

**To install the Amazon ECS container agent on an Amazon Linux AMI EC2 instance**

1. Launch an Amazon Linux AMI EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

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
   [ec2-user ~]$ sudo service ecs start
   ```

   Output:

   ```
   ecs start/running, process 2804
   ```

1. \(Optional\) You can verify that the agent is running and see some information about your new container instance with the agent introspection API\. For more information, see [Amazon ECS container agent introspection](ecs-agent-introspection.md)\.

   ```
   [ec2-user ~]$ curl -s http://localhost:51678/v1/metadata | python -mjson.tool
   ```

## Installing the Amazon ECS container agent on a non\-Amazon Linux EC2 instance<a name="ecs-agent-install-nonamazonlinux"></a>

To install the Amazon ECS container agent on a non\-Amazon Linux EC2 instance, you can download the agent from one of the regional S3 buckets and install it\.

**Note**  
When using a non\-Amazon Linux AMI, your Amazon EC2 instance requires `cgroupfs` support for the `cgroup` driver in order for the Amazon ECS agent to support task level resource limits\. For more information, see [Amazon ECS agent on GitHub](https://github.com/aws/amazon-ecs-agent)\.

The latest Amazon ECS container agent files, by Region, for each system architecture are listed below for reference\.


| Region | Region name | Amazon ECS init deb files | Amazon ECS init rpm files | 
| --- | --- | --- | --- | 
| us\-east\-2 | US East \(Ohio\) |  [Amazon ECS init amd64](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| us\-east\-1 | US East \(N\. Virginia\) |  [Amazon ECS init amd64](https://s3.us-east-1.amazonaws.com/amazon-ecs-agent-us-east-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-east-1.amazonaws.com/amazon-ecs-agent-us-east-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-east-1.amazonaws.com/amazon-ecs-agent-us-east-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-east-1.amazonaws.com/amazon-ecs-agent-us-east-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| us\-west\-1 | US West \(N\. California\) |  [Amazon ECS init amd64](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| us\-west\-2 | US West \(Oregon\) |  [Amazon ECS init amd64](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-east\-1 | Asia Pacific \(Hong Kong\) |  [Amazon ECS init amd64](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) |  [Amazon ECS init amd64](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) |  [Amazon ECS init amd64](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) |  [Amazon ECS init amd64](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) |  [Amazon ECS init amd64](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) |  [Amazon ECS init amd64](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| ca\-central\-1 | Canada \(Central\) |  [Amazon ECS init amd64](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| eu\-central\-1 | Europe \(Frankfurt\) |  [Amazon ECS init amd64](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| eu\-west\-1 | Europe \(Ireland\) |  [Amazon ECS init amd64](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| eu\-west\-2 | Europe \(London\) |  [Amazon ECS init amd64](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| eu\-west\-3 | Europe \(Paris\) |  [Amazon ECS init amd64](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| sa\-east\-1 | South America \(São Paulo\) |  [Amazon ECS init amd64](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/amazon-ecs-init-latest.x86_64.rpm) [Amazon ECS init aarch64](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| us\-gov\-east\-1 | AWS GovCloud \(US\-East\) |  [Amazon ECS init amd64](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 
| us\-gov\-west\-1 | AWS GovCloud \(US\-West\) |  [Amazon ECS init amd64](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/amazon-ecs-init-latest.amd64.deb) \(amd64\) [Amazon ECS init arm64](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/amazon-ecs-init-latest.arm64.deb) \(arm64\)  |  [Amazon ECS init x86\_64](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/amazon-ecs-init-latest.x86_64.rpm) \(x86\_64\) [Amazon ECS init aarch64](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/amazon-ecs-init-latest.aarch64.rpm) \(aarch64\)  | 

**To install the Amazon ECS container agent on an Amazon EC2 instance using a non\-Amazon Linux AMI**

1. Launch an Amazon EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Install the latest version of Docker on your instance\.

1. Check your Docker version to verify that your system meets the minimum version requirement\.

   ```
   docker --version
   ```

1. Download the appropriate Amazon ECS agent file for your operating system and system architecture and install it\.

   For `deb` architectures:

   ```
   ubuntu:~$ curl -O https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.amd64.deb
   ubuntu:~$ sudo dpkg -i amazon-ecs-init-latest.amd64.deb
   ```

   For `rpm` architectures:

   ```
   fedora:~$ curl -O https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/amazon-ecs-init-latest.x86_64.rpm
   fedora:~$ sudo yum localinstall -y amazon-ecs-init-latest.x86_64.rpm
   ```

1. \(Optional\) To register the instance with a cluster other than the `default` cluster, edit the `/etc/ecs/ecs.config` file and add the following contents\. The following example specifies the `MyCluster` cluster\.

   ```
   ECS_CLUSTER=MyCluster
   ```

   For more information about these and other agent runtime options, see [Amazon ECS container agent configuration](ecs-agent-config.md)\. 
**Note**  
You can optionally store your agent environment variables in Amazon S3 \(which can be downloaded to your container instances at launch time using Amazon EC2 user data\)\. This is recommended for sensitive information such as authentication credentials for private repositories\. For more information, see [Storing container instance configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3) and [Private registry authentication for tasks](private-auth.md)\.

1. Start the `ecs` service\.

   ```
   ubuntu:~$ sudo systemctl start ecs
   ```

## Running the Amazon ECS agent with host network mode<a name="container_agent_host"></a>

When running the Amazon ECS container agent, `ecs-init` will create the container agent container with the `host` network mode\. This is the only supported network mode for the container agent container\. 

This allows you to block access to the [Amazon EC2 instance metadata service endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) \(`http://169.254.169.254`\) for the containers started by the container agent\. This ensures that containers cannot access IAM role credentials from the container instance profile and enforces that tasks use only the IAM task role credentials\. For more information, see [Task IAM role](task-iam-roles.md)\.

This also makes it so the container agent doesn't contend for connections and network traffic on the `docker0` bridge\.