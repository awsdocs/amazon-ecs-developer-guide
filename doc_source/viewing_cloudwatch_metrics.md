# Viewing Amazon ECS metrics<a name="viewing_cloudwatch_metrics"></a>

After you have enabled CloudWatch metrics for Amazon ECS, you can view those metrics on the Amazon ECS and CloudWatch consoles\. The Amazon ECS console provides a 24\-hour maximum, minimum, and average view of your cluster and service metrics\. The CloudWatch console provides a fine\-grained and customizable display of your resources, as well as the number of running tasks in a service\.

**Topics**
+ [Viewing cluster metrics using the Amazon ECS console](#viewing_cluster_metrics)
+ [Viewing service metrics using the Amazon ECS console](#viewing_service_metrics)
+ [Viewing Amazon ECS metrics using the CloudWatch console](#viewing_metrics_console)

## Viewing cluster metrics using the Amazon ECS console<a name="viewing_cluster_metrics"></a>

Cluster and service metrics are available on the Amazon ECS console\. The view provided for cluster metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information about cluster metrics, see [Cluster reservation](cloudwatch-metrics.md#cluster_reservation) and [Cluster utilization](cloudwatch-metrics.md#cluster_utilization)\.

**To view cluster metrics on the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the cluster that you want to view metrics for\.

1. On the **Cluster: *cluster\-name*** page, choose **Metrics**\.

## Viewing service metrics using the Amazon ECS console<a name="viewing_service_metrics"></a>

Amazon ECS service CPU and memory utilization metrics are available on the Amazon ECS console\. The view provided for service metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information, see [Service utilization](cloudwatch-metrics.md#service_utilization)\.

**To view service metrics in the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the cluster that contains the service that you want to view metrics for\.

1. On the **Cluster: *cluster\-name*** page, choose **Services**\.

1. Choose the service that you want to view metrics for\.

1. On the **Service: *service\-name*** page, choose **Metrics**\.

## Viewing Amazon ECS metrics using the CloudWatch console<a name="viewing_metrics_console"></a>

Amazon ECS cluster and service metrics can also be viewed on the CloudWatch console\. The console provides the most detailed view of Amazon ECS metrics, and you can tailor the views to suit your needs\. You can view [Cluster reservation](cloudwatch-metrics.md#cluster_reservation), [Cluster utilization](cloudwatch-metrics.md#cluster_utilization), [Service utilization](cloudwatch-metrics.md#service_utilization), and the [Service `RUNNING` task count](cloudwatch-metrics.md#cw_running_task_count)\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**To view metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the **Metrics** section in the navigation pane, choose **ECS**\.

1. Choose the metrics to view\. Cluster metrics are scoped as **ECS > ClusterName** and service utilization metrics are scoped as **ECS > ClusterName, ServiceName**\. The following example shows cluster CPU and memory utilization\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cw-console-metrics-view.png)