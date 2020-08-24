# Using the awslogs Log Driver<a name="using_awslogs"></a>

You can configure the containers in your tasks to send log information to CloudWatch Logs\. If you are using the Fargate launch type for your tasks, this allows you to view the logs from your containers\. If you are using the EC2 launch type, this enables you to view different logs from your containers in one convenient location, and it prevents your container logs from taking up disk space on your container instances\. This topic helps you get started using the `awslogs` log driver in your task definitions\.

**Note**  
The type of information that is logged by the containers in your task depends mostly on their `ENTRYPOINT` command\. By default, the logs that are captured show the command output that you would normally see in an interactive terminal if you ran the container locally, which are the `STDOUT` and `STDERR` I/O streams\. The `awslogs` log driver simply passes these logs from Docker to CloudWatch Logs\. For more information on how Docker logs are processed, including alternative ways to capture different file data or streams, see [View logs for a container or service](https://docs.docker.com/config/containers/logging/) in the Docker documentation\.

To send system logs from your Amazon ECS container instances to CloudWatch Logs, see [Using CloudWatch Logs with container instances](using_cloudwatch_logs.md)\. For more information about CloudWatch Logs, see [Monitoring Log Files](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html) and [CloudWatch Logs quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch Logs User Guide*\.

## Enabling the awslogs Log Driver for Your Containers<a name="enable_awslogs"></a>

If you are using the Fargate launch type for your tasks, all you need to do to enable the `awslogs` log driver is add the required `logConfiguration` parameters to your task definition\. For more information, see [Specifying a Log Configuration in your Task Definition](#specify-log-config)\.

If you are using the EC2 launch type for your tasks and want to enable the `awslogs` log driver, your Amazon ECS container instances require at least version 1\.9\.0 of the container agent\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

**Note**  
If you are not using the Amazon ECS\-optimized AMI \(with at least version 1\.9\.0\-1 of the `ecs-init` package\) for your container instances, you also need to specify that the `awslogs` logging driver is available on the container instance when you start the agent by using the following environment variable in your docker run statement or environment variable file\. For more information, see [Installing the Amazon ECS Container Agent](ecs-agent-install.md)\.  

```
ECS_AVAILABLE_LOGGING_DRIVERS='["json-file","awslogs"]'
```

Your Amazon ECS container instances also require `logs:CreateLogStream` and `logs:PutLogEvents` permission on the IAM role with which you launch your container instances\. If you created your Amazon ECS container instance role before `awslogs` log driver support was enabled in Amazon ECS, then you might need to add this permission\. If your container instances use the managed IAM policy for container instances, then your container instances should have the correct permissions\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

## Creating a Log Group<a name="create_awslogs_loggroups"></a>

The `awslogs` log driver can send log streams to an existing log group in CloudWatch Logs or it can create a new log group on your behalf\. The AWS Management Console provides an auto\-configure option which creates a log group on your behalf using the task definition family name with `ecs` as the prefix\. Alternatively, you can manually specify your log configuration options and specify the `awslogs-create-group` option with a value of `true` which will create the log groups on your behalf\.

**Note**  
To use the `awslogs-create-group` option to have your log group created, your IAM policy must include the `logs:CreateLogGroup` permission\.

### Using the Auto\-configuration Feature to Create a Log Group<a name="create_awslogs_loggroups_auto"></a>

When registering a task definition in the Amazon ECS console, you have the option to allow Amazon ECS to auto\-configure your CloudWatch logs\. This option creates a log group on your behalf using the task definition family name with `ecs` as the prefix\.

**To use log group auto\-configuration option in the Amazon ECS console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the left navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. Select your compatibility option and choose **Next Step**\.

1. Choose **Add container**\.

1. In the **Storage and Logging** section, for **Log configuration**, choose **Auto\-configure CloudWatch Logs**\.

1. Enter your awslogs log driver options\. For more information, see [Specifying a Log Configuration in your Task Definition](#specify-log-config)\.

1. Complete the rest of the task definition wizard\.

## Available awslogs Log Driver Options<a name="create_awslogs_logdriver_options"></a>

The `awslogs` log driver supports the following options in Amazon ECS task definitions\. For more information, see [CloudWatch Logs logging driver](https://docs.docker.com/config/containers/logging/awslogs/)\.

`awslogs-create-group`  
Required: No  
Specify whether you want the log group automatically created\. If this option is not specified, it defaults to `false`\.  
Your IAM policy must include the `logs:CreateLogGroup` permission before you attempt to use `awslogs-create-group`\.

`awslogs-region`  
Required: Yes  
Specify the region to which the `awslogs` log driver should send your Docker logs\. You can choose to send all of your logs from clusters in different regions to a single region in CloudWatch Logs so that they are all visible in one location, or you can separate them by region for more granularity\. Be sure that the specified log group exists in the region that you specify with this option\.

`awslogs-endpoint`  
Required: No  
By default, Docker uses either the `awslogs-region` log option or the detected Region to construct the remote CloudWatch Logs API endpoint\. Use the `awslogs-endpoint` log option to override the default endpoint with the provided endpoint\.

`awslogs-group`  
Required: Yes  
You must specify a log group to which the `awslogs` log driver sends its log streams\. For more information, see [Creating a Log Group](#create_awslogs_loggroups)\.

`awslogs-stream-prefix`  
Required: Optional for the EC2 launch type, required for the Fargate launch type\.  
The `awslogs-stream-prefix` option allows you to associate a log stream with the specified prefix, the container name, and the ID of the Amazon ECS task to which the container belongs\. If you specify a prefix with this option, then the log stream takes the following format:  

```
prefix-name/container-name/ecs-task-id
```
If you do not specify a prefix with this option, then the log stream is named after the container ID that is assigned by the Docker daemon on the container instance\. Because it is difficult to trace logs back to the container that sent them with just the Docker container ID \(which is only available on the container instance\), we recommend that you specify a prefix with this option\.  
For Amazon ECS services, you could use the service name as the prefix, which would allow you to trace log streams to the service that the container belongs to, the name of the container that sent them, and the ID of the task to which the container belongs\.  
You must specify a stream\-prefix for your logs in order to have your logs appear in the Log pane when using the Amazon ECS console\.

`awslogs-datetime-format`  
Required: No  
This option defines a multiline start pattern in Python `strftime` format\. A log message consists of a line that matches the pattern and any following lines that don’t match the pattern\. Thus the matched line is the delimiter between log messages\.  
One example of a use case for using this format is for parsing output such as a stack dump, which might otherwise be logged in multiple entries\. The correct pattern allows it to be captured in a single entry\.  
For more information, see [awslogs\-datetime\-format](https://docs.docker.com/config/containers/logging/awslogs/#awslogs-datetime-format)\.  
This option always takes precedence if both `awslogs-datetime-format` and `awslogs-multiline-pattern` are configured\.  
Multiline logging performs regular expression parsing and matching of all log messages, which may have a negative impact on logging performance\.

`awslogs-multiline-pattern`  
Required: No  
This option defines a multiline start pattern using a regular expression\. A log message consists of a line that matches the pattern and any following lines that don’t match the pattern\. Thus the matched line is the delimiter between log messages\.  
For more information, see [awslogs\-multiline\-pattern](https://docs.docker.com/config/containers/logging/awslogs/#awslogs-multiline-pattern)\.  
This option is ignored if `awslogs-datetime-format` is also configured\.  
Multiline logging performs regular expression parsing and matching of all log messages\. This may have a negative impact on logging performance\.

## Specifying a Log Configuration in your Task Definition<a name="specify-log-config"></a>

Before your containers can send logs to CloudWatch, you must specify the `awslogs` log driver for containers in your task definition\. This section describes the log configuration for a container to use the `awslogs` log driver\. For more information, see [Creating a task definition](create-task-definition.md)\.

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

After you have registered a task definition with the `awslogs` log driver in a container definition log configuration, you can run a task or create a service with that task definition to start sending logs to CloudWatch Logs\. For more information, see [Running tasks](ecs_run_task.md) and [Creating a service](create-service.md)\.

## Viewing awslogs Container Logs in CloudWatch Logs<a name="viewing_awslogs"></a>

For tasks using the EC2 launch type, after your container instance role has the proper permissions to send logs to CloudWatch Logs, your container agents are updated to at least version 1\.9\.0, and you have configured and started a task with containers that use the `awslogs` log driver, your configured containers should be sending their log data to CloudWatch Logs\. You can view and search these logs in the console\.

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

1. Select a log group to view\. You should see the log groups that you created in [Creating a Log Group](#create_awslogs_loggroups)\.  
![\[awslogs console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/awslogs-log-groups.png)

1. Choose a log stream to view\.  
![\[awslogs console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/awslogs-log-stream.png)