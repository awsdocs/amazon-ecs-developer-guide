# Exporting application metrics to Amazon CloudWatch<a name="application-metrics-cloudwatch"></a>

****  
The AWS Distro for OpenTelemetry \(ADOT\) metrics collection feature is in preview\. The preview is open to all AWS accounts\. Features may be added or changed before announcing General Availability\.

Amazon ECS on Fargate supports exporting your custom application metrics to Amazon CloudWatch as custom metrics\. This is done by adding the AWS Distro for OpenTelemetry sidecar container to your task definition\. The new Amazon ECS console experience simplifies this process by adding the **Use metric collection** option when creating a new task definition\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.

The application metrics are exported to CloudWatch Logs with log group name `/aws/ecs/application/metrics` and the metrics can be viewed in the `ECS/AWSOTel/Application` namespace\. Your application must be instrumented with the OpenTelemetry SDK\. For more information, see [Introduction to AWS Distro for OpenTelemetry](https://aws-otel.github.io/docs/introduction) in the AWS Distro for OpenTelemetry documentation\.

## Considerations<a name="application-metrics-cloudwatch-considerations"></a>

The following should be considered when using the Amazon ECS on Fargate integration with AWS Distro for OpenTelemetry to send application metrics to Amazon CloudWatch\.
+ When this integration is used, Amazon ECS doesn't send any task\-level metrics to CloudWatch Container Insights\. You can enable Container Insights at the Amazon ECS cluster level to receive those metrics\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ CloudWatch supports a maximum of 30 dimensions per metric\. By default, Amazon ECS defaults to including the `TaskARN`, `ClusterARN`, `LaunchType`, `TaskDefinitionFamily`, and `TaskDefinitionRevision` dimensions to the metrics\. The remaining 25 dimensions can be defined by your application\. If more than 30 dimensions are configured, CloudWatch can't display them\. When this occurs, the application metrics will appear in the `ECS/AWSOTel/Application` CloudWatch metric namespace but without any dimensions\. You can instrument your application to add additional dimensions\. For more information, see [Using CloudWatch metrics with AWS Distro for OpenTelemetry](https://aws-otel.github.io/docs/getting-started/cloudwatch-metrics#cloudwatch-emf-exporter-awsemf) in the AWS Distro for OpenTelemetry documentation\. 

## Required IAM permissions for AWS Distro for OpenTelemetry integration with Amazon CloudWatch<a name="application-metrics-cloudwatch-iam"></a>

The Amazon ECS integration with AWS Distro for OpenTelemetry requires that you create a task IAM role and specify the role in your task definition\. We recommend that the AWS Distro for OpenTelemetry sidecar also be configured to route container logs to CloudWatch Logs which requires a task execution IAM role be created and specified in your task definition as well\. The new Amazon ECS console experience takes care of the task execution IAM role on your behalf, but the task IAM role must be created manually and added to your task definition\. For more information about the task execution IAM role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
If you're also collecting application trace data using the AWS Distro for OpenTelemetry integration, ensure your task IAM role also contains the permissions necessary for that integration\. For more information, see [Collecting application trace data](trace-data.md)\.

**To create a task IAM role for AWS Distro for OpenTelemetry integration with CloudWatch**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, **Create policy**\.

1. On the **Create policy** page, switch to the **JSON** tab, copy and paste the following IAM policy JSON into the field, then choose **Next: Tags**\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:PutLogEvents",
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:DescribeLogStreams",
                   "logs:DescribeLogGroups",
                   "cloudwatch:PutMetricData"
               ],
               "Resource": "*"
           }
       ]
   }
   ```
**Note**  
If your application requires any additional permissions, you should add them to this policy\. Each task definition may only specify one task IAM role\. For example, if you are using a custom configuration file stored in Systems Manager, you should add the `ssm:GetParameters` permission to this IAM policy\.

1. \(Optional\) Add one or more tags to the policy, then choose **Next: Review**\.

1. For **Name**, specify `AWSDistroOpenTelemetryPolicyForCloudWatch`\.

1. For **Description**, specify an optional description, then choose **Create policy**\.

1. In the navigation pane, choose **Roles**, **Create role**\.

1. In the **Select type of trusted entity** section, choose **AWS service**, **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task**, then choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, search for **AWSDistroOpenTelemetryPolicyForCloudWatch**, select the policy, and then choose **Next: Tags**\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy and then choose **Next: Review**\.

1. For **Role name**, specify `AmazonECS_OpenTelemetryCloudWatchRole` and choose **Create role**\.

## Specifying the AWS Distro for OpenTelemetry sidecar in your task definition<a name="application-metrics-cloudwatch-containerdefinitions"></a>

The new Amazon ECS console experience simplifies the experience of creating the AWS Distro for OpenTelemetry sidecar container by using the **Use metric collection** option\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.

If you're not using the Amazon ECS console, you can add the AWS Distro for OpenTelemetry sidecar container to your task definition manually\. The following task definition example shows the container definition for adding the AWS Distro for OpenTelemetry sidecar for Amazon CloudWatch integration\.

```
{
	"family": "otel-using-cloudwatch",
	"taskRoleArn": "arn:aws:iam::111122223333:role/AmazonECS_OpenTelemetryCloudWatchRole",
	"executionRoleArn": "arn:aws:iam::111122223333:role/ecsTaskExecutionRole",
	"containerDefinitions": [{
			"name": "aws-otel-emitter",
			"image": "application-image",
			"logConfiguration": {
				"logDriver": "awslogs",
				"options": {
					"awslogs-create-group": "true",
					"awslogs-group": "/ecs/aws-otel-emitter",
					"awslogs-region": "us-east-1",
					"awslogs-stream-prefix": "ecs"
				}
			},
			"dependsOn": [{
				"containerName": "aws-otel-collector",
				"condition": "START"
			}]
		},
		{
			"name": "aws-otel-collector",
			"image": "public.ecr.aws/aws-observability/aws-otel-collector:v0.17.0",
			"essential": true,
			"command": [
				"--config=/etc/ecs/ecs-cloudwatch.yaml"
			],
			"logConfiguration": {
				"logDriver": "awslogs",
				"options": {
					"awslogs-create-group": "True",
					"awslogs-group": "/ecs/ecs-aws-otel-sidecar-collector",
					"awslogs-region": "us-east-1",
					"awslogs-stream-prefix": "ecs"
				}
			}
		}
	],
	"networkMode": "awsvpc",
	"requiresCompatibilities": [
		"FARGATE"
	],
	"cpu": "1024",
	"memory": "3072"
}
```