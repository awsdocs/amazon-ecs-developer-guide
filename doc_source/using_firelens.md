# Custom log routing<a name="using_firelens"></a>

FireLens for Amazon ECS enables you to use task definition parameters to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics\. FireLens works with [Fluentd](https://www.fluentd.org/) and [Fluent Bit](https://fluentbit.io/)\. We provide the AWS for Fluent Bit image or you can use your own Fluentd or Fluent Bit image\.

Creating Amazon ECS task definitions with a FireLens configuration is supported using the AWS SDKs, AWS CLI, and AWS Management Console\.

## Considerations<a name="firelens-considerations"></a>

The following should be considered when using FireLens for Amazon ECS:
+ FireLens for Amazon ECS is supported for tasks using both the Fargate and EC2 launch types\.
+ FireLens for Amazon ECS is supported in AWS CloudFormation templates\. For more information, see [AWS::ECS::TaskDefinition FirelensConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-taskdefinition-firelensconfiguration.html) in the *AWS CloudFormation User Guide*
+ For tasks that use the `bridge` network mode, the container with the FireLens configuration must start before any application containers that rely on it start\. To control the start order of your containers, use dependency conditions in your task definition\. For more information, see [Container Dependency](task_definition_parameters.md#container_definition_dependson)\.
**Note**  
If you use dependency condition parameters in container definitions with a FireLens configuration, ensure that each container has a `START` or `HEALTHY` condition requirement\.

## Required IAM permissions<a name="firelens-iam"></a>

To use this feature, you must create an IAM role for your tasks that provides the permissions necessary to use any AWS services that the tasks require\. For example, if a container is routing logs to Kinesis Data Firehose, then the task would require permission to call the `firehose:PutRecordBatch` API\. For more information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

The following example IAM policy adds the required permissions for routing logs to Kinesis Data Firehose\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "firehose:PutRecordBatch"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

Your task may also require the Amazon ECS task execution role under the following conditions\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.
+ If your task uses the Fargate launch type and you are pulling container images from Amazon ECR or referencing sensitive data from AWS Secrets Manager in your log configuration, then you must include the task execution IAM role\.
+ If you are specifying a custom configuration file that is hosted in Amazon S3, your task execution IAM role must include the `s3:GetObject` permission for the configuration file and the `s3:GetBucketLocation` permission on the Amazon S3 bucket that the file is in\. For more information, see [Specifying Permissions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service Console User Guide*\.

  The following example IAM policy adds the required permissions for retrieving a file from Amazon S3\. Specify the name of your Amazon S3 bucket and configuration file name\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "s3:GetObject"
        ],
        "Resource": [
          "arn:aws:s3:::examplebucket/folder_name/config_file_name"
        ]
      },
      {
        "Effect": "Allow",
        "Action": [
          "s3:GetBucketLocation"
        ],
        "Resource": [
          "arn:aws:s3:::examplebucket"
        ]
      }
    ]
  }
  ```

## Using the AWS for Fluent Bit image<a name="firelens-using-fluentbit"></a>

AWS provides a Fluent Bit image with plugins for both CloudWatch Logs and Kinesis Data Firehose\. We recommend using Fluent Bit as your log router because it has a lower resource utilization rate than Fluentd\. For more information, see [CloudWatch Logs for Fluent Bit](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit) and [Amazon Kinesis Firehose for Fluent Bit](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit)\.

The **AWS for Fluent Bit** image is available on [Docker Hub](https://hub.docker.com/r/amazon/aws-for-fluent-bit)\. However, we recommend that you use the following images in Amazon ECR because they provide higher availability\.


| Region Name | Region | Image URI | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  us\-east\-1  |  `906394416424.dkr.ecr.us-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US East \(Ohio\)  |  us\-east\-2  |  `906394416424.dkr.ecr.us-east-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US West \(N\. California\)  |  us\-west\-1  |  `906394416424.dkr.ecr.us-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  US West \(Oregon\)  |  us\-west\-2  |  `906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Hong Kong\)  |  ap\-east\-1  |  `449074385750.dkr.ecr.ap-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  |  `906394416424.dkr.ecr.ap-south-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  |  `906394416424.dkr.ecr.ap-northeast-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  |  `906394416424.dkr.ecr.ap-southeast-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  |  `906394416424.dkr.ecr.ap-southeast-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  |  `906394416424.dkr.ecr.ap-northeast-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Canada \(Central\)  |  ca\-central\-1  |  `906394416424.dkr.ecr.ca-central-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Frankfurt\)  |  eu\-central\-1  |  `906394416424.dkr.ecr.eu-central-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Ireland\)  |  eu\-west\-1  |  `906394416424.dkr.ecr.eu-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(London\)  |  eu\-west\-2  |  `906394416424.dkr.ecr.eu-west-2.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Paris\)  |  eu\-west\-3  |  `906394416424.dkr.ecr.eu-west-3.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  |  `906394416424.dkr.ecr.eu-north-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  Middle East \(Bahrain\)  |  me\-south\-1  |  `741863432321.dkr.ecr.me-south-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  South America \(São Paulo\)  |  sa\-east\-1  |  `906394416424.dkr.ecr.sa-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  AWS GovCloud \(US\-East\)  |  us\-gov\-east\-1  |  `161423150738.dkr.ecr.us-gov-east-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  AWS GovCloud \(US\-West\)  |  us\-gov\-west\-1  |  `161423150738.dkr.ecr.us-gov-west-1.amazonaws.com/aws-for-fluent-bit:latest`  | 
|  China \(Beijing\)  |  cn\-north\-1  |  `128054284489.dkr.ecr.cn-north-1.amazonaws.com.cn/aws-for-fluent-bit:latest`  | 
|  China \(Ningxia\)  |  cn\-northwest\-1  |  `128054284489.dkr.ecr.cn-northwest-1.amazonaws.com.cn/aws-for-fluent-bit:latest`  | 

## Creating a task definition that uses a FireLens configuration<a name="firelens-taskdef"></a>

To use custom log routing with FireLens you must specify the following in your task definition:
+ A log router container containing a FireLens configuration\. We recommend that the container be marked as `essential`\.
+ One or more application containers that contain a log configuration specifying the `awsfirelens` log driver\.
+ A task IAM role ARN containing the permissions needed for the task to route the logs\.

When creating a new task definition using the AWS Management Console, there is a FireLens integration section that makes it easy to add a log router container\. For more information, see [Creating a task definition](create-task-definition.md)\.

Amazon ECS converts the log configuration and generates the Fluentd or Fluent Bit output configuration\. The output configuration is mounted in the log routing container at `/fluent-bit/etc/fluent-bit.conf` for Fluent Bit and `/fluentd/etc/fluent.conf` for Fluentd\.

The following task definition example defines a log router container that uses Fluent Bit to route its logs to CloudWatch Logs\. It also defines an application container that uses a log configuration to route logs to Amazon Kinesis Data Firehose\.

```
{
	"family": "firelens-example-firehose",
	"taskRoleArn": "arn:aws:iam::123456789012:role/ecs_task_iam_role",
	"containerDefinitions": [
		{
			"essential": true,
			"image": "906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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

### Using Amazon ECS metadata<a name="firelens-taskdef-metadata"></a>

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
         "image":"906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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

### Specifying a custom configuration file<a name="firelens-taskdef-customconfig"></a>

In addition to the auto\-generated configuration file that FireLens creates on your behalf, you can also specify a custom configuration file\. The configuration file format is the native format for the log router you're using\. For more information, see [Fluentd Config File Syntax](https://docs.fluentd.org/configuration/config-file) and [Fluent Bit Configuration Schema](https://fluentbit.io/documentation/0.14/configuration/schema.html)\.

In your custom configuration file, for tasks using the `bridge` or `awsvpc` network mode, you should not set a Fluentd or Fluent Bit forward input over TCP because FireLens will add it to the input configuration\.

Your FireLens configuration must contain the following options to specify a custom configuration file:

`config-file-type`  
The source location of the custom configuration file\. The available options are `s3` or `file`\.

`config-file-value`  
The source for the custom configuration file\. If the `s3` config file type is used, the config file value is the full ARN of the Amazon S3 bucket and file\. If the `file` config file type is used, the config file value is the full path of the configuration file that exists either in the container image or on a volume that is mounted in the container\.  
When using a custom configuration file, you must specify a different path than the one FireLens uses\. Amazon ECS reserves the `/fluent-bit/etc/fluent-bit.conf` filepath for Fluent Bit and `/fluentd/etc/fluent.conf` for Fluentd\.

The following example shows the syntax required when specifying a custom configuration\.

**Important**  
To specify a custom configuration file that is hosted in Amazon S3, ensure you have created a task execution IAM role with the proper permissions\. For more information, see [Required IAM permissions](#firelens-iam)\.

The following shows the syntax required when specifying a custom configuration:

```
{
   "containerDefinitions":[
      {
         "essential":true,
         "image":"906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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
For tasks using the Fargate launch type, the only supported `config-file-type` value is `file`\.

## Using Fluent logger libraries<a name="firelens-fluent-logger"></a>

When the `awsfirelens` log driver is specified in a task definition, the ECS agent injects the following environment variables into the container:

`FLUENT_HOST`  
The IP address assigned to the FireLens container\.

`FLUENT_PORT`  
The port that the Fluent Forward protocol is listening on\.

The `FLUENT_HOST` and `FLUENT_PORT` environment variables enable you to log directly to the log router from code instead of going through `stdout`\. For more information, see [fluent\-logger\-golang](https://github.com/fluent/fluent-logger-golang) on GitHub\.

## Filtering logs using regular expressions<a name="firelens-filtering-logs"></a>

Fluentd and Fluent Bit both support filtering of logs based on their content\. FireLens provides a simple method for enabling this filtering\. In the log configuration `options` in a container definition, you can specify the special keys `exclude-pattern` and `include-pattern` that take regular expressions as their values\. The `exclude-pattern` key causes all logs that match its regular expression to be dropped\. With `include-pattern`, only logs that match its regular expression are sent\. These keys can be used together\.

The following example demonstrates how to use this filter\.

```
{
   "containerDefinitions":[
      {
         "logConfiguration":{
            "logDriver":"awsfirelens",
            "options":{
               "@type":"cloudwatch_logs",
               "log_group_name":"firelens-testing",
               "auto_create_stream":"true",
               "use_tag_as_stream":"true",
               "region":"us-west-2",
               "exclude-pattern":"^[a-z][aeiou].*$",
               "include-pattern":"^.*[aeiou]$"
            }
         }
      }
   ]
}
```

## Example task definitions<a name="firelens-example-taskdefs"></a>

The following are some example task definitions demonstrating common log routing options\. For more examples, see [Amazon ECS FireLens Examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

**Topics**
+ [Forwarding Logs to CloudWatch Logs](#firelens-example-cloudwatch)
+ [Forwarding logs to an Amazon Kinesis Data Firehose delivery stream](#firelens-example-firehose)
+ [Forwarding to an external Fluentd or Fluent Bit](#firelens-example-forward)

### Forwarding Logs to CloudWatch Logs<a name="firelens-example-cloudwatch"></a>

The following task definition example demonstrates how to specify a log configuration that forwards logs to a CloudWatch Logs log group\. For more information, see [What Is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.

In the log configuration options, specify the log group name and the Region it exists in\. To have Fluent Bit create the log group on your behalf, specify `"auto_create_group":"true"`\. You can also specify a log stream prefix, which assists in filtering\. For more information, see [Fluent Bit Plugin for CloudWatch Logs](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit/blob/master/README.md)\.

```
{
	"family": "firelens-example-cloudwatch",
	"taskRoleArn": "arn:aws:iam::123456789012:role/ecs_task_iam_role",
	"containerDefinitions": [
		{
			"essential": true,
			"image": "906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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
			 "image": "nginx",
			 "name": "app",
			 "logConfiguration": {
				 "logDriver":"awsfirelens",
				 "options": {
					"Name": "cloudwatch",
					"region": "us-west-2",
					"log_group_name": "firelens-testing-fluent-bit",
					"auto_create_group": "true",
					"log_stream_prefix": "from-fluent-bit"
				}
			},
			"memoryReservation": 100
		}
	]
}
```

### Forwarding logs to an Amazon Kinesis Data Firehose delivery stream<a name="firelens-example-firehose"></a>

The following task definition example demonstrates how to specify a log configuration that forwards logs to an Amazon Kinesis Data Firehose delivery stream\. The Kinesis Data Firehose delivery stream must already exist\. For more information, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

In the log configuration options, specify the delivery stream name and the Region it exists in\. For more information, see [Fluent Bit Plugin for Amazon Kinesis Firehose](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit/blob/master/README.md)\.

```
{
	"family": "firelens-example-firehose",
	"taskRoleArn": "arn:aws:iam::123456789012:role/ecs_task_iam_role",
	"containerDefinitions": [
		{
			"essential": true,
			"image": "906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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

### Forwarding to an external Fluentd or Fluent Bit<a name="firelens-example-forward"></a>

The following task definition example demonstrates how to specify a log configuration that forwards logs to an external Fluentd or Fluent Bit host\. Specify the `host` and `port` for your environment\.

```
{
	"family": "firelens-example-forward",
	"taskRoleArn": "arn:aws:iam::123456789012:role/ecs_task_iam_role",
	"containerDefinitions": [
		{
			"essential": true,
			"image": "906394416424.dkr.ecr.us-west-2.amazonaws.com/aws-for-fluent-bit:latest",
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
					"Name": "forward",
					"Host": "fluentdhost",
					"Port": "24224"
				}
			},
			"memoryReservation": 100
		}
	]
}
```