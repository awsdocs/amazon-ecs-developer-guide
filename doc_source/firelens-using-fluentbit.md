# Using the AWS for Fluent Bit image<a name="firelens-using-fluentbit"></a>

AWS provides a Fluent Bit image with plugins for both CloudWatch Logs and Kinesis Data Firehose\. We recommend using Fluent Bit as your log router because it has a lower resource utilization rate than Fluentd\. For more information, see [CloudWatch Logs for Fluent Bit](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit) and [Amazon Kinesis Firehose for Fluent Bit](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit)\.

The **AWS for Fluent Bit** image is available on Amazon ECR on both the Amazon ECR Public Gallery and in an Amazon ECR repository in most Regions for high availability\.

## Amazon ECR Public Gallery<a name="firelens-image-ecrpublic"></a>

The AWS for Fluent Bit image is available on the Amazon ECR Public Gallery\. This is the recommended location to download the AWS for Fluent Bit image as it is a public repository and available to be used from all AWS Regions\. For more details, see [aws\-for\-fluent\-bit](https://gallery.ecr.aws/aws-observability/aws-for-fluent-bit) on the Amazon ECR Public Gallery\.

You can pull the AWS for Fluent Bit image from the Amazon ECR Public Gallery by specifying the repository URL with the desired image tag\. The available image tags can be found on the **Image tags** tab on the Amazon ECR Public Gallery\.

The following shows the syntax to use for the Docker CLI\.

```
docker pull public.ecr.aws/aws-observability/aws-for-fluent-bit:tag
```

For example, you can pull the latest stable AWS for Fluent Bit image using this Docker CLI command:

```
docker pull public.ecr.aws/aws-observability/aws-for-fluent-bit:stable
```

**Note**  
Unauthenticated pulls are allowed, but have a lower rate limit than authenticated pulls\. To authenticate using your AWS account before pulling, use the following command:  

```
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws
```

## Amazon ECR<a name="firelens-image-ecr"></a>

The AWS for Fluent Bit image is available on Amazon ECR for high availability\. These images are available in most AWS Regions, including AWS GovCloud \(US\)\.

The latest stable AWS for Fluent Bit image URI can be retrieved using the following command\.

```
aws ssm get-parameters \
      --names /aws/service/aws-for-fluent-bit/stable \
      --region us-east-1
```

All versions of the AWS for Fluent Bit image can be listed using the following command to query the Systems Manager Parameter Store parameter\.

```
aws ssm get-parameters-by-path \
      --path /aws/service/aws-for-fluent-bit \
      --region us-east-1
```

The latest stable AWS for Fluent Bit image can be referenced in an AWS CloudFormation template by referencing the Systems Manager parameter store name\. The following is an example:

```
Parameters:
  FireLensImage:
    Description: Fluent Bit image for the FireLens Container
    Type: AWS::SSM::Parameter::Value<String>
    Default: /aws/service/aws-for-fluent-bit/stable
```