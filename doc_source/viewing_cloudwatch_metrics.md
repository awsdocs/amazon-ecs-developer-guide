# Viewing Amazon ECS Metrics<a name="viewing_cloudwatch_metrics"></a>

After you have enabled CloudWatch metrics for Amazon ECS, you can view those metrics in both the Amazon ECS and CloudWatch consoles\. The Amazon ECS console provides a 24\-hour maximum, minimum, and average view of your cluster and service metrics, while the CloudWatch console provides a fine\-grained and customizable display of your resources, as well as the number of running tasks in a service\.

**Topics**
+ [Viewing Cluster Metrics in the Amazon ECS Console](#viewing_cluster_metrics)
+ [Viewing Service Metrics in the Amazon ECS Console](#viewing_service_metrics)
+ [Viewing Amazon ECS Metrics in the CloudWatch Console](#viewing_metrics_console)

## Viewing Cluster Metrics in the Amazon ECS Console<a name="viewing_cluster_metrics"></a>

Cluster and service metrics are available in the Amazon ECS console\. The view provided for cluster metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information about cluster metrics, see [Cluster Reservation](cloudwatch-metrics.md#cluster_reservation) and [Cluster Utilization](cloudwatch-metrics.md#cluster_utilization)\.

**To view cluster metrics in the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster to view metrics with\.

1. On the **Cluster: *cluster\-name*** page, choose the **Metrics** tab to view cluster metrics\.

## Viewing Service Metrics in the Amazon ECS Console<a name="viewing_service_metrics"></a>

Service CPU and memory utilization metrics are available in the Amazon ECS console\. The view provided for service metrics shows the average, minimum, and maximum values for the previous 24\-hour period, with data points available in 5\-minute intervals\. For more information about service utilization metrics, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\.

**To view service metrics in the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose the cluster that contains the service to view metrics with\.

1. On the **Cluster: *cluster\-name*** page, choose the **Services** tab to view the services in that cluster\.

1. Choose the service to view metrics with\.

1. On the **Service: *service\-name*** page, choose the **Metrics** tab to view service metrics\.

## Viewing Amazon ECS Metrics in the CloudWatch Console<a name="viewing_metrics_console"></a>

Amazon ECS cluster and service metrics can also be viewed in the CloudWatch console\. The CloudWatch console provides the most detailed view of Amazon ECS metrics, and you can tailor the views to suit your needs\. You can view [Cluster Reservation](cloudwatch-metrics.md#cluster_reservation), [Cluster Utilization](cloudwatch-metrics.md#cluster_utilization), [Service Utilization](cloudwatch-metrics.md#service_utilization), and the [Service `RUNNING` Task Count](cloudwatch-metrics.md#cw_running_task_count)\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**To view metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the **Metrics** section in the left navigation, choose **ECS**\.

1. Choose the metrics to view\. Cluster metrics are scoped as **ECS > ClusterName** and service utilization metrics are scoped as **ECS > ClusterName, ServiceName**\. The example below shows cluster CPU and memory utilization\.  
![\[CloudWatch console metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cw-console-metrics-view.png)