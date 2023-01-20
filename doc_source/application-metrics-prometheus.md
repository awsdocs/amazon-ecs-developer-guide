# Exporting application metrics to Amazon Managed Service for Prometheus<a name="application-metrics-prometheus"></a>

Amazon ECS supports exporting your task\-level CPU, memory, network, and storage metrics and your custom application metrics to Amazon Managed Service for Prometheus\. This is done by adding the AWS Distro for OpenTelemetry sidecar container to your task definition\. The new Amazon ECS console experience simplifies this process by adding the **Use metric collection** option when creating a new task definition\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.

The metrics are exported to Amazon Managed Service for Prometheus and can be viewed using the Amazon Managed Grafana dashboard\. Your application must be instrumented with either Prometheus libraries or with the OpenTelemetry SDK\. For more information about instrumenting your application with the OpenTelemetry SDK, see [Introduction to AWS Distro for OpenTelemetry](https://aws-otel.github.io/docs/introduction) in the AWS Distro for OpenTelemetry documentation\.

When using the Prometheus libraries, your application must expose a `/metrics` endpoint which is used to scrape the metrics data\. For more information about instrumenting your application with Prometheus libraries, see [Prometheus client libraries](https://prometheus.io/docs/instrumenting/clientlibs/) in the Prometheus documentation\.

## Considerations<a name="application-metrics-prometheus-considerations"></a>

The following should be considered when using the Amazon ECS on Fargate integration with AWS Distro for OpenTelemetry to send application metrics to Amazon Managed Service for Prometheus\.
+ The AWS Distro for OpenTelemetry integration is supported for Amazon ECS workloads hosted on Fargate and Amazon ECS workloads hosted on Amazon EC2 instances\. External instances aren't supported currently\.
+ By default, AWS Distro for OpenTelemetry includes all available task\-level dimensions for your application metrics when exporting to Amazon Managed Service for Prometheus\. You can also instrument your application to add additional dimensions\. For more information, see [Getting Started with Prometheus Remote Write Exporter for Amazon Managed Service for Prometheus](https://aws-otel.github.io/docs/getting-started/prometheus-remote-write-exporter) in the AWS Distro for OpenTelemetry documentation\. 

## Required IAM permissions for AWS Distro for OpenTelemetry integration with Amazon Managed Service for Prometheus<a name="application-metrics-prometheus-iam"></a>

The Amazon ECS integration with Amazon Managed Service for Prometheus using the AWS Distro for OpenTelemetry sidecar requires that you create a task IAM role and specify the role in your task definition\. This task IAM role must be created manually using the steps below prior to registering your task definition\.

We recommend that the AWS Distro for OpenTelemetry sidecar also be configured to route container logs to CloudWatch Logs which requires a task execution IAM role be created and specified in your task definition as well\. The new Amazon ECS console experience takes care of the task execution IAM role on your behalf, but the task IAM role must be created manually\. For more information about creating a task execution IAM role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

**Important**  
If you're also collecting application trace data using the AWS Distro for OpenTelemetry integration, ensure your task IAM role also contains the permissions necessary for that integration\. For more information, see [Collecting application trace data](trace-data.md)\.

**To create a task IAM role for AWS Distro for OpenTelemetry integration with Amazon Managed Service for Prometheus**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\.

1. In the **Select type of trusted entity** section, choose **AWS service**, **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service Task**, then choose **Next: Permissions**\.

1. In the **Attach permissions policy** section, search for the **AmazonPrometheusRemoteWriteAccess** policy, select the policy, and then choose **Next: Tags**\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy and then choose **Next: Review**\.

1. For **Role name**, specify `AmazonECS_OpenTelemetryPrometheusRole` and choose **Create role**\.

## Specifying the AWS Distro for OpenTelemetry sidecar in your task definition<a name="application-metrics-prometheus-containerdefinitions"></a>

The new Amazon ECS console experience simplifies the experience of creating the AWS Distro for OpenTelemetry sidecar container by using the **Use metric collection** option\. For more information, see [Creating a task definition using the new console](create-task-definition.md)\.

If you're not using the Amazon ECS console, you can add the AWS Distro for OpenTelemetry sidecar container to your task definition manually\. The following task definition example shows the container definition for adding the AWS Distro for OpenTelemetry sidecar for Amazon Managed Service for Prometheus integration\.

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
					"awslogs-region": "aws-region",
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
			"image": "public.ecr.aws/aws-observability/aws-otel-collector:v0.25.0",
			"essential": true,
			"command": [
				"--config=/etc/ecs/ecs-amp.yaml"
			],
			"environment": [{
				"name": "AWS_PROMETHEUS_ENDPOINT",
				"value": "https://aps-workspaces.aws-region.amazonaws.com/workspaces/ws-a1b2c3d4-5678-90ab-cdef-EXAMPLE11111/api/v1/remote_write"
			}],
			"logConfiguration": {
				"logDriver": "awslogs",
				"options": {
					"awslogs-create-group": "True",
					"awslogs-group": "/ecs/ecs-aws-otel-sidecar-collector",
					"awslogs-region": "aws-region",
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