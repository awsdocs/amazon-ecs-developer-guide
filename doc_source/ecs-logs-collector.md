# Amazon ECS logs collector<a name="ecs-logs-collector"></a>

If you are unsure how to collect all of the various logs on your container instances, you can use the Amazon ECS logs collector\. It is available on GitHub for both [Linux](https://github.com/awslabs/ecs-logs-collector) and [Windows](https://github.com/awslabs/aws-ecs-logs-collector-for-windows)\. The script collects general operating system logs as well as Docker and Amazon ECS container agent logs, which can be helpful for troubleshooting AWS Support cases\. It then compresses and archives the collected information into a single file that can easily be shared for diagnostic purposes\. It also supports enabling debug mode for the Docker daemon and the Amazon ECS container agent on Amazon Linux variants, such as the Amazon ECS\-optimized AMI\. Currently, the Amazon ECS logs collector supports the following operating systems:
+ Amazon Linux
+ Red Hat Enterprise Linux 7
+ Debian 8
+ Ubuntu 14\.04
+ Windows Server 2016

**Note**  
The source code for the Amazon ECS logs collector is available on GitHub for both [Linux](https://github.com/awslabs/ecs-logs-collector) and [Windows](https://github.com/awslabs/aws-ecs-logs-collector-for-windows)\. We encourage you to submit pull requests for changes that you would like to have included\. However, Amazon Web Services does not currently support running modified copies of this software\.

**To download and run the Amazon ECS logs collector for Linux**

1. Connect to your container instance\. For more information, see [Connect to your container instance](instance-connect.md)\.

1. Download the Amazon ECS logs collector script\.

   ```
   curl -O https://raw.githubusercontent.com/awslabs/ecs-logs-collector/master/ecs-logs-collector.sh
   ```

1. Run the script to collect the logs and create the archive\.
**Note**  
To enable the debug mode for the Docker daemon and the Amazon ECS container agent, add the `--mode=enable-debug` option to the command below\. This may restart the Docker daemon, which kills all containers that are running on the instance\. Consider draining the container instance and moving any important tasks to other container instances before enabling debug mode\. For more information, see [Container instance draining](container-instance-draining.md)\.

   ```
   [ec2-user ~]$ sudo bash ./ecs-logs-collector.sh
   ```

After you have run the script, you can examine the collected logs in the `collect` folder that the script created\. The `collect.tgz` file is a compressed archive of all of the logs, which you can share with AWS Support for diagnostic help\.

**To download and run the Amazon ECS logs collector for Windows**

1. Connect to your container instance\. For more information, see [Connecting to Your Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Download the Amazon ECS logs collector script using PowerShell\.

   ```
   Invoke-WebRequest -OutFile ecs-logs-collector.ps1 https://raw.githubusercontent.com/awslabs/aws-ecs-logs-collector-for-windows/master/ecs-logs-collector.ps1
   ```

1. Run the script to collect the logs and create the archive\.
**Note**  
To enable the debug mode for the Docker daemon and the Amazon ECS container agent, add the `-RunMode debug` option to the command below\. This restarts the Docker daemon, which kills all containers that are running on the instance\. Consider draining the container instance and moving any important tasks to other container instances before enabling debug mode\. For more information, see [Container instance draining](container-instance-draining.md)\.

   ```
   .\ecs-logs-collector.ps1
   ```

After you have run the script, you can examine the collected logs in the `collect` folder that the script created\. The `collect.tgz` file is a compressed archive of all of the logs, which you can share with AWS Support for diagnostic help\.