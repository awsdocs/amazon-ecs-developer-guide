# Using the awslogs Log Driver<a name="using_awslogs"></a>

You can configure the containers in your tasks to send log information to CloudWatch Logs\. If you are using the Fargate launch type for your tasks, this allows you to view the logs from your containers\. If you are using the EC2 launch type, this enables you to view different logs from your containers in one convenient location, and it prevents your container logs from taking up disk space on your container instances\. This topic helps you get started using the `awslogs` log driver in your task definitions\.

To send system logs from your Amazon ECS container instances to CloudWatch Logs, see [Using CloudWatch Logs with Container Instances](using_cloudwatch_logs.md)\. For more information about CloudWatch Logs, see [Monitoring Log Files](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch User Guide*\.

**Topics**
+ [Enabling the awslogs Log Driver for Your Containers](#enable_awslogs)
+ [Creating Your Log Groups](#create_awslogs_loggroups)
+ [Available awslogs Log Driver Options](#create_awslogs_logdriver_options)
+ [Specifying a Log Configuration in your Task Definition](#specify-log-config)
+ [Viewing awslogs Container Logs in CloudWatch Logs](#viewing_awslogs)

## Enabling the awslogs Log Driver for Your Containers<a name="enable_awslogs"></a>

If you are using the Fargate launch type for your tasks, all you need to do to enable the `awslogs` log driver is add the required logConfiguration parameters to your task definition\. For more information, see [Specifying a Log Configuration in your Task Definition](#specify-log-config)\.

If you are using the EC2 launch type for your tasks and want to enable the `awslogs` log driver, your Amazon ECS container instances require at least version 1\.9\.0 of the container agent\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

**Note**  
If you are not using the Amazon ECS\-optimized AMI \(with at least version 1\.9\.0\-1 of the `ecs-init` package\) for your container instances, you also need to specify that the `awslogs` logging driver is available on the container instance when you start the agent by using the following environment variable in your docker run statement or environment variable file\. For more information, see [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\.  

```
ECS_AVAILABLE_LOGGING_DRIVERS='["json-file","awslogs"]'
```

Your Amazon ECS container instances also require `logs:CreateLogStream` and `logs:PutLogEvents` permission on the IAM role with which you launch your container instances\. If you created your Amazon ECS container instance role before `awslogs` log driver support was enabled in Amazon ECS, then you might need to add this permission\. If your container instances use the managed IAM policy for container instances, then your container instances should have the correct permissions\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

## Creating Your Log Groups<a name="create_awslogs_loggroups"></a>

The `awslogs` log driver can send log streams to existing log groups in CloudWatch Logs, but it cannot create log groups\. Before you launch any tasks that use the `awslogs` log driver, you should ensure the log groups that you intend your containers to use are created\. The console provides an auto\-configure option so if you register your task definitions in the console and choose the **Auto\-configure CloudWatch Logs** option your log groups will be created for you\. Alternatively, you can manually create your log groups using the following steps\.

As an example, you could have a task with a WordPress container \(which uses the `awslogs-wordpress` log group\) that is linked to a MySQL container \(which uses the `awslogs-mysql` log group\)\. The sections below show how to create these log groups with the AWS CLI and with the CloudWatch console\.

### Creating a Log Group with the AWS CLI<a name="create_awslogs_loggroups_cli"></a>

The AWS Command Line Interface \(AWS CLI\) is a unified tool to manage your AWS services\. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts\. For more information, see the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)\.

If you have a working installation of the AWS CLI, you can use it to create your log groups\. The command below creates a log group called `awslogs-wordpress` in the `us-west-2` region\. Run this command for each log group to create, replacing the log group name with your value and region name to the desired log destination\.

```
aws logs create-log-group --log-group-name awslogs-wordpress --region us-west-2
```

### Using the Auto\-configuration Feature to Create a Log Group<a name="create_awslogs_loggroups_auto"></a>

When registering a task definition in the Amazon ECS console, you have the option to allow Amazon ECS to auto\-configure your CloudWatch logs, which will also create the specified log groups for you\. The auto\-configuration option sets up the CloudWatch logs and log groups with the specified prefix to make it easy\.

**To create a log group in the Amazon ECS console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the left navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. Choose your compatibility option and then **Next Step**\.

1. Choose **Add container** to begin creating your container definition\.

1. In the **Storage and Logging** section, for **Log configuration** choose **Auto\-configure CloudWatch Logs**\.

1. Enter your awslogs log driver options\. For more details, see [Specifying a Log Configuration in your Task Definition](#specify-log-config)\.

1. Complete the rest of the task definition wizard\.

### Creating a Log Group with the CloudWatch Console<a name="create_awslogs_loggroups_console"></a>

The following procedure creates a log group in the CloudWatch console\.

**To create a log group in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Logs**\.

1. Choose **Actions**, **Create log group**\.

1. For **Log Group Name**, enter the name of the log group to create\.

1. Choose **Create log group** to finish\.

## Available awslogs Log Driver Options<a name="create_awslogs_logdriver_options"></a>

The `awslogs` log driver supports the following options in Amazon ECS task definitions\. For more information, see [CloudWatch Logs logging driver](https://docs.docker.com/config/containers/logging/awslogs/)\.

`awslogs-create-group`  
Required: No  
Specify whether you want the log group automatically created\. If this option is not specified, it defaults to `false`\.  
Your IAM policy must include the `logs:CreateLogGroup` permission before you attempt to use `awslogs-create-group`\.

`awslogs-datetime-format`  
Required: No  
This option defines a multiline start pattern in Python `strftime` format\. A log message consists of a line that matches the pattern and any following lines that don’t match the pattern\. Thus the matched line is the delimiter between log messages\.  
One example of a use case for using this format is for parsing output such as a stack dump, which might otherwise be logged in multiple entries\. The correct pattern allows it to be captured in a single entry\.  
This option always takes precedence if both awslogs\-datetime\-format and awslogs\-multiline\-pattern are configured\.  
Multiline logging performs regular expression parsing and matching of all log messages, which may have a negative impact on logging performance\.

`awslogs-region`  
Required: Yes  
Specify the region to which the `awslogs` log driver should send your Docker logs\. You can choose to send all of your logs from clusters in different regions to a single region in CloudWatch Logs so that they are all visible in one location, or you can separate them by region for more granularity\. Be sure that the specified log group exists in the region that you specify with this option\.

`awslogs-group`  
Required: Yes  
You must specify a log group to which the `awslogs` log driver will send its log streams\. For more information, see [Creating Your Log Groups](#create_awslogs_loggroups)\.

`awslogs-multiline-pattern`  
Required: No  
This option defines a multiline start pattern using a regular expression\. A log message consists of a line that matches the pattern and any following lines that don’t match the pattern\. Thus the matched line is the delimiter between log messages\.  
This option is ignored if `awslogs-datetime-format` is also configured\.  
Multiline logging performs regular expression parsing and matching of all log messages\. This may have a negative impact on logging performance\.

`awslogs-stream-prefix`  
Required: Optional for EC2 launch type, required for Fargate launch type\.  
The `awslogs-stream-prefix` option allows you to associate a log stream with the specified prefix, the container name, and the ID of the Amazon ECS task to which the container belongs\. If you specify a prefix with this option, then the log stream takes the following format:  

```
prefix-name/container-name/ecs-task-id
```
If you do not specify a prefix with this option, then the log stream is named after the container ID that is assigned by the Docker daemon on the container instance\. Because it is difficult to trace logs back to the container that sent them with just the Docker container ID \(which is only available on the container instance\), we recommend that you specify a prefix with this option\.  
For Amazon ECS services, you could use the service name as the prefix, which would allow you to trace log streams to the service that the container belongs to, the name of the container that sent them, and the ID of the task to which the container belongs\.  
You must specify a stream\-prefix for your logs in order to have your logs appear in the Log pane when using the Amazon ECS console\.

## Specifying a Log Configuration in your Task Definition<a name="specify-log-config"></a>

Before your containers can send logs to CloudWatch, you must specify the `awslogs` log driver for containers in your task definition\. This section describes the log configuration for a container to use the `awslogs` log driver\. For more information, see [Creating a Task Definition](create-task-definition.md)\.

The task definition JSON shown below has a `logConfiguration` object specified for each container; one for the WordPress container that sends logs to a log group called `awslogs-wordpress`, and one for a MySQL container that sends logs to a log group called `awslogs-mysql`\. Both containers use the `awslogs-example` log stream prefix\.

```
{
    "containerDefinitions": [
        {
            "name": "wordpress",
            "links": [
                "mysql"
            ],
            "image": "wordpress",
            "essential": true,
            "portMappings": [
                {
                    "containerPort": 80,
                    "hostPort": 80
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "awslogs-wordpress",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "awslogs-example"
                }
            },
            "memory": 500,
            "cpu": 10
        },
        {
            "environment": [
                {
                    "name": "MYSQL_ROOT_PASSWORD",
                    "value": "password"
                }
            ],
            "name": "mysql",
            "image": "mysql",
            "cpu": 10,
            "memory": 500,
            "essential": true,
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "awslogs-mysql",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "awslogs-example"
                }
            }
        }
    ],
    "family": "awslogs-example"
}
```

In the Amazon ECS console, the log configuration for the `wordpress` container is specified as shown in the image below\. 

![\[Console log configuration\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/awslogs-console-config.png)

After you have registered a task definition with the `awslogs` log driver in a container definition log configuration, you can run a task or create a service with that task definition to start sending logs to CloudWatch Logs\. For more information, see [Running Tasks](ecs_run_task.md) and [Creating a Service](create-service.md)\.

## Viewing awslogs Container Logs in CloudWatch Logs<a name="viewing_awslogs"></a>

After your container instance role has the proper permissions to send logs to CloudWatch Logs, your container agents are updated to at least version 1\.9\.0, and you have configured and started a task with containers that use the `awslogs` log driver, your configured containers should be sending their log data to CloudWatch Logs\. You can view and search these logs in the console\.

**To view your CloudWatch Logs data for a container from the Amazon ECS console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster that contains the task to view\.

1. On the **Cluster: *cluster\_name*** page, choose **Tasks** and select the task to view\.

1. On the **Task: *task\_id*** page, expand the container view by choosing the arrow to the left of the container name\.

1. In the **Log Configuration** section, choose **View logs in CloudWatch**, which opens the associated log stream in the CloudWatch console\.  
![\[Task definition view of log configuration\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/view_logs_in_cw.png)

**To view your CloudWatch Logs data in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Logs**\.

1. Select a log group to view\. You should see the log groups that you created in [Creating Your Log Groups](#create_awslogs_loggroups)\.  
![\[awslogs console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/awslogs-log-groups.png)

1. Choose a log stream to view\.  
![\[awslogs console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/awslogs-log-stream.png)