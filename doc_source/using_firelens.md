# Custom log routing<a name="using_firelens"></a>

FireLens for Amazon ECS enables you to use task definition parameters to route logs to an AWS service or AWS Partner Network \(APN\) destination for log storage and analytics\. FireLens works with [Fluentd](https://www.fluentd.org/) and [Fluent Bit](https://fluentbit.io/)\. We provide the AWS for Fluent Bit image or you can use your own Fluentd or Fluent Bit image\.

Creating Amazon ECS task definitions with a FireLens configuration is supported using the AWS SDKs, AWS CLI, and AWS Management Console\.

## Considerations<a name="firelens-considerations"></a>

The following should be considered when using FireLens for Amazon ECS:
+ FireLens for Amazon ECS is supported for tasks hosted on both AWS Fargate and Amazon EC2\.
+ FireLens for Amazon ECS is supported in AWS CloudFormation templates\. For more information, see [AWS::ECS::TaskDefinition FirelensConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ecs-taskdefinition-firelensconfiguration.html) in the *AWS CloudFormation User Guide*
+ FireLens listens on port `24224`, so to ensure that the FireLens log router isn't reachable outside of the task you should not allow ingress traffic on port `24224` in the security group your task uses\. For tasks using the `awsvpc` network mode, this is the security group associated with the task\. For tasks using the `host` network mode, this is the security group associated with the Amazon EC2 instance hosting the task\. For tasks using the `bridge` network mode, don't create any port mappings that use port `24224`\.
+ For tasks that use the `bridge` network mode, the container with the FireLens configuration must start before any application containers that rely on it start\. To control the start order of your containers, use dependency conditions in your task definition\. For more information, see [Container dependency](task_definition_parameters.md#container_definition_dependson)\.
**Note**  
If you use dependency condition parameters in container definitions with a FireLens configuration, ensure that each container has a `START` or `HEALTHY` condition requirement\.
+ The Amazon ECS\-optimized Bottlerocket AMI does not support FireLens\.

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
+ If your task is hosted on Fargate and you are pulling container images from Amazon ECR or referencing sensitive data from AWS Secrets Manager in your log configuration, then you must include the task execution IAM role\.
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

## Using Fluent logger libraries or Log4j over TCP<a name="firelens-fluent-logger"></a>

When the `awsfirelens` log driver is specified in a task definition, the Amazon ECS container agent injects the following environment variables into the container:

`FLUENT_HOST`  
The IP address assigned to the FireLens container\.

`FLUENT_PORT`  
The port that the Fluent Forward protocol is listening on\.

The `FLUENT_HOST` and `FLUENT_PORT` environment variables enable you to log directly to the log router from code instead of going through `stdout`\. For more information, see [fluent\-logger\-golang](https://github.com/fluent/fluent-logger-golang) on GitHub\.

**Topics**
+ [Using the AWS for Fluent Bit image](firelens-using-fluentbit.md)
+ [Creating a task definition that uses a FireLens configuration](firelens-taskdef.md)
+ [Filtering logs using regular expressions](firelens-filtering-logs.md)
+ [Example task definitions](firelens-example-taskdefs.md)