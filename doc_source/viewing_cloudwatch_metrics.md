# Viewing Amazon ECS metrics<a name="viewing_cloudwatch_metrics"></a>

After you have turned on CloudWatch metrics for Amazon ECS, you can view those metrics on the Amazon ECS and CloudWatch consoles\. The Amazon ECS console provides a 24\-hour maximum, minimum, and average view of your cluster and service metrics\. The CloudWatch console provides a fine\-grained and customizable display of your resources, as well as the number of running tasks in a service\.

**Topics**
+ [Viewing cluster metrics using the Amazon ECS console](#viewing_cluster_metrics)
+ [Viewing service metrics using the Amazon ECS console](#viewing_service_metrics)
+ [Viewing Amazon ECS metrics using the CloudWatch console](#viewing_metrics_console)

## Viewing cluster metrics using the Amazon ECS console<a name="viewing_cluster_metrics"></a>

Cluster and service metrics are available on the Amazon ECS console\. The view provided for cluster metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information about cluster metrics, see [Cluster reservation](cloudwatch-metrics.md#cluster_reservation) and [Cluster utilization](cloudwatch-metrics.md#cluster_utilization)\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. Select the cluster that you want to view metrics for\.

1. On the **Cluster: *cluster\-name*** page, choose **Metrics**\.

## Viewing service metrics using the Amazon ECS console<a name="viewing_service_metrics"></a>

Amazon ECS service CPU and memory utilization metrics are available on the Amazon ECS console\. The view provided for service metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information, see [Service utilization](cloudwatch-metrics.md#service_utilization)\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. Select the cluster that you want to view metrics for\.

1. On the **Cluster: *cluster\-name*** page, select the service\.

   The metrics are available under **Health and metrics**\.

## Viewing Amazon ECS metrics using the CloudWatch console<a name="viewing_metrics_console"></a>

Amazon ECS cluster and service metrics can also be viewed on the CloudWatch console\. The console provides the most detailed view of Amazon ECS metrics, and you can tailor the views to suit your needs\. You can view [Cluster reservation](cloudwatch-metrics.md#cluster_reservation), [Cluster utilization](cloudwatch-metrics.md#cluster_utilization), [Service utilization](cloudwatch-metrics.md#service_utilization), and the [Service `RUNNING` task count](cloudwatch-metrics.md#cw_running_task_count)\. For information about how to view the metrics, see [View available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html) the *Amazon CloudWatch User Guide*\.