# Collecting application metrics<a name="metrics-data"></a>

****  
The AWS Distro for OpenTelemetry \(ADOT\) metrics collection feature is in preview\. The preview is open to all AWS accounts\. Features may be added or changed before announcing General Availability\.

Amazon ECS on Fargate supports collecting metrics from your applications running on Fargate and exporting them to either Amazon CloudWatch or Amazon Managed Service for Prometheus\. Amazon ECS uses an AWS Distro for OpenTelemetry sidecar container to collect and route your application metrics to the destination\. The new Amazon ECS console experience simplifies the process of adding this integration when creating your task definitions\.

**Topics**
+ [Exporting application metrics to Amazon CloudWatch](application-metrics-cloudwatch.md)
+ [Exporting application metrics to Amazon Managed Service for Prometheus](application-metrics-prometheus.md)