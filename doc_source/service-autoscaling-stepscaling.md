# Step scaling policies<a name="service-autoscaling-stepscaling"></a>

Although Amazon ECS service auto scaling supports using Application Auto Scaling step scaling policies, we recommend using target tracking scaling policies instead\. For example, if you want to scale your service when CPU utilization falls below or rises above a certain level, create a target tracking scaling policy based on the CPU utilization metric provided by Amazon ECS\. For more information, see [Target tracking scaling policies](service-autoscaling-targettracking.md)\.

With step scaling policies, you create and manage the CloudWatch alarms that initiate the scaling process\. If the target tracking alarms don't work for your use case, you can use step scaling\. You can also use target tracking scaling with step scaling for an advanced scaling policy configuration\. For example, you can configure a more aggressive response when utilization reaches a certain level\. 

When you create a step scaling policy, you specify one or more step adjustments that automatically scale the number of instances dynamically based on the size of the alarm breach\. Each step adjustment specifies the following: 
+ A lower bound for the metric value 
+ An upper bound for the metric value 
+ The amount by which to scale, based on the scaling adjustment type

CloudWatch aggregates metric data points based on the statistic for the metric that is associated with your CloudWatch alarm\. When the alarm is breached, the appropriate scaling policy is invoked\. Application Auto Scaling applies the aggregation type to the most recent metric data points from CloudWatch \(as opposed to the raw metric data\)\. It compares this aggregated metric value against the upper and lower bounds defined by the step adjustments to determine which step adjustment to perform\. 