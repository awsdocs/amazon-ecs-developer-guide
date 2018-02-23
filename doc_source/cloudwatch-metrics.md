# Amazon ECS CloudWatch Metrics<a name="cloudwatch-metrics"></a>

You can monitor your Amazon ECS resources using Amazon CloudWatch, which collects and processes raw data from Amazon ECS into readable, near real\-time metrics\. These statistics are recorded for a period of two weeks, so that you can access historical information and gain a better perspective on how your clusters or services are performing\. Amazon ECS metric data is automatically sent to CloudWatch in 1\-minute periods\. For more information about CloudWatch, see the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.


+ [Enabling CloudWatch Metrics](#enable_cloudwatch)
+ [Available Metrics and Dimensions](#available_cloudwatch_metrics)
+ [Cluster Reservation](#cluster_reservation)
+ [Cluster Utilization](#cluster_utilization)
+ [Service Utilization](#service_utilization)
+ [Service `RUNNING` Task Count](#cw_running_task_count)
+ [Viewing Amazon ECS Metrics](viewing_cloudwatch_metrics.md)
+ [Tutorial: Scaling Container Instances with CloudWatch Alarms](cloudwatch_alarm_autoscaling.md)

## Enabling CloudWatch Metrics<a name="enable_cloudwatch"></a>

Any task or service using the Fargate launch type will be enabled for CloudWatch metrics automatically, no need for further action\.

For any task or service using the Standard launch type, your Amazon ECS container instances require at least version 1\.4\.0 of the container agent to enable CloudWatch metrics; however, we recommend using the latest container agent version\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

If you are starting your agent manually \(for example, if you are not using the Amazon ECS\-optimized AMI for your container instances\), see [Manually Updating the Amazon ECS Container Agent \(for Non\-Amazon ECS\-optimized AMIs\)](manually_update_agent.md)\.

Your Amazon ECS container instances also require `ecs:StartTelemetrySession` permission on the IAM role that you launch your container instances with\. If you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, then you might need to add this permission\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

**Note**  
You can disable CloudWatch metrics collection by setting `ECS_DISABLE_METRICS=true` in your Amazon ECS container agent configuration\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

## Available Metrics and Dimensions<a name="available_cloudwatch_metrics"></a>

The metrics and dimensions that Amazon ECS sends to Amazon CloudWatch are listed below\.

### Amazon ECS Metrics<a name="ecs-metrics"></a>

Amazon ECS provides metrics for you to monitor the CPU and memory reservation and utilization across your cluster as a whole, and the CPU and memory utilization on the services in your clusters\.

The metrics made available will depend on the launch type of the tasks and services in your clusters\. If you are using the Fargate launch type for your services then CPU and memory utilization metrics are provided to assist in the monitoring of your services\. For the Amazon EC2 launch type you will own and need to monitor the EC2 instances that make your underlying infrastructure so additional CPU and memory reservation and utilization metrics are made available at the cluster, service, and task level\.

Amazon ECS sends the following metrics to CloudWatch every minute\. When Amazon ECS collects metrics, it collects multiple data points every minute\. It then aggregates them to one data point before sending the data to CloudWatch\. So in CloudWatch, one sample count is actually the aggregate of multiple data points during one minute\.

The `AWS/ECS` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `CPUReservation`  |  The percentage of CPU units that are reserved by running tasks in the cluster\. Cluster CPU reservation \(this metric can only be filtered by `ClusterName`\) is measured as the total CPU units that are reserved by Amazon ECS tasks on the cluster, divided by the total CPU units that were registered for all of the container instances in the cluster\. This metric is only used for tasks using the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples\. Unit: Percent  | 
|  `CPUUtilization`  |  The percentage of CPU units that are used in the cluster or service\. Cluster CPU utilization \(metrics that are filtered by `ClusterName` without `ServiceName`\) is measured as the total CPU units in use by Amazon ECS tasks on the cluster, divided by the total CPU units that were registered for all of the container instances in the cluster\. Cluster CPU utilization metrics are only used for tasks using the EC2 launch type\. Service CPU utilization \(metrics that are filtered by `ClusterName` and `ServiceName`\) is measured as the total CPU units in use by the tasks that belong to the service, divided by the total number of CPU units that are reserved for the tasks that belong to the service\. Service CPU utilization metrics are used for tasks using both the Fargate and the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples\. Unit: Percent  | 
|  `MemoryReservation`  |  The percentage of memory that is reserved by running tasks in the cluster\. Cluster memory reservation \(this metric can only be filtered by `ClusterName`\) is measured as the total memory that is reserved by Amazon ECS tasks on the cluster, divided by the total amount of memory that was registered for all of the container instances in the cluster\. This metric is only used for tasks using the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples\. Unit: Percent  | 
|  `MemoryUtilization`  |  The percentage of memory that is used in the cluster or service\.  Cluster memory utilization \(metrics that are filtered by `ClusterName` without `ServiceName`\) is measured as the total memory in use by Amazon ECS tasks on the cluster, divided by the total amount of memory that was registered for all of the container instances in the cluster\. Cluster memory utilization metrics are only used for tasks using the EC2 launch type\. Service memory utilization \(metrics that are filtered by `ClusterName` and `ServiceName`\) is measured as the total memory in use by the tasks that belong to the service, divided by the total memory that is reserved for the tasks that belong to the service\. Service memory utilization metrics are used for tasks using both the Fargate and the EC2 launch type\. Valid Dimensions: `ClusterName`, `ServiceName` Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples\. Unit: Percent  | 

**Note**  
If you are using tasks with the EC2 launch type and have Linux container instances, the Amazon ECS container agent relies on Docker `stats` metrics to gather CPU and memory data for each container running on the instance\. If you are using an Amazon ECS agent prior to version 1\.14\.0, ECS includes filesystem cache usage when reporting memory utilization to CloudWatch so your CloudWatch graphs show a higher than actual memory utilization for tasks\. To remediate this, starting with Amazon ECS agent version 1\.14\.0, the Amazon ECS container agent excludes the filesystem cache usage from the memory utilization metric\. This change does not impact the out\-of\-memory behavior of containers\.

### Dimensions for Amazon ECS Metrics<a name="ecs-metrics-dimensions"></a>

Amazon ECS metrics use the `AWS/ECS` namespace and provide metrics for the following dimensions:


| Dimension | Description | 
| --- | --- | 
|  `ClusterName`  |  This dimension filters the data you request for all resources in a specified cluster\. All Amazon ECS metrics are filtered by `ClusterName`\.  | 
|  `ServiceName`  |  This dimension filters the data you request for all resources in a specified service within a specified cluster\.  | 

## Cluster Reservation<a name="cluster_reservation"></a>

Cluster reservation metrics are measured as the percentage of CPU and memory that is reserved by all Amazon ECS tasks on a cluster when compared to the aggregate CPU and memory that was registered for each active container instance in the cluster\. This metric is only utilized on clusters with tasks or services using the Standard launch type and is not compatible with any using the Fargate launch type\.

```
                                  (Total CPU units reserved by tasks in cluster) x 100
Cluster CPU reservation =  --------------------------------------------------------------
                           (Total CPU units registered by container instances in cluster)
```

```
                                     (Total MiB of memory reserved by tasks in cluster x 100)
Cluster memory reservation =  ------------------------------------------------------------------
                              (Total MiB of memory registered by container instances in cluster)
```

When you run a task in a cluster, Amazon ECS parses its task definition and reserves the aggregate CPU units and MiB of memory that is specified in its container definitions\. Each minute, Amazon ECS calculates the number of CPU units and MiB of memory that are currently reserved for each task that is running in the cluster\. The total amount of CPU and memory reserved for all tasks running on the cluster is calculated, and those numbers are reported to CloudWatch as a percentage of the total registered resources for the cluster\. If you specify a soft limit \(`memoryReservation`\), then it will be used to calculate the amount of reserved memory\. Otherwise, the hard limit \(`memory`\) is used\. For more information about hard and soft limits, see [Task Definition Parameters](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#container_definitions)\.

For example, a cluster has two active container instances registered, a `c4.4xlarge` instance and a `c4.large` instance\. The `c4.4xlarge` instance registers into the cluster with 16,384 CPU units and 30,158 MiB of memory\. The `c4.large` instance registers with 2,048 CPU units and 3,768 MiB of memory\. The aggregate resources of this cluster are 18,432 CPU units and 33,926 MiB of memory\.

If a task definition reserves 1,024 CPU units and 2,048 MiB of memory, and ten tasks are started with this task definition on this cluster \(and no other tasks are currently running\), a total of 10,240 CPU units and 20,480 MiB of memory are reserved, which is reported to CloudWatch as 55% CPU reservation and 60% memory reservation for the cluster\.

The illustration below shows the total registered CPU units in a cluster and what their reservation and utilization means to existing tasks and new task placement\. The lower \(Reserved, utilized\) and center \(Reserved, not utilized\) blocks represent the total CPU units that are reserved for the existing tasks that are running on the cluster, or the `CPUReservation` CloudWatch metric\. The lower block represents the reserved CPU units that the running tasks are actually using on the cluster, or the `CPUUtilization` CloudWatch metric\. The upper block represents CPU units that are not reserved by existing tasks; these CPU units are available for new task placement\. Existing tasks can utilize these unreserved CPU units as well, if their need for CPU resources increases\. For more information, see the [cpu](task_definition_parameters.md#ContainerDefinition-cpu) task definition parameter documentation\.

![\[Cluster CPU reservation and utilization\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/telemetry.png)

## Cluster Utilization<a name="cluster_utilization"></a>

Cluster utilization is measured as the percentage of CPU and memory that is used by all Amazon ECS tasks on a cluster when compared to the aggregate CPU and memory that was registered for each active container instance in the cluster\. This metric is only utilized on clusters with tasks or services using the Standard launch type and is not compatible with any using the Fargate launch type\.

```
                                  (Total CPU units used by tasks in cluster) x 100
Cluster CPU utilization =  --------------------------------------------------------------
                           (Total CPU units registered by container instances in cluster)
```

```
                                     (Total MiB of memory used by tasks in cluster x 100)
Cluster memory utilization =  ------------------------------------------------------------------
                              (Total MiB of memory registered by container instances in cluster)
```

Each minute, the Amazon ECS container agent on each container instance calculates the number of CPU units and MiB of memory that are currently being used for each task that is running on that container instance, and this information is reported back to Amazon ECS\. The total amount of CPU and memory used for all tasks running on the cluster is calculated, and those numbers are reported to CloudWatch as a percentage of the total registered resources for the cluster\.

For example, a cluster has two active container instances registered, a `c4.4xlarge` instance and a `c4.large` instance\. The `c4.4xlarge` instance registers into the cluster with 16,384 CPU units and 30,158 MiB of memory\. The `c4.large` instance registers with 2,048 CPU units and 3,768 MiB of memory\. The aggregate resources of this cluster are 18,432 CPU units and 33,926 MiB of memory\.

If ten tasks are running on this cluster that each consume 1,024 CPU units and 2,048 MiB of memory, a total of 10,240 CPU units and 20,480 MiB of memory are utilized on the cluster, which is reported to CloudWatch as 55% CPU utilization and 60% memory utilization for the cluster\.

## Service Utilization<a name="service_utilization"></a>

Service utilization is measured as the percentage of CPU and memory that is used by the Amazon ECS tasks that belong to a service on a cluster when compared to the CPU and memory that is specified in the service's task definition\. This metric is compatible with services with tasks using both the Standard and Fargate launch types\.

```
                                      (Total CPU units used by tasks in service) x 100
Service CPU utilization =  ----------------------------------------------------------------------------
                           (Total CPU units specified in task definition) x (number of tasks in service)
```

```
                                         (Total MiB of memory used by tasks in service) x 100
Service memory utilization =  --------------------------------------------------------------------------------
                              (Total MiB of memory specified in task definition) x (number of tasks in service)
```

Each minute, the Amazon ECS container agent on each container instance calculates the number of CPU units and MiB of memory that are currently being used for each task owned by the service that is running on that container instance, and this information is reported back to Amazon ECS\. The total amount of CPU and memory used for all tasks owned by the service that are running on the cluster is calculated, and those numbers are reported to CloudWatch as a percentage of the total resources that are specified for the service in the service's task definition\. If you specify a soft limit \(`memoryReservation`\), then it will be used to calculate the amount of reserved memory\. Otherwise, the hard limit \(`memory`\) is used\. For more information about hard and soft limits, see [Task Definition Parameters](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#container_definitions)\.

For example, the task definition for a service specifies a total of 512 CPU units and 1,024 MiB of memory \(with the hard limit `memory` parameter\) for all of its containers\. The service has a desired count of 1 running task, the service is running on a cluster with 1 `c4.large` container instance \(with 2,048 CPU units and 3,768 MiB of total memory\), and there are no other tasks running on the cluster\. Although the task specifies 512 CPU units, because it is the only running task on a container instance with 2,048 CPU units, it has the ability to use up to four times the specified amount \(2,048 / 512\); however, the specified memory of 1,024 MiB is a hard limit and it cannot be exceeded, so in this case, service memory utilization cannot exceed 100%\.

If the previous example used the soft limit `memoryReservation` instead of the hard limit `memory` parameter, the service's tasks could use more than the specified 1,024 MiB of memory if they needed to\. In this case, the service's memory utilization could exceed 100%\.

If this task is performing CPU\-intensive work during a period and using all 2,048 of the available CPU units and 512 MiB of memory, then the service reports 400% CPU utilization and 50% memory utilization\. If the task is idle and using 128 CPU units and 128 MiB of memory, then the service reports 25% CPU utilization and 12\.5% memory utilization\.

## Service `RUNNING` Task Count<a name="cw_running_task_count"></a>

You can use CloudWatch metrics to view the number of tasks in your services that are in the `RUNNING` state\. For example, you can set a CloudWatch alarm for this metric to alert you if the number of running tasks in your service falls below a specified value\. 

**To view the number of running tasks in a service**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics** section on the navigation pane\.

1. On the **All metrics** tab, choose **ECS**\.

1. Choose **ClusterName, ServiceName** and choose any metric \(either `CPUUtilization` or `MemoryUtilization`\) that corresponds to the service to view running tasks in\.

1. On the **Graphed metrics** tab, change the **Period** to **1 Minute** and the **Statistic** to **Sample Count**\.

1. The value displayed in the graph indicates the number of `RUNNING` tasks in the service\.  
![\[Cluster metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/running-task-count.png)