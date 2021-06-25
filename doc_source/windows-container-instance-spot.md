# Using Spot Instances<a name="windows-container-instance-spot"></a>

A Spot Instance is an unused Amazon EC2 instance that is available for less than the On\-Demand price\. Because Spot Instances allow you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly\. The hourly price for a Spot Instance is called a Spot price\. The Spot price of each instance type in each Availability Zone is set by Amazon EC2, and adjusted gradually based on the long\-term supply of and demand for Spot Instances\. For more information, see [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-spot-instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

You can register Spot Instances to your Amazon ECS clusters\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

## Spot Instance Draining<a name="windows-spot-instance-draining"></a>

Amazon EC2 terminates, stops, or hibernates your Spot Instance when the Spot price exceeds the maximum price for your request or capacity is no longer available\. Amazon EC2 provides a Spot Instance interruption notice, which gives the instance a two\-minute warning before it is interrupted\. If Amazon ECS Spot Instance draining is enabled on the instance, ECS receives the Spot Instance interruption notice and places the instance in `DRAINING` status\.

**Important**  
Amazon ECS monitors for the Spot Instance interruption notices that have the `terminate` and `stop` instance\-actions\. If you specified either the `hibernate` instance interruption behavior when requesting your Spot Instances or Spot Fleet, then Amazon ECS Spot Instance draining is not supported for those instances\.

When a container instance is set to `DRAINING`, Amazon ECS prevents new tasks from being scheduled for placement on the container instance\. Service tasks on the draining container instance that are in the `PENDING` state are stopped immediately\. If there are container instances in the cluster that are available, replacement service tasks are started on them\.

Spot Instance draining is disabled by default and must be manually enabled\. To enable Spot Instance draining for a new container instance, when launching the container instance add the following script into the **User data** field, replacing *MyCluster* with the name of the cluster to register the container instance to\.

```
<powershell>

Import-Module ECSTools

Initialize-ECSAgent -Cluster your_cluster_name -ECS_ENABLE_SPOT_INSTANCE_DRAINING

</powershell>
```

For more information, see [Launching an Amazon ECS Windows container instance](launch_window-container_instance.md)\.