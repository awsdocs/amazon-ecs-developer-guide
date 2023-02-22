# Custom log routing<a name="using_firelens"></a>

You can use FireLens for Amazon ECS to use task definition parameters to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics\. FireLens works with [Fluentd](https://www.fluentd.org/) and [Fluent Bit](https://fluentbit.io/)\. We provide the AWS for Fluent Bit image or you can use your own Fluentd or Fluent Bit image\.

Creating Amazon ECS task definitions with a FireLens configuration is supported using the AWS SDKs, AWS CLI, and AWS Management Console\.

## Considerations<a name="firelens-considerations"></a>

Consider the following when using FireLens for Amazon ECS:
+ FireLens for Amazon ECS is supported for tasks that are hosted on both AWS Fargate on Linux and Amazon EC2 on Linux\. Windows containers don't support FireLens\.

  For information about how to configure centralized logging for Windows containers, see [Centralized logging for Windows containers on Amazon ECS using Fluent Bit](http://aws.amazon.com/blogs/containers/centralized-logging-for-windows-containers-on-amazon-ecs-using-fluent-bit/)\.
+ FireLens for Amazon ECS is supported in AWS CloudFormation templates\. For more information, see [AWS::ECS::TaskDefinition FirelensConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-taskdefinition-firelensconfiguration.html) in the *AWS CloudFormation User Guide*
+ FireLens listens on port `24224`, so to ensure that the FireLens log router isn't reachable outside of the task, you must not allow inbound traffic on port `24224` in the security group your task uses\. For tasks that use the `awsvpc` network mode, this is the security group associated with the task\. For tasks using the `host` network mode, this is the security group that's associated with the Amazon EC2 instance hosting the task\. For tasks that use the `bridge` network mode, don't create any port mappings that use port `24224`\.
+ For tasks that use the `bridge` network mode, the container with the FireLens configuration must start before any application containers that rely on it start\. To control the start order of your containers, use dependency conditions in your task definition\. For more information, see [Container dependency](task_definition_parameters.md#container_definition_dependson)\.
**Note**  
If you use dependency condition parameters in container definitions with a FireLens configuration, ensure that each container has a `START` or `HEALTHY` condition requirement\.
+ Amazon ECS\-optimized Bottlerocket AMIs prior to the Bottlerocket `v1.13.0` release do not support FireLens\.
+ By default, FireLens adds the cluster and task definition name and the Amazon Resource Name \(ARN\) of the cluster as metadata keys to your stdout/stderr container logs\. The following is an example of the metadata format\.

  ```
  "ecs_cluster": "cluster-name",
  "ecs_task_arn": "arn:aws:ecs:region:111122223333:task/cluster-name/f2ad7dba413f45ddb4EXAMPLE",
  "ecs_task_definition": "task-def-name:revision",
  ```

  If you do not want the metadata in your logs, set `enable-ecs-log-metadata` to `false` in the `firelensConfiguration` section of the task definition\.

  ```
  "firelensConfiguration":{
     "type":"fluentbit",
     "options":{
        "enable-ecs-log-metadata":"false",
        "config-file-type":"file",
        "config-file-value":"/extra.conf"
  }
  ```

## Required IAM permissions<a name="firelens-iam"></a>

To use this feature, you must create an IAM role for your tasks that provides the permissions necessary to use any AWS services that the tasks require\. For example, if a container is routing logs to Kinesis Data Firehose, the task requires permission to call the `firehose:PutRecordBatch` API\. For more information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

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
+ If your task is hosted on Fargate and you are pulling container images from Amazon ECR or referencing sensitive data from AWS Secrets Manager in your log configuration, then you must include the task execution IAM role\.
+ If you are specifying a custom configuration file that's hosted in Amazon S3, your task execution IAM role must include the `s3:GetObject` permission for the configuration file and the `s3:GetBucketLocation` permission on the Amazon S3 bucket that the file is in\. For more information, see [Specifying Permissions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service User Guide*\.

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

## Fluentd buffer limit<a name="firelens-docker-buffer-limit"></a>

When you create a task definition, you can specify the number of events that are buffered in memory by specifying the value \(in bytes\) in the `log-driver-buffer-limit`\. For more information, see [Fluentd logging driver](https://docs.docker.com/config/containers/logging/fluentd/) in the Docker documentation\.

Use this option when there's high throughput, because Docker might run out of buffer memory and discard buffer messages so it can add new messages\. The lost logs might make it difficult to troubleshoot\. Setting the buffer limit might help to prevent this issue\.

The following shows the syntax for specifying the `log-driver-buffer-limit`:

```
{
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
                    "Name": "firehose",
                    "region": "us-west-2",
                    "delivery_stream": "my-stream",
                    "log-driver-buffer-limit": "2097152"
                }
            },
            "dependsOn": [
                {
                    "containerName": "log_router",
                    "condition": "START"
                }
            ],
            "memoryReservation": 100
        }
    ]
}
```

Consider the following when using FireLens for Amazon ECS with the buffer limit option:
+ This option is supported on the Amazon EC2 launch type and the Fargate launch type with platform version `1.4.0` or later\.
+ The option is only valid when `logDriver` is set to `awsfirelens`\.
+ The default buffer limit is `1` MiB\.
+ The valid values are `0` and `536870912` \(512 MiB\)\.
+ The total amount of memory allocated at the task level must be greater than the amount of memory that's allocated for all the containers in addition to the memory buffer limit\. The total amount of buffer memory specified must be less than `536870912` \(512MiB\) when you don't specify the container `memory` and `memoryReservertion` values\. More specifically, you can have an app container with the `awsfirelens` log driver and the `log-driver-buffer-limit` option set to 300 MiB\. However, you won’t be allowed to run tasks if you have more than two containers with the` log-driver-buffer-limit` set to 300 MiB \(300 MiB \* 2 > 512 MiB\)\.

## Using Fluent logger libraries or Log4j over TCP<a name="firelens-fluent-logger"></a>

When the `awsfirelens` log driver is specified in a task definition, the Amazon ECS container agent injects the following environment variables into the container:

`FLUENT_HOST`  
The IP address that's assigned to the FireLens container\.

`FLUENT_PORT`  
The port that the Fluent Forward protocol is listening on\.

You can use the `FLUENT_HOST` and `FLUENT_PORT` environment variables to log directly to the log router from code instead of going through `stdout`\. For more information, see [fluent\-logger\-golang](https://github.com/fluent/fluent-logger-golang) on GitHub\.

**Topics**
+ [Using the AWS for Fluent Bit image](firelens-using-fluentbit.md)
+ [Creating a task definition that uses a FireLens configuration](firelens-taskdef.md)
+ [Filtering logs using regular expressions](firelens-filtering-logs.md)
+ [Example task definitions](firelens-example-taskdefs.md)
