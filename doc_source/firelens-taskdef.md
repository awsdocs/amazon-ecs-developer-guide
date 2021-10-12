# Creating a task definition that uses a FireLens configuration<a name="firelens-taskdef"></a>

To use custom log routing with FireLens you must specify the following in your task definition:
+ A log router container containing a FireLens configuration\. We recommend that the container be marked as `essential`\.
+ One or more application containers that contain a log configuration specifying the `awsfirelens` log driver\.
+ A task IAM role ARN containing the permissions needed for the task to route the logs\.

When creating a new task definition using the AWS Management Console, there is a FireLens integration section that makes it easy to add a log router container\. For more information, see [Creating a task definition](create-task-definition.md)\.

Amazon ECS converts the log configuration and generates the Fluentd or Fluent Bit output configuration\. The output configuration is mounted in the log routing container at `/fluent-bit/etc/fluent-bit.conf` for Fluent Bit and `/fluentd/etc/fluent.conf` for Fluentd\.

**Important**  
FireLens listens on port `24224`, so to ensure that the FireLens log router isn't reachable outside of the task you should not allow ingress traffic on port `24224` in the security group your task uses\. For tasks using the `awsvpc` network mode, this is the security group associated with the task\. For tasks using the `host` network mode, this is the security group associated with the Amazon EC2 instance hosting the task\. For tasks using the `bridge` network mode, don't create any port mappings that use port `24224`\.

The following task definition example defines a log router container that uses Fluent Bit to route its logs to CloudWatch Logs\. It also defines an application container that uses a log configuration to route logs to Amazon Kinesis Data Firehose\.

```
{
	"family": "firelens-example-firehose",
	"taskRoleArn": "arn:aws:iam::123456789012:role/ecs_task_iam_role",
	"containerDefinitions": [
		{
			"essential": true,
			"image": "906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:stable",
			"name": "log_router",
			"firelensConfiguration": {
				"type": "fluentbit"
			},
			"logConfiguration": {
				"logDriver": "awslogs",
				"options": {
					"awslogs-group": "firelens-container",
					"awslogs-region": "us-west-2",
					"awslogs-create-group": "true",
					"awslogs-stream-prefix": "firelens"
				}
			},
			"memoryReservation": 50
		 },
		 {
			 "essential": true,
			 "image": "httpd",
			 "name": "app",
			 "logConfiguration": {
				 "logDriver":"awsfirelens",
				 "options": {
					"Name": "firehose",
					"region": "us-west-2",
					"delivery_stream": "my-stream"
				}
			},
			"memoryReservation": 100		
            }
	]
}
```

The key\-value pairs specified as options in the `logConfiguration` object are used to generate the Fluentd or Fluent Bit output configuration\. The following is a code example from a Fluent Bit output definition\.

```
[OUTPUT]
    Name   firehose
    Match  app-firelens*
    region us-west-2
    delivery_stream my-stream
```

**Note**  
FireLens manages the `match` configuration\. This configuration is not specified in your task definition\. 

## Using Amazon ECS metadata<a name="firelens-taskdef-metadata"></a>

When specifying a FireLens configuration in a task definition, you can optionally toggle the value for `enable-ecs-log-metadata`\. By default, Amazon ECS adds additional fields in your log entries that help identify the source of the logs\. You can disable this action by setting `enable-ecs-log-metadata` to `false`\.
+ `ecs_cluster` – The name of the cluster that the task is part of\.
+ `ecs_task_arn` – The full ARN of the task that the container is part of\.
+ `ecs_task_definition` – The task definition name and revision that the task is using\.
+ `ec2_instance_id` – The Amazon EC2 instance ID that the container is hosted on\. This field is only valid for tasks using the EC2 launch type\.

The following shows the syntax required when specifying an Amazon ECS log metadata setting value:

```
{
   "containerDefinitions":[
      {
         "essential":true,
         "image":"906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:stable",
         "name":"log_router",
         "firelensConfiguration":{
            "type":"fluentbit",
            "options":{
               "enable-ecs-log-metadata":"true | false"
            }
         }
      }
   ]
}
```

## Specifying a custom configuration file<a name="firelens-taskdef-customconfig"></a>

In addition to the auto\-generated configuration file that FireLens creates on your behalf, you can also specify a custom configuration file\. The configuration file format is the native format for the log router you're using\. For more information, see [Fluentd Config File Syntax](https://docs.fluentd.org/configuration/config-file) and [Fluent Bit Configuration Schema](https://fluentbit.io/documentation/0.14/configuration/schema.html)\.

In your custom configuration file, for tasks using the `bridge` or `awsvpc` network mode, you should not set a Fluentd or Fluent Bit forward input over TCP because FireLens will add it to the input configuration\.

Your FireLens configuration must contain the following options to specify a custom configuration file:

`config-file-type`  
The source location of the custom configuration file\. The available options are `s3` or `file`\.  
Tasks hosted on AWS Fargate only support the `file` configuration file type\.

`config-file-value`  
The source for the custom configuration file\. If the `s3` config file type is used, the config file value is the full ARN of the Amazon S3 bucket and file\. If the `file` config file type is used, the config file value is the full path of the configuration file that exists either in the container image or on a volume that is mounted in the container\.  
When using a custom configuration file, you must specify a different path than the one FireLens uses\. Amazon ECS reserves the `/fluent-bit/etc/fluent-bit.conf` filepath for Fluent Bit and `/fluentd/etc/fluent.conf` for Fluentd\.

The following example shows the syntax required when specifying a custom configuration\.

**Important**  
To specify a custom configuration file that is hosted in Amazon S3, ensure you have created a task execution IAM role with the proper permissions\. For more information, see [Required IAM permissions](using_firelens.md#firelens-iam)\.

The following shows the syntax required when specifying a custom configuration:

```
{
   "containerDefinitions":[
      {
         "essential":true,
         "image":"906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:stable",
         "name":"log_router",
         "firelensConfiguration":{
            "type":"fluentbit",
            "options":{
               "config-file-type":"s3 | file",
               "config-file-value":"arn:aws:s3:::mybucket/fluent.conf | filepath"
            }
         }
      }
   ]
}
```

**Note**  
Tasks hosted on AWS Fargate only support the `file` configuration file type\.