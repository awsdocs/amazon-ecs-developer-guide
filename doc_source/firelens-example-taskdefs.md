# Example task definitions<a name="firelens-example-taskdefs"></a>

The following are some example task definitions demonstrating common custom log routing options\. For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

**Topics**
+ [Forwarding logs to CloudWatch Logs](#firelens-example-cloudwatch)
+ [Forwarding logs to an Amazon Kinesis Data Firehose delivery stream](#firelens-example-firehose)
+ [Forwarding logs to an Amazon OpenSearch Service domain](#firelens-example-opensearch)
+ [Parsing container logs that are serialized JSON](#firelens-example-parsing)
+ [Forwarding to an external Fluentd or Fluent Bit](#firelens-example-forward)

## Forwarding logs to CloudWatch Logs<a name="firelens-example-cloudwatch"></a>

**Note**  
For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

The following task definition example demonstrates how to specify a log configuration that forwards logs to a CloudWatch Logs log group\. For more information, see [What Is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*\.

In the log configuration options, specify the log group name and the AWS Region it exists in\. To have Fluent Bit create the log group on your behalf, specify `"auto_create_group":"true"`, to set the fluentd\-buffer\-limit use `log-driver-buffer-limit`\. You can also specify the task ID as the log stream prefix, which assists in filtering\. For more information, see [Fluent Bit Plugin for CloudWatch Logs](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit/blob/master/README.md)\.

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
             "image": "httpd",
             "name": "app",
             "logConfiguration": {
                 "logDriver":"awsfirelens",
                 "options": {
                    "Name": "cloudwatch",
                    "region": "us-west-2",
                    "log_group_name": "firelens-blog",
                    "auto_create_group": "true",
                    "log_stream_prefix": "from-fluent-bit",
                    "log-driver-buffer-limit": "2097152" 
                }
            },
            "memoryReservation": 100        
            }
    ]
}
```

## Forwarding logs to an Amazon Kinesis Data Firehose delivery stream<a name="firelens-example-firehose"></a>

**Note**  
For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

The following task definition example demonstrates how to specify a log configuration that forwards logs to an Amazon Kinesis Data Firehose delivery stream\. The Kinesis Data Firehose delivery stream must already exist\. For more information, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

In the log configuration options, specify the delivery stream name and the Region it exists in\. For more information, see [Fluent Bit Plugin for Amazon Kinesis Firehose](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit/blob/master/README.md)\.

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

## Forwarding logs to an Amazon OpenSearch Service domain<a name="firelens-example-opensearch"></a>

**Note**  
For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

The following task definition example demonstrates how to specify a log configuration that forwards logs to an Amazon OpenSearch Service; domain\. The Amazon OpenSearch Service domain must already exist\. For more information, see [What is Amazon OpenSearch Service](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html) in the *Amazon OpenSearch Service Developer Guide*\.

In the log configuration options, specify the log options required for OpenSearch Service integration\. For more information, see [Fluent Bit for Amazon OpenSearch Service](https://docs.fluentbit.io/manual/pipeline/outputs/elasticsearch)\.

```
{
    "family": "firelens-example-opensearch",
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
                "logDriver": "awsfirelens",
                "options": {
                    "Name": "es",
                    "Host": "vpc-fake-domain-ke7thhzo07jawrhmz6mb7ite7y.us-west-2.es.amazonaws.com",
                    "Port": "443",
                    "Index": "my_index",
                    "Type": "my_type",
                    "AWS_Auth": "On",
                    "AWS_Region": "us-west-2",
                    "tls": "On"
                }
            },
            "memoryReservation": 100
        }
    ]
}
```

## Parsing container logs that are serialized JSON<a name="firelens-example-parsing"></a>

**Note**  
For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

Beginning with AWS for Fluent Bit version 1\.3, there's a JSON parser that's included in the AWS for Fluent Bit image\. The following example shows how to reference the JSON parser in the FireLens configuration of your task definition\.

```
"firelensConfiguration": {
    "type": "fluentbit",
    "options": {
        "config-file-type": "file",
        "config-file-value": "/fluent-bit/configs/parse-json.conf"
    }
},
```

The Fluent Bit config file parses any logs that are in JSON \(for example, if the logs at your destination looked like the following without JSON parsing\)\.

```
{
    "source": "stdout",
    "log": "{\"requestID\": \"b5d716fca19a4252ad90e7b8ec7cc8d2\", \"requestInfo\": {\"ipAddress\": \"204.16.5.19\", \"path\": \"/activate\", \"user\": \"TheDoctor\"}}",
    "container_id": "e54cccfac2b87417f71877907f67879068420042828067ae0867e60a63529d35",
    "container_name": "/ecs-demo-6-container2-a4eafbb3d4c7f1e16e00"
    "ecs_cluster": "mycluster",
    "ecs_task_arn": "arn:aws:ecs:us-east-2:01234567891011:task/mycluster/3de392df-6bfa-470b-97ed-aa6f482cd7a6",
    "ecs_task_definition": "demo:7"
    "ec2_instance_id": "i-06bc83dbc2ac2fdf8"
}
```

With the JSON parsing, the log looks like the following\.

```
{
    "source": "stdout",
    "container_id": "e54cccfac2b87417f71877907f67879068420042828067ae0867e60a63529d35",
    "container_name": "/ecs-demo-6-container2-a4eafbb3d4c7f1e16e00"
    "ecs_cluster": "mycluster",
    "ecs_task_arn": "arn:aws:ecs:us-east-2:01234567891011:task/mycluster/3de392df-6bfa-470b-97ed-aa6f482cd7a6",
    "ecs_task_definition": "demo:7"
    "ec2_instance_id": "i-06bc83dbc2ac2fdf8"
    "requestID": "b5d716fca19a4252ad90e7b8ec7cc8d2",
    "requestInfo": {
        "ipAddress": "204.16.5.19",
        "path": "/activate",
        "user": "TheDoctor"
    }
}
```

The serialized JSON is expanded into top level fields in the final JSON output\. For more information about JSON parsing, see [Parser](https://docs.fluentbit.io/manual/pipeline/filters/parser) in the Fluent Bit documentation\.

## Forwarding to an external Fluentd or Fluent Bit<a name="firelens-example-forward"></a>

**Note**  
For more examples, see [Amazon ECS FireLens examples](https://github.com/aws-samples/amazon-ecs-firelens-examples) on GitHub\.

The following task definition example demonstrates how to specify a log configuration that forwards logs to an external Fluentd or Fluent Bit host\. Specify the `host` and `port` for your environment\.

```
{
	"family": "firelens-example-forward",
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