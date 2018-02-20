# Service Auto Scaling<a name="service-auto-scaling"></a>

Your Amazon ECS service can optionally be configured to use Service Auto Scaling to adjust its desired count up or down in response to CloudWatch alarms\. Service Auto Scaling leverages the Application Auto Scaling service to provide this functionality\. Service Auto Scaling is available in all regions that support Amazon ECS\. For more information, see the [Application Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)\.

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. You can use these service utilization metrics to scale your service out to deal with high demand at peak times, and to scale your service in to reduce costs during periods of low utilization\. For more information, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\.

**Note**  
Service Auto Scaling is not currently available in the ECS console in the following regions:  


****  

| Region Name | Region | 
| --- | --- | 
| China \(Beijing\) | cn\-north\-1 | 

Amazon ECS Service Auto Scaling supports the following types of scaling policies:

+ [Target Tracking Scaling Policies](service-autoscaling-targettracking.md)—Increase or decrease  the number of tasks that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.

+ [Step Scaling Policies](service-autoscaling-stepscaling.md)—Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, which vary based on the size of the alarm breach\.