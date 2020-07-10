# Installing the Amazon ECS Container Agent<a name="ecs-agent-install"></a>

If your container instance was not launched using an Amazon ECS\-optimized AMI, you can install the Amazon ECS container agent manually using one of the following procedures\. The Amazon ECS container agent is included in the Amazon ECS\-optimized AMIs and does not require installation\.
+ For Amazon Linux 2 instances, you can install the agent using the `amazon-linux-extras` command\. For more information, see [Installing the Amazon ECS Container Agent on an Amazon Linux 2 EC2 Instance](#ecs-agent-install-al2)\.
+ For Amazon Linux AMI instances, you can install the agent using the Amazon YUM repo\. For more information, see [Installing the Amazon ECS Container Agent on an Amazon Linux AMI EC2 Instance](#ecs-agent-install-amazonlinux)\.
+ For non\-Amazon Linux instances, you can either download the agent from one of the regional S3 buckets or from Docker Hub\. If you download from one of the regional S3 buckets, you can optionally verify the validity of the container agent file using the PGP signature\. For more information, see [Installing the Amazon ECS Container Agent on a non\-Amazon Linux EC2 Instance](#ecs-agent-install-nonamazonlinux)

**Note**  
The systemd units for both ECS and Docker services have a directive to wait for `cloud-init` to finish before starting both services\. The `cloud-init` process is not considered finished until your Amazon EC2 user data has finished running\. Therefore, starting ECS or Docker via Amazon EC2 user data may cause a deadlock\. To start the container agent using Amazon EC2 user data you can use `systemctl enable --now --no-block ecs.service`\.

## Installing the Amazon ECS Container Agent on an Amazon Linux 2 EC2 Instance<a name="ecs-agent-install-al2"></a>

To install the Amazon ECS container agent on an Amazon Linux 2 EC2 instance using the `amazon-linux-extras` command, use the following steps\.

**To install the Amazon ECS container agent on an Amazon Linux 2 EC2 instance**

1. Launch an Amazon Linux 2 EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Disable the `docker` Amazon Linux extra repository\. The `ecs` Amazon Linux extra repository ships with its own version of Docker, so the `docker` extra must be disabled to avoid any potential future conflicts\. This ensures that you are always using the Docker version that Amazon ECS intends for you to use with a particular version of the container agent\.

   ```
   [ec2-user ~]$ sudo amazon-linux-extras disable docker
   ```

1. Install and enable the `ecs` Amazon Linux extra repository\.

   ```
   [ec2-user ~]$ sudo amazon-linux-extras install -y ecs; sudo systemctl enable --now ecs
   ```

1. \(Optional\) You can verify that the agent is running and see some information about your new container instance with the agent introspection API\. For more information, see [Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)\.

   ```
   [ec2-user ~]$ curl -s http://localhost:51678/v1/metadata | python -mjson.tool
   ```
**Note**  
If you get no response, ensure that you associated the Amazon ECS container instance IAM role when launching the instance\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

## Installing the Amazon ECS Container Agent on an Amazon Linux AMI EC2 Instance<a name="ecs-agent-install-amazonlinux"></a>

To install the Amazon ECS container agent on an Amazon Linux AMI EC2 instance using the Amazon YUM repo, use the following steps\.

**To install the Amazon ECS container agent on an Amazon Linux AMI EC2 instance**

1. Launch an Amazon Linux AMI EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

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
   [ec2-user ~]$ curl -s http://localhost:51678/v1/metadata | python -mjson.tool
   ```

## Installing the Amazon ECS Container Agent on a non\-Amazon Linux EC2 Instance<a name="ecs-agent-install-nonamazonlinux"></a>

To install the Amazon ECS container agent on a non\-Amazon Linux EC2 instance, you can either download the agent from one of the regional S3 buckets or from Docker Hub\. If you download from one of the regional S3 buckets, you can optionally verify the validity of the container agent file using the PGP signature\.

The latest Amazon ECS container agent files, by Region, are listed below for reference\.


| Region | Region Name | Container agent | Container agent signature | 
| --- | --- | --- | --- | 
| us\-east\-2 | US East \(Ohio\) | [ECS container agent](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/ecs-agent-latest.tar) | [PGP signature](https://s3.us-east-2.amazonaws.com/amazon-ecs-agent-us-east-2/ecs-agent-latest.tar.asc) | 
| us\-east\-1 | US East \(N\. Virginia\) | [ECS container agent](https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-latest.tar) | [PGP signature](https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-latest.tar.asc) | 
| us\-west\-1 | US West \(N\. California\) | [ECS container agent](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/ecs-agent-latest.tar) | [PGP signature](https://s3.us-west-1.amazonaws.com/amazon-ecs-agent-us-west-1/ecs-agent-latest.tar.asc) | 
| us\-west\-2 | US West \(Oregon\) | [ECS container agent](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/ecs-agent-latest.tar) | [PGP signature](https://s3.us-west-2.amazonaws.com/amazon-ecs-agent-us-west-2/ecs-agent-latest.tar.asc) | 
| ap\-east\-1 | Asia Pacific \(Hong Kong\) | [ECS container agent](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-east-1.amazonaws.com/amazon-ecs-agent-ap-east-1/ecs-agent-latest.tar.asc) | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) | [ECS container agent](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-northeast-1.amazonaws.com/amazon-ecs-agent-ap-northeast-1/ecs-agent-latest.tar.asc) | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) | [ECS container agent](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-northeast-2.amazonaws.com/amazon-ecs-agent-ap-northeast-2/ecs-agent-latest.tar.asc) | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) | [ECS container agent](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-south-1.amazonaws.com/amazon-ecs-agent-ap-south-1/ecs-agent-latest.tar.asc) | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) | [ECS container agent](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-southeast-1.amazonaws.com/amazon-ecs-agent-ap-southeast-1/ecs-agent-latest.tar.asc) | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) | [ECS container agent](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/ecs-agent-latest.tar) | [PGP signature](https://s3.ap-southeast-2.amazonaws.com/amazon-ecs-agent-ap-southeast-2/ecs-agent-latest.tar.asc) | 
| ca\-central\-1 | Canada \(Central\) | [ECS container agent](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/ecs-agent-latest.tar) | [PGP signature](https://s3.ca-central-1.amazonaws.com/amazon-ecs-agent-ca-central-1/ecs-agent-latest.tar.asc) | 
| eu\-central\-1 | Europe \(Frankfurt\) | [ECS container agent](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/ecs-agent-latest.tar) | [PGP signature](https://s3.eu-central-1.amazonaws.com/amazon-ecs-agent-eu-central-1/ecs-agent-latest.tar.asc) | 
| eu\-west\-1 | Europe \(Ireland\) | [ECS container agent](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/ecs-agent-latest.tar) | [PGP signature](https://s3.eu-west-1.amazonaws.com/amazon-ecs-agent-eu-west-1/ecs-agent-latest.tar.asc) | 
| eu\-west\-2 | Europe \(London\) | [ECS container agent](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/ecs-agent-latest.tar) | [PGP signature](https://s3.eu-west-2.amazonaws.com/amazon-ecs-agent-eu-west-2/ecs-agent-latest.tar.asc) | 
| eu\-west\-3 | Europe \(Paris\) | [ECS container agent](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/ecs-agent-latest.tar) | [PGP signature](https://s3.eu-west-3.amazonaws.com/amazon-ecs-agent-eu-west-3/ecs-agent-latest.tar.asc) | 
| sa\-east\-1 | South America \(São Paulo\) | [ECS container agent](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/ecs-agent-latest.tar) | [PGP signature](https://s3.sa-east-1.amazonaws.com/amazon-ecs-agent-sa-east-1/ecs-agent-latest.tar.asc) | 
| us\-gov\-east\-1 | AWS GovCloud \(US\-East\) | [ECS container agent](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/ecs-agent-latest.tar) | [PGP signature](https://s3.us-gov-east-1.amazonaws.com/amazon-ecs-agent-us-gov-east-1/ecs-agent-latest.tar.asc) | 
| us\-gov\-west\-1 | AWS GovCloud \(US\-West\) | [ECS container agent](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/ecs-agent-latest.tar) | [PGP signature](https://s3.us-gov-west-1.amazonaws.com/amazon-ecs-agent-us-gov-west-1/ecs-agent-latest.tar.asc) | 

**To install the Amazon ECS container agent on a non\-Amazon Linux EC2 instance**

1. Launch an Amazon EC2 instance with an IAM role that allows access to Amazon ECS\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

1. Connect to your instance\.

1. Install the latest version of Docker on your instance\.
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

   In this example, the Docker version is `1.4.1`, which is below the minimum version of 1\.9\.0\. This instance needs to upgrade its Docker version before proceeding\. For information about installing the latest Docker version on your particular Linux distribution, go to [https://docs\.docker\.com/engine/installation/](https://docs.docker.com/engine/installation/)\.

1. Run the following commands on your container instance to allow the port proxy to route traffic using loopback addresses\.

   ```
   ubuntu:~$ sudo sh -c "echo 'net.ipv4.conf.all.route_localnet = 1' >> /etc/sysctl.conf"
   ubuntu:~$ sudo sysctl -p /etc/sysctl.conf
   ```

1. Run the following commands on your container instance to enable IAM roles for tasks\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

   ```
   ubuntu:~$ sudo apt-get install iptables-persistent
   ubuntu:~$ sudo iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
   ubuntu:~$ sudo iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679
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
You can optionally store your agent environment variables in Amazon S3 \(which can be downloaded to your container instances at launch time using Amazon EC2 user data\)\. This is recommended for sensitive information such as authentication credentials for private repositories\. For more information, see [Storing Container Instance Configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3) and [Private registry authentication for tasks](private-auth.md)\.

1. Pull and run the latest Amazon ECS container agent on your container instance\.
**Note**  
Use Docker restart policies or a process manager \(such as upstart or systemd\) to treat the container agent as a service or a daemon and ensure that it is restarted after exiting\. For more information, see [Automatically start containers](https://docs.docker.com/engine/admin/host_integration/) and [Restart policies](https://docs.docker.com/engine/reference/run/#restart-policies-restart) in the Docker documentation\. The Linux variants of the Amazon ECS\-optimized AMI use the `ecs-init` RPM for this purpose, and you can view the [source code for this RPM](https://github.com/aws/amazon-ecs-init) on GitHub\. For example systemd unit files for Ubuntu 16\.04 and CentOS 7, see [Example Container Instance User Data Configuration Scripts](example_user_data_scripts.md)\.

   The following example of the agent run command is broken into separate lines to show each option\. For more information about these and other agent runtime options, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.
**Important**  
Operating systems with SELinux enabled require the `--privileged` option in your docker run command\. In addition, for SELinux\-enabled container instances, we recommend that you add the `:Z` option to the `/log` and `/data` volume mounts\. However, the host mounts for these volumes must exist before you run the command or you receive a `no such file or directory` error\. Take the following action if you experience difficulty running the Amazon ECS agent on an SELinux\-enabled container instance:  
Create the host volume mount points on your container instance\.  

     ```
     ubuntu:~$ sudo mkdir -p /var/log/ecs /var/lib/ecs/data
     ```
Add the `--privileged` option to the docker run command below\.
Append the `:Z` option to the `/log` and `/data` container volume mounts \(for example, `--volume=/var/log/ecs/:/log:Z`\) to the docker run command below\.

   1. \(Optional\) Download the ECS container agent tarball from the regional S3 URL and load it\. If you don't download the agent tarball from S3, the `docker run` command in the next step will download it from Docker Hub for you automatically\.

      ```
      ubuntu:~$ curl -o ecs-agent.tar https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-latest.tar
      ```
**Note**  
To download other versions of the Amazon ECS container agent, use one of the following formats, changing the version number in the URL:  

      ```
      ecs-agent-<version>.tar
      ecs-agent-<SHA>.tar
      ```
For example:  

      ```
      https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-v1.18.0.tar
      https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-c0defea9.tar
      ```

      Load the ECS container agent image\.

      ```
      ubuntu:~$ sudo docker load --input ./ecs-agent.tar
      ```

   1. Run the ECS container agent image\.

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
**Important**  
The `host` network mode is the only supported network mode for the container agent container\. For more information, see [Running the Amazon ECS Container Agent with Host Network Mode](#container_agent_host)\.
**Note**  
If you receive an `Error response from daemon: Cannot start container` message, you can delete the failed container with the sudo docker rm ecs\-agent command and try running the agent again\. 

1. \(Optional\) If you downloaded the Amazon ECS container agent file from S3, you can verify the validity of the file\.

   1. Download and install GnuPG\. For more information about GNUpg, see the [GnuPG website](https://www.gnupg.org)\. For Linux systems, install `gpg` using the package manager on your flavor of Linux\.

   1. Retrieve the Amazon ECS PGP public key\. You can use a command to do this or manually create the key and then import it\.

      1. Option 1: Retrieve the key with the following command\.

         ```
         gpg --keyserver hkp://keys.gnupg.net --recv BCE9D9A42D51784F
         ```

      1. Option 2: Create a file with the following contents of the Amazon ECS PGP public key and then import it:

         ```
         -----BEGIN PGP PUBLIC KEY BLOCK-----
         Version: GnuPG v2
         
         mQINBFq1SasBEADliGcT1NVJ1ydfN8DqebYYe9ne3dt6jqKFmKowLmm6LLGJe7HU
         jGtqhCWRDkN+qPpHqdArRgDZAtn2pXY5fEipHgar4CP8QgRnRMO2fl74lmavr4Vg
         7K/KH8VHlq2uRw32/B94XLEgRbGTMdWFdKuxoPCttBQaMj3LGn6Pe+6xVWRkChQu
         BoQAhjBQ+bEm0kNy0LjNgjNlnL3UMAG56t8E3LANIgGgEnpNsB1UwfWluPoGZoTx
         N+6pHBJrKIL/1v/ETU4FXpYw2zvhWNahxeNRnoYj3uycHkeliCrw4kj0+skizBgO
         2K7oVX8Oc3j5+ZilhL/qDLXmUCb2az5cMM1mOoF8EKX5HaNuq1KfwJxqXE6NNIcO
         lFTrT7QwD5fMNld3FanLgv/ZnIrsSaqJOL6zRSq8O4LN1OWBVbndExk2Kr+5kFxn
         5lBPgfPgRj5hQ+KTHMa9Y8Z7yUc64BJiN6F9Nl7FJuSsfqbdkvRLsQRbcBG9qxX3
         rJAEhieJzVMEUNl+EgeCkxj5xuSkNU7zw2c3hQZqEcrADLV+hvFJktOz9Gm6xzbq
         lTnWWCz4xrIWtuEBA2qE+MlDheVd78a3gIsEaSTfQq0osYXaQbvlnSWOoc1y/5Zb
         zizHTJIhLtUyls9WisP2s0emeHZicVMfW61EgPrJAiupgc7kyZvFt4YwfwARAQAB
         tCRBbWF6b24gRUNTIDxlY3Mtc2VjdXJpdHlAYW1hem9uLmNvbT6JAhwEEAECAAYF
         AlrjL0YACgkQHivRXs0TaQrg1g/+JppwPqHnlVPmv7lessB8I5UqZeD6p6uVpHd7
         Bs3pcPp8BV7BdRbs3sPLt5bV1+rkqOlw+0gZ4Q/ue/YbWtOAt4qY0OcEo0HgcnaX
         lsB827QIfZIVtGWMhuh94xzm/SJkvngml6KB3YJNnWP61A9qJ37/VbVVLzvcmazA
         McWB4HUMNrhd0JgBCo0gIpqCbpJEvUc02Bjn23eEJsS9kC7OUAHyQkVnx4d9UzXF
         4OoISF6hmQKIBoLnRrAlj5Qvs3GhvHQ0ThYq0Grk/KMJJX2CSqt7tWJ8gk1n3H3Y
         SReRXJRnv7DsDDBwFgT6r5Q2HW1TBUvaoZy5hF6maD09nHcNnvBjqADzeT8Tr/Qu
         bBCLzkNSYqqkpgtwv7seoD2P4n1giRvDAOEfMZpVkUr+C252IaH1HZFEz+TvBVQM
         Y8OWWxmIJW+J6evjo3N1eO19UHv71jvoF8zljbI4bsL2c+QTJmOv7nRqzDQgCWyp
         Id/v2dUVVTk1j9omuLBBwNJzQCB+72LcIzJhYmaP1HC4LcKQG+/f41exuItenatK
         lEJQhYtyVXcBlh6Yn/wzNg2NWOwb3vqY/F7m6u9ixAwgtIMgPCDE4aJ86zrrXYFz
         N2HqkTSQh77Z8KPKmyGopsmN/reMuilPdINb249nA0dzoN+nj+tTFOYCIaLaFyjs
         Z0r1QAOJAjkEEwECACMFAlq1SasCGwMHCwkIBwMCAQYVCAIJCgsEFgIDAQIeAQIX
         gAAKCRC86dmkLVF4T9iFEACEnkm1dNXsWUx34R3c0vamHrPxvfkyI1FlEUen8D1h
         uX9xy6jCEROHWEp0rjGK4QDPgM93sWJ+s1UAKg214QRVzft0y9/DdR+twApA0fzy
         uavIthGd6+03jAAo6udYDE+cZC3P7XBbDiYEWk4XAF9I1JjB8hTZUgvXBL046JhG
         eM17+crgUyQeetkiOQemLbsbXQ40Bd9V7zf7XJraFd8VrwNUwNb+9KFtgAsc9rk+
         YIT/PEf+YOPysgcxI4sTWghtyCulVnuGoskgDv4v73PALU0ieUrvvQVqWMRvhVx1
         0X90J7cC1KOyhlEQQ1aFTgmQjmXexVTwIBm8LvysFK6YXM41KjOrlz3+6xBIm/qe
         bFyLUnf4WoiuOplAaJhK9pRY+XEnGNxdtN4D26Kd0F+PLkm3Tr3Hy3b1Ok34FlGr
         KVHUq1TZD7cvMnnNKEELTUcKX+1mV3an16nmAg/my1JSUt6BNK2rJpY1s/kkSGSE
         XQ4zuF2IGCpvBFhYAlt5Un5zwqkwwQR3/n2kwAoDzonJcehDw/C/cGos5D0aIU7I
         K2X2aTD3+pA7Mx3IMe2hqmYqRt9X42yF1PIEVRneBRJ3HDezAgJrNh0GQWRQkhIx
         gz6/cTR+ekr5TptVszS9few2GpI5bCgBKBisZIssT89aw7mAKWut0Gcm4qM9/yK6
         1bkCDQRatUmrARAAxNPvVwreJ2yAiFcUpdRlVhsuOgnxvs1QgsIw3H7+Pacr9Hpe
         8uftYZqdC82KeSKhpHq7c8gMTMucIINtH25x9BCc73E33EjCL9Lqov1TL7+QkgHe
         T+JIhZwdD8Mx2K+LVVVu/aWkNrfMuNwyDUciSI4D5QHa8T+F8fgN4OTpwYjirzel
         5yoICMr9hVcbzDNv/ozKCxjx+XKgnFc3wrnDfJfntfDAT7ecwbUTL+viQKJ646s+
         psiqXRYtVvYInEhLVrJ0aV6zHFoigE/Bils6/g7ru1Q6CEHqEw++APs5CcE8VzJu
         WAGSVHZgun5Y9N4quR/M9Vm+IPMhTxrAg7rOvyRN9cAXfeSMf77I+XTifigNna8x
         t/MOdjXr1fjF4pThEi5u6WsuRdFwjY2azEv3vevodTi4HoJReH6dFRa6y8c+UDgl
         2iHiOKIpQqLbHEfQmHcDd2fix+AaJKMnPGNku9qCFEMbgSRJpXz6BfwnY1QuKE+I
         R6jA0frUNt2jhiGG/F8RceXzohaaC/Cx7LUCUFWc0n7z32C9/Dtj7I1PMOacdZzz
         bjJzRKO/ZDv+UN/c9dwAkllzAyPMwGBkUaY68EBstnIliW34aWm6IiHhxioVPKSp
         VJfyiXPO0EXqujtHLAeChfjcns3I12YshT1dv2PafG53fp33ZdzeUgsBo+EAEQEA
         AYkCHwQYAQIACQUCWrVJqwIbDAAKCRC86dmkLVF4T+ZdD/9x/8APzgNJF3o3STrF
         jvnV1ycyhWYGAeBJiu7wjsNWwzMFOv15tLjB7AqeVxZn+WKDD/mIOQ45OZvnYZuy
         X7DR0JszaH9wrYTxZLVruAu+t6UL0y/XQ4L1GZ9QR6+r+7t1Mvbfy7BlHbvX/gYt
         Rwe/uwdibI0CagEzyX+2D3kTOlHO5XThbXaNf8AN8zha91Jt2Q2UR2X5T6JcwtMz
         FBvZnl3LSmZyE0EQehS2iUurU4uWOpGppuqVnbi0jbCvCHKgDGrqZ0smKNAQng54
         F365W3g8AfY48s8XQwzmcliowYX9bT8PZiEi0J4QmQh0aXkpqZyFefuWeOL2R94S
         XKzr+gRh3BAULoqF+qK+IUMxTip9KTPNvYDpiC66yBiT6gFDji5Ca9pGpJXrC3xe
         TXiKQ8DBWDhBPVPrruLIaenTtZEOsPc4I85yt5U9RoPTStcOr34s3w5yEaJagt6S
         Gc5r9ysjkfH6+6rbi1ujxMgROSqtqr+RyB+V9A5/OgtNZc8llK6u4UoOCde8jUUW
         vqWKvjJB/Kz3u4zaeNu2ZyyHaOqOuH+TETcW+jsY9IhbEzqN5yQYGi4pVmDkY5vu
         lXbJnbqPKpRXgM9BecV9AMbPgbDq/5LnHJJXg+G8YQOgp4lR/hC1TEFdIp5wM8AK
         CWsENyt2o1rjgMXiZOMF8A5oBLkCDQRatUuSARAAr77kj7j2QR2SZeOSlFBvV7oS
         mFeSNnz9xZssqrsm6bTwSHM6YLDwc7Sdf2esDdyzONETwqrVCg+FxgL8hmo9hS4c
         rR6tmrP0mOmptr+xLLsKcaP7ogIXsyZnrEAEsvW8PnfayoiPCdc3cMCR/lTnHFGA
         7EuR/XLBmi7Qg9tByVYQ5Yj5wB9V4B2yeCt3XtzPqeLKvaxl7PNelaHGJQY/xo+m
         V0bndxf9IY+4oFJ4blD32WqvyxESo7vW6WBh7oqv3Zbm0yQrr8a6mDBpqLkvWwNI
         3kpJR974tg5o5LfDu1BeeyHWPSGm4U/G4JB+JIG1ADy+RmoWEt4BqTCZ/knnoGvw
         D5sTCxbKdmuOmhGyTssoG+3OOcGYHV7pWYPhazKHMPm201xKCjH1RfzRULzGKjD+
         yMLT1I3AXFmLmZJXikAOlvE3/wgMqCXscbycbLjLD/bXIuFWo3rzoezeXjgi/DJx
         jKBAyBTYO5nMcth1O9oaFd9d0HbsOUDkIMnsgGBE766Piro6MHo0T0rXl07Tp4pI
         rwuSOsc6XzCzdImj0Wc6axS/HeUKRXWdXJwno5awTwXKRJMXGfhCvSvbcbc2Wx+L
         IKvmB7EB4K3fmjFFE67yolmiw2qRcUBfygtH3eL5XZU28MiCpue8Y8GKJoBAUyvf
         KeM1rO8Jm3iRAc5a/D0AEQEAAYkEPgQYAQIACQUCWrVLkgIbAgIpCRC86dmkLVF4
         T8FdIAQZAQIABgUCWrVLkgAKCRDePL1hra+LjtHYD/9MucxdFe6bXO1dQR4tKhhQ
         P0LRqy6zlBY9ILCLowNdGZdqorogUiUymgn3VhEhVtxTOoHcN7qOuM01PNsRnOeS
         EYjf8Xrb1clzkD6xULwmOclTb9bBxnBc/4PFvHAbZW3QzusaZniNgkuxt6BTfloS
         Of4inq71kjmGK+TlzQ6mUMQUg228NUQC+a84EPqYyAeY1sgvgB7hJBhYL0QAxhcW
         6m20Rd8iEc6HyzJ3yCOCsKip/nRWAbf0OvfHfRBp0+m0ZwnJM8cPRFjOqqzFpKH9
         HpDmTrC4wKP1+TL52LyEqNh4yZitXmZNV7giSRIkk0eDSko+bFy6VbMzKUMkUJK3
         D3eHFAMkujmbfJmSMTJOPGn5SB1HyjCZNx6bhIIbQyEUB9gKCmUFaqXKwKpF6rj0
         iQXAJxLR/shZ5Rk96VxzOphUl7T90m/PnUEEPwq8KsBhnMRgxa0RFidDP+n9fgtv
         HLmrOqX9zBCVXh0mdWYLrWvmzQFWzG7AoE55fkf8nAEPsalrCdtaNUBHRXA0OQxG
         AHMOdJQQvBsmqMvuAdjkDWpFu5y0My5ddU+hiUzUyQLjL5Hhd5LOUDdewlZgIw1j
         xrEAUzDKetnemM8GkHxDgg8koev5frmShJuce7vSjKpCNg3EIJSgqMOPFjJuLWtZ
         vjHeDNbJy6uNL65ckJy6WhGjEADS2WAW1D6Tfekkc21SsIXk/LqEpLMR/0g5OUif
         wcEN1rS9IJXBwIy8MelN9qr5KcKQLmfdfBNEyyceBhyVl0MDyHOKC+7PofMtkGBq
         13QieRHv5GJ8LB3fclqHV8pwTTo3Bc8z2g0TjmUYAN/ixETdReDoKavWJYSE9yoM
         aaJu279ioVTrwpECse0XkiRyKToTjwOb73CGkBZZpJyqux/rmCV/fp4ALdSW8zbz
         FJVORaivhoWwzjpfQKhwcU9lABXi2UvVm14v0AfeI7oiJPSU1zM4fEny4oiIBXlR
         zhFNih1UjIu82X16mTm3BwbIga/s1fnQRGzyhqUIMii+mWra23EwjChaxpvjjcUH
         5ilLc5Zq781aCYRygYQw+hu5nFkOH1R+Z50Ubxjd/aqUfnGIAX7kPMD3Lof4KldD
         Q8ppQriUvxVo+4nPV6rpTy/PyqCLWDjkguHpJsEFsMkwajrAz0QNSAU5CJ0G2Zu4
         yxvYlumHCEl7nbFrm0vIiA75Sa8KnywTDsyZsu3XcOcf3g+g1xWTpjJqy2bYXlqz
         9uDOWtArWHOis6bq8l9RE6xr1RBVXS6uqgQIZFBGyq66b0dIq4D2JdsUvgEMaHbc
         e7tBfeB1CMBdA64e9Rq7bFR7Tvt8gasCZYlNr3lydh+dFHIEkH53HzQe6l88HEic
         +0jVnLkCDQRa55wJARAAyLya2Lx6gyoWoJN1a6740q3o8e9d4KggQOfGMTCflmeq
         ivuzgN+3DZHN+9ty2KxXMtn0mhHBerZdbNJyjMNT1gAgrhPNB4HtXBXum2wS57WK
         DNmade914L7FWTPAWBG2Wn448OEHTqsClICXXWy9IICgclAEyIq0Yq5mAdTEgRJS
         Z8t4GpwtDL9gNQyFXaWQmDmkAsCygQMvhAlmu9xOIzQG5CxSnZFk7zcuL60k14Z3
         Cmt49k4T/7ZU8goWi8tt+rU78/IL3J/fF9+1civ1OwuUidgfPCSvOUW1JojsdCQA
         L+RZJcoXq7lfOFj/eNjeOSstCTDPfTCL+kThE6E5neDtbQHBYkEX1BRiTedsV4+M
         ucgiTrdQFWKf89G72xdv8ut9AYYQ2BbEYU+JAYhUH8rYYui2dHKJIgjNvJscuUWb
         +QEqJIRleJRhrO+/CHgMs4fZAkWF1VFhKBkcKmEjLn1f7EJJUUW84ZhKXjO/AUPX
         1CHsNjziRceuJCJYox1cwsoq6jTE50GiNzcIxTn9xUc0UMKFeggNAFys1K+TDTm3
         Bzo8H5ucjCUEmUm9lhkGwqTZgOlRX5eqPX+JBoSaObqhgqCa5IPinKRa6MgoFPHK
         6sYKqroYwBGgZm6Js5chpNchvJMs/3WXNOEVg0J3z3vP0DMhxqWm+r+n9zlW8qsA
         EQEAAYkEPgQYAQgACQUCWuecCQIbAgIpCRC86dmkLVF4T8FdIAQZAQgABgUCWuec
         CQAKCRBQ3szEcQ5hr+ykD/4tOLRHFHXuKUcxgGaubUcVtsFrwBKma1cYjqaPms8u
         6Sk0wfGRI32G/GhOrp0Ts/MOkbObq6VLTh8N5Yc/53MEl8zQFw9Y5AmRoW4PZXER
         ujs5s7p4oR7xHMihMjCCBn1bvrR+34YPfgzTcgLiOEFHYT8UTxwnGmXOvNkMM7md
         xD3CV5q6VAte8WKBo/220II3fcQlc9r/oWX4kXXkb0v9hoGwKbDJ1tzqTPrp/xFt
         yohqnvImpnlz+Q9zXmbrWYL9/g8VCmW/NN2gju2G3Lu/TlFUWIT4v/5OPK6TdeNb
         VKJO4+S8bTayqSG9CML1S57KSgCo5HUhQWeSNHI+fpe5oX6FALPT9JLDce8OZz1i
         cZZ0MELP37mOOQun0AlmHm/hVzf0f311PtbzcqWaE51tJvgUR/nZFo6Ta3O5Ezhs
         3VlEJNQ1Ijf/6DH87SxvAoRIARCuZd0qxBcDK0avpFzUtbJd24lRA3WJpkEiMqKv
         RDVZkE4b6TW61f0o+LaVfK6E8oLpixegS4fiqC16mFrOdyRk+RJJfIUyz0WTDVmt
         g0U1CO1ezokMSqkJ7724pyjr2xf/r9/sC6aOJwB/lKgZkJfC6NqL7TlxVA31dUga
         LEOvEJTTE4gl+tYtfsCDvALCtqL0jduSkUo+RXcBItmXhA+tShW0pbS2Rtx/ixua
         KohVD/0R4QxiSwQmICNtm9mw9ydIl1yjYXX5a9x4wMJracNY/LBybJPFnZnT4dYR
         z4XjqysDwvvYZByaWoIe3QxjX84V6MlI2IdAT/xImu8gbaCI8tmyfpIrLnPKiR9D
         VFYfGBXuAX7+HgPPSFtrHQONCALxxzlbNpS+zxt9r0MiLgcLyspWxSdmoYGZ6nQP
         RO5Nm/ZVS+u2imPCRzNUZEMa+dlE6kHx0rS0dPiuJ4O7NtPeYDKkoQtNagspsDvh
         cK7CSqAiKMq06UBTxqlTSRkm62eOCtcs3p3OeHu5GRZF1uzTET0ZxYkaPgdrQknx
         ozjP5mC7X+45lcCfmcVt94TFNL5HwEUVJpmOgmzILCI8yoDTWzloo+i+fPFsXX4f
         kynhE83mSEcr5VHFYrTY3mQXGmNJ3bCLuc/jq7ysGq69xiKmTlUeXFm+aojcRO5i
         zyShIRJZ0GZfuzDYFDbMV9amA/YQGygLw//zP5ju5SW26dNxlf3MdFQE5JJ86rn9
         MgZ4gcpazHEVUsbZsgkLizRp9imUiH8ymLqAXnfRGlU/LpNSefnvDFTtEIRcpOHc
         bhayG0bk51Bd4mioOXnIsKy4j63nJXA27x5EVVHQ1sYRN8Ny4Fdr2tMAmj2O+X+J
         qX2yy/UX5nSPU492e2CdZ1UhoU0SRFY3bxKHKB7SDbVeav+K5g==
         =Gi5D
         -----END PGP PUBLIC KEY BLOCK-----
         ```

         The details of the Amazon ECS PGP public key for reference:

         ```
         Key ID: BCE9D9A42D51784F
         Type: RSA
         Size: 4096/4096
         Expires: Never
         User ID: Amazon ECS
         Key fingerprint: F34C 3DDA E729 26B0 79BE AEC6 BCE9 D9A4 2D51 784F
         ```

         Import the Amazon ECS PGP public key with the following command\.

         ```
         gpg --import <public_key_filename>
         ```

   1. Download the ECS container agent signature\. ECS container agent signatures are ascii detached PGP signatures stored in files with the extension `.asc`\. The signatures file has the same name as its corresponding executable, with `.asc` appended\.

      ```
      curl -o ecs-agent.asc https://s3.amazonaws.com/amazon-ecs-agent-us-east-1/ecs-agent-latest.tar.asc 
      ```

   1. Verify the signature\.

      ```
      gpg --verify ecs-agent.asc ./ecs-agent.tar
      ```

      Expected output:

      ```
      gpg: Signature made Wed 16 May 2018 08:21:06 PM UTC using RSA key ID 710E61AF
      gpg: Good signature from "Amazon ECS <ecs-security@amazon.com>" [unknown]
      gpg: WARNING: This key is not certified with a trusted signature!
      gpg:          There is no indication that the signature belongs to the owner.
      Primary key fingerprint: F34C 3DDA E729 26B0 79BE  AEC6 BCE9 D9A4 2D51 784F
           Subkey fingerprint: D64B B6F9 0CF3 77E9 B5FB  346F 50DE CCC4 710E 61AF
      ```
**Note**  
The warning in the output is expected and is not problematic; it occurs because there is not a chain of trust between your personal PGP key \(if you have one\) and the Amazon ECS PGP key\. For more information, see [Web of trust](https://en.wikipedia.org/wiki/Web_of_trust)\.

## Running the Amazon ECS Container Agent with Host Network Mode<a name="container_agent_host"></a>

When running the Amazon ECS container agent, `ecs-init` will create the container agent container with the `host` network mode\. This is the only supported network mode for the container agent container\. 

This enables you to block access to the [Amazon EC2 instance metadata service endpoint](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) \(`http://169.254.169.254`\) for the containers started by the container agent\. This ensures that containers cannot access IAM role credentials from the container instance profile and enforces that tasks use only the IAM task role credentials\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

This also makes it so the container agent doesn't contend for connections and network traffic on the `docker0` bridge\.