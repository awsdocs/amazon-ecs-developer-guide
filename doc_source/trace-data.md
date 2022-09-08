# Collecting application trace data<a name="trace-data"></a>

Amazon ECS integrates with AWS Distro for OpenTelemetry to collect trace data from your application\. Amazon ECS uses an AWS Distro for OpenTelemetry sidecar container to collect and route trace data to AWS X\-Ray\. For more information, see [Setting up AWS Distro for OpenTelemetry Collector in Amazon ECS](https://aws-otel.github.io/docs/setup/ecs)\.

For the AWS Distro for OpenTelemetry Collector to send trace data to AWS X\-Ray, your application must be configured to create the trace data\. For more information, see [Instrumenting your application for AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-instrumenting-your-app.html) in the *AWS X\-Ray Developer Guide*\.

## Required IAM permissions for AWS Distro for OpenTelemetry integration with AWS X\-Ray<a name="trace-data-iam"></a>

The Amazon ECS integration with AWS Distro for OpenTelemetry requires that you create a task IAM role and specify the role in your task definition\. We recommend that the AWS Distro for OpenTelemetry sidecar also be configured to route container logs to CloudWatch Logs which requires a task execution IAM role be created and specified in your task definition as well\. The new Amazon ECS console experience takes care of the task execution IAM role on your behalf, but the task IAM role must be created manually\. For more information about creating a task execution IAM role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
If you're also collecting application metrics using the AWS Distro for OpenTelemetry integration, ensure your task IAM role also contains the permissions necessary for that integration\. For more information, see [Collecting application metrics](metrics-data.md)\.

**To create a task IAM role for AWS Distro for OpenTelemetry integration**

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
                   "xray:PutTraceSegments",
                   "xray:PutTelemetryRecords",
                   "xray:GetSamplingRules",
                   "xray:GetSamplingTargets",
                   "xray:GetSamplingStatisticSummaries"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. \(Optional\) Add one or more tags to the policy, then choose **Next: Review**\.

1. For **Name**, specify `AWSDistroOpenTelemetryPolicyForXray`\.

1. For **Description**, specify an optional description, then choose **Create policy**\.

1. In the navigation pane, choose **Roles**, **Create role**\.

1. In the **Select type of trusted entity** section, choose **AWS service**, **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task**, then choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, search for **AWSDistroOpenTelemetryPolicyForXray**, select the policy, and then choose **Next: Tags**\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy and then choose **Next: Review**\.

1. For **Role name**, specify `AmazonECS_OpenTelemetryXrayRole` and choose **Create role**\.

## Specifying the AWS Distro for OpenTelemetry sidecar for AWS X\-Ray integration in your task definition<a name="trace-data-containerdefinitions"></a>

The new Amazon ECS console experience simplifies the experience of creating the AWS Distro for OpenTelemetry sidecar container by using the **Use trace collection** option\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.

If you're not using the Amazon ECS console, you can add the AWS Distro for OpenTelemetry sidecar container to your task definition\. The following task definition snippet shows the container definition for adding the AWS Distro for OpenTelemetry sidecar for AWS X\-Ray integration\.

```
{
	"family": "otel-using-xray",
	"taskRoleArn": "arn:aws:iam::111122223333:role/AmazonECS_OpenTelemetryXrayRole",
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
				"--config=/etc/ecs/otel-instance-metrics-config.yaml"
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