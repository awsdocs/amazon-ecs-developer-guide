# Using CloudWatch Logs with Container Instances<a name="using_cloudwatch_logs"></a>

You can configure your container instances to send log information to CloudWatch Logs\. This enables you to view different logs from your container instances in one convenient location\. This topic helps you get started using CloudWatch Logs on your container instances that were launched with the Amazon ECS\-optimized Amazon Linux AMI\.

For information about sending container logs from your tasks to CloudWatch Logs, see [Using the awslogs Log Driver](using_awslogs.md)\. For more information about CloudWatch Logs, see [Monitoring Log Files](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.

**Topics**
+ [CloudWatch Logs IAM Policy](#cwl_iam_policy)
+ [Installing the CloudWatch Logs Agent](#installing_cwl_agent)
+ [Configuring and Starting the CloudWatch Logs Agent](#configure_cwl_agent)
+ [Viewing CloudWatch Logs](#viewing_cwlogs)
+ [Configuring CloudWatch Logs at Launch with User Data](#cwlogs_user_data)

## CloudWatch Logs IAM Policy<a name="cwl_iam_policy"></a>

Before your container instances can send log data to CloudWatch Logs, you must create an IAM policy to allow your container instances to use the CloudWatch Logs APIs, and then you must attach that policy to `ecsInstanceRole`\.

**To create the `ECS-CloudWatchLogs` IAM policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. Choose **Create policy**, **JSON**\.

1. Enter the following policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "logs:DescribeLogStreams"
               ],
               "Resource": [
                   "arn:aws:logs:*:*:*"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, enter `ECS-CloudWatchLogs` for the **Name** and choose **Create policy**\.

**To attach the `ECS-CloudWatchLogs` policy to `ecsInstanceRole`**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Choose `ecsInstanceRole`\. If the role does not exist, follow the procedures in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) to create the role\.

1. Choose **Permissions**, **Attach policy**\.

1. To narrow the available policies to attach, for **Filter**, type **ECS\-CloudWatchLogs**\.

1. Check the box to the left of the **ECS\-CloudWatchLogs** policy and choose **Attach policy**\.

## Installing the CloudWatch Logs Agent<a name="installing_cwl_agent"></a>

After you have added the `ECS-CloudWatchLogs` policy to your `ecsInstanceRole`, you can install the CloudWatch Logs agent on your container instances\.

**Note**  
This procedure was written for the Amazon ECS\-optimized Amazon Linux AMI, and may not work on other operating systems\. For information about installing the agent on other operating systems, see [Getting Started with CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CWL_GettingStarted.html) in the *Amazon CloudWatch User Guide*\.

**To install the CloudWatch Logs agent**
+ Run the following command to install the CloudWatch Logs agent\.

  ```
  [ec2-user ~]$ sudo yum install -y awslogs
  ```

After you have installed the agent, proceed to the next section to configure the agent\.

## Configuring and Starting the CloudWatch Logs Agent<a name="configure_cwl_agent"></a>

The CloudWatch Logs agent configuration file \(`/etc/awslogs/awslogs.conf`\) describes the log files to send to CloudWatch Logs\. The agent configuration file's `[general]` section defines common configurations that apply to all log streams, and you can add individual log stream sections for each file on your container instances that you want to monitor\. For more information, see [CloudWatch Logs Agent Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AgentReference.html) in the *Amazon CloudWatch User Guide*\.

The example configuration file below is configured for the Amazon ECS\-optimized Amazon Linux AMI, and it provides log streams for several common log files:

`/var/log/dmesg`  
The message buffer of the Linux kernel\.

`/var/log/messages`  
Global system messages\.

`/var/log/docker`  
Docker daemon log messages\.

`/var/log/ecs/ecs-init.log`  
Log messages from the `ecs-init` upstart job\.

`/var/log/ecs/ecs-agent.log`  
Log messages from the Amazon ECS container agent\.

`/var/log/ecs/audit.log`  
Log messages from the IAM roles for the task credential provider\.

You can use the example file below for your Amazon ECS container instances, but you must substitute the `{cluster}` and `{container_instance_id}` entries with the cluster name and container instance ID for each container instance so that the log streams are grouped by cluster name and separate for each individual container instance\. The procedure that follows the example configuration file has steps to replace the cluster name and container instance ID placeholders\.

```
[general]
state_file = /var/lib/awslogs/agent-state        
 
[/var/log/dmesg]
file = /var/log/dmesg
log_group_name = /var/log/dmesg
log_stream_name = {cluster}/{container_instance_id}

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %b %d %H:%M:%S

[/var/log/docker]
file = /var/log/docker
log_group_name = /var/log/docker
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%S.%f

[/var/log/ecs/ecs-init.log]
file = /var/log/ecs/ecs-init.log
log_group_name = /var/log/ecs/ecs-init.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/ecs-agent.log]
file = /var/log/ecs/ecs-agent.log.*
log_group_name = /var/log/ecs/ecs-agent.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/audit.log]
file = /var/log/ecs/audit.log.*
log_group_name = /var/log/ecs/audit.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ
```

**To configure the CloudWatch Logs agent**

1. Back up the existing CloudWatch Logs agent configuration file\.

   ```
   [ec2-user ~]$ sudo mv /etc/awslogs/awslogs.conf /etc/awslogs/awslogs.conf.bak
   ```

1. Create a blank configuration file\.

   ```
   [ec2-user ~]$ sudo touch /etc/awslogs/awslogs.conf
   ```

1. Open the `/etc/awslogs/awslogs.conf` file with a text editor, and copy the example file above into it\.

1. Install the jq JSON query utility\.

   ```
   [ec2-user ~]$ sudo yum install -y jq
   ```

1. Query the Amazon ECS introspection API to find the cluster name and set it to an environment variable\.

   ```
   [ec2-user ~]$ cluster=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .Cluster')
   ```

1. Replace the `{cluster}` placeholders in the file with the value of the environment variable you set in the previous step\.

   ```
   [ec2-user ~]$ sudo sed -i -e "s/{cluster}/$cluster/g" /etc/awslogs/awslogs.conf
   ```

1. Query the Amazon ECS introspection API operation to find the container instance ID and set it to an environment variable\.

   ```
   [ec2-user ~]$ container_instance_id=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F/ '{print $2}' )
   ```

1. Replace the `{container_instance_id}` placeholders in the file with the value of the environment variable you set in the previous step\.

   ```
   [ec2-user ~]$ sudo sed -i -e "s/{container_instance_id}/$container_instance_id/g" /etc/awslogs/awslogs.conf
   ```

**To configure the CloudWatch Logs agent Region**

By default, the CloudWatch Logs agent sends data to the `us-east-1` region\. To send your data to a different region, such as the Region in which your cluster is located, you can set the Region in the `/etc/awslogs/awscli.conf` file\.

1. Open the `/etc/awslogs/awscli.conf` file with a text editor\.

1. In the `[default]` section, replace `us-east-1` with the Region from which to view log data\.

1. Save the file and exit your text editor\.

**To start the CloudWatch Logs agent**

1. Start the CloudWatch Logs agent with the following command\.

   ```
   [ec2-user ~]$ sudo service awslogs start
   ```

   Output:

   ```
   Starting awslogs:                                          [  OK  ]
   ```

1. Use the chkconfig command to ensure that the CloudWatch Logs agent starts at every system boot\.

   ```
   [ec2-user ~]$ sudo chkconfig awslogs on
   ```

## Viewing CloudWatch Logs<a name="viewing_cwlogs"></a>

After you have given your container instance role the proper permissions to send logs to CloudWatch Logs, and you have configured and started the agent, your container instance should be sending its log data to CloudWatch Logs\. You can view and search these logs in the AWS Management Console\.

**Note**  
New instance launches may take a few minutes to send data to CloudWatch Logs\.

**To view your CloudWatch Logs data**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Logs**\.

   You should see the log groups you configured in [Configuring and Starting the CloudWatch Logs Agent](#configure_cwl_agent)\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cwl-log-groups.png)

1. Choose a log group to view\.

1. Choose a log stream to view\. The streams are identified by the cluster name and container instance ID that sent the logs\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cw_log_stream.png)

## Configuring CloudWatch Logs at Launch with User Data<a name="cwlogs_user_data"></a>

When you launch an Amazon ECS container instance in Amazon EC2, you have the option of passing user data to the instance that can be used to perform common automated configuration tasks and even run scripts after the instance starts\. You can pass several types of user data to instances, including shell scripts, `cloud-init` directives, and system services\. You can also pass this data into the launch wizard as plaintext, as a file \(this is useful for launching instances via the command line tools\), or as base64\-encoded text \(for API calls\)\.

The example user data block below performs the following tasks:
+ Installs the `awslogs` package, which contains the CloudWatch Logs agent
+ Installs the jq JSON query utility
+ Writes the configuration file for the CloudWatch Logs agent and configures the Region to send data to \(the Region in which the container instance is located\)
+ Gets the cluster name and container instance ID after the Amazon ECS container agent starts and then writes those values to the CloudWatch Logs agent configuration file log streams
+ Starts the CloudWatch Logs agent
+ Configures the CloudWatch Logs agent to start at every system boot

```
Content-Type: multipart/mixed; boundary="==BOUNDARY=="
MIME-Version: 1.0

--==BOUNDARY==
Content-Type: text/x-shellscript; charset="us-ascii"
#!/bin/bash
# Install awslogs and the jq JSON parser
yum install -y awslogs jq

# Inject the CloudWatch Logs configuration file contents
cat > /etc/awslogs/awslogs.conf <<- EOF
[general]
state_file = /var/lib/awslogs/agent-state        
 
[/var/log/dmesg]
file = /var/log/dmesg
log_group_name = /var/log/dmesg
log_stream_name = {cluster}/{container_instance_id}

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %b %d %H:%M:%S

[/var/log/docker]
file = /var/log/docker
log_group_name = /var/log/docker
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%S.%f

[/var/log/ecs/ecs-init.log]
file = /var/log/ecs/ecs-init.log
log_group_name = /var/log/ecs/ecs-init.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/ecs-agent.log]
file = /var/log/ecs/ecs-agent.log.*
log_group_name = /var/log/ecs/ecs-agent.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/audit.log]
file = /var/log/ecs/audit.log.*
log_group_name = /var/log/ecs/audit.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

EOF

--==BOUNDARY==
Content-Type: text/x-shellscript; charset="us-ascii"
#!/bin/bash
# Set the region to send CloudWatch Logs data to (the region where the container instance is located)
region=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r .region)
sed -i -e "s/region = us-east-1/region = $region/g" /etc/awslogs/awscli.conf

--==BOUNDARY==
Content-Type: text/upstart-job; charset="us-ascii"

#upstart-job
description "Configure and start CloudWatch Logs agent on Amazon ECS container instance"
author "Amazon Web Services"
start on started ecs

script
	exec 2>>/var/log/ecs/cloudwatch-logs-start.log
	set -x
	
	until curl -s http://localhost:51678/v1/metadata
	do
		sleep 1	
	done

	# Grab the cluster and container instance ARN from instance metadata
	cluster=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .Cluster')
	container_instance_id=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F/ '{print $2}' )
	
	# Replace the cluster name and container instance ID placeholders with the actual values
	sed -i -e "s/{cluster}/$cluster/g" /etc/awslogs/awslogs.conf
	sed -i -e "s/{container_instance_id}/$container_instance_id/g" /etc/awslogs/awslogs.conf
	
	service awslogs start
	chkconfig awslogs on
end script
--==BOUNDARY==--
```

If you have created the `ECS-CloudWatchLogs` policy and attached it to your `ecsInstanceRole` as described in [CloudWatch Logs IAM Policy](#cwl_iam_policy), then you can add the above user data block to any container instances that you launch manually\. You can also add it to an Auto Scaling launch configuration\. Your container instances that are launched with this user data begin sending their log data to CloudWatch Logs as soon as they launch\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.