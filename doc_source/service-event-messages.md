# Service Event Messages<a name="service-event-messages"></a>

If you are troubleshooting a problem with a service, the first place you should check for diagnostic information is the service event log\.

**To check the service event log in the Amazon ECS console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, choose the cluster in which your service resides\.

1. On the **Cluster : *clustername*** page, choose the service to inspect\.

1. On the **Service : *servicename*** page, choose **Events**\.  
![\[Service event messages\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/events.png)

1. Examine the **Message** column for errors or other helpful information\.

## Service Event Messages<a name="service-event-messages-list"></a>

The following are examples of service event messages you may see in the console:
+ [\(service *service\-name*\) was unable to place a task because the resources could not be found\.](#service-event-messages-1)
+ [\(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* encountered error "AGENT"\.](#service-event-messages-2)
+ [\(service *service\-name*\) \(instance *instance\-id*\) is unhealthy in \(elb *elb\-name*\) due to \(reason Instance has failed at least the UnhealthyThreshold number of health checks consecutively\.\)](#service-event-messages-3)
+ [\(service *service\-name*\) is unable to consistently start tasks successfully\.](#service-event-messages-4)

### \(service *service\-name*\) was unable to place a task because the resources could not be found\.<a name="service-event-messages-1"></a>

In the above image, this service could not find the available resources to add another task\. The possible causes for this are:

Not enough ports  
If your task uses fixed host port mapping \(for example, your task uses port 80 on the host for a web server\), you must have at least one container instance per task, because only one container can use a single host port at a time\. You should add container instances to your cluster or reduce your number of desired tasks\.

Not enough memory  
If your task definition specifies 1000 MiB of memory, and the container instances in your cluster each have 1024 MiB of memory, you can only run one copy of this task per container instance\. You can experiment with less memory in your task definition so that you could launch more than one task per container instance, or launch more container instances into your cluster\.  
If you are trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, see [Container Instance Memory Management](memory-management.md)\.

Not enough CPU  
A container instance has 1,024 CPU units for every CPU core\. If your task definition specifies 1,000 CPU units, and the container instances in your cluster each have 1,024 CPU units, you can only run one copy of this task per container instance\. You can experiment with fewer CPU units in your task definition so that you could launch more than one task per container instance, or launch more container instances into your cluster\.

Not enough available ENI attachment points  
Tasks that use the `awsvpc` network mode each receive their own elastic network interface, which is attached to the container instance that hosts it\. Amazon EC2 instances have a limit to the number of network interfaces that can be attached to them, and the primary network interface counts as one\. For example, a `c4.large` instance may have three network interfaces attached to it\. The primary network adapter for the instance counts as one, so you can attach 2 more ENIs to the instance\. Because each `awsvpc` task requires a network interface, you can only run two such tasks on this instance type\. For more information about how many network interfaces are supported per instance type, see [IP Addresses Per Network Interface Per Instance Type](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the *Amazon EC2 User Guide for Linux Instances*\. You can add container instances to your cluster to provide more available network adapters\.

Container instance missing required attribute  
Some task definition parameters require a specific Docker remote API version to be installed on the container instance\. Others, such as the logging driver options, require the container instances to register those log drivers with the `ECS_AVAILABLE_LOGGING_DRIVERS` agent configuration variable\. If your task definition contains a parameter that requires a specific container instance attribute, and you do not have any available container instances that can satisfy this requirement, the task cannot be placed\. For more information on which attributes are required for specific task definition parameters and agent configuration variables, see [Task Definition Parameters](task_definition_parameters.md) and [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

### \(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* encountered error "AGENT"\.<a name="service-event-messages-2"></a>

The Amazon ECS container agent on the closest matching container instance for task placement is disconnected\. If you can connect to the container instance with SSH, you can examine the agent logs; for more information, see [Amazon ECS Container Agent Log](logs.md#agent-logs)\. You should also verify that the agent is running on the instance\. If you are using the Amazon ECS\-optimized AMI, you can try stopping and restarting the agent with the following command:

```
sudo stop ecs && sudo start ecs
```

### \(service *service\-name*\) \(instance *instance\-id*\) is unhealthy in \(elb *elb\-name*\) due to \(reason Instance has failed at least the UnhealthyThreshold number of health checks consecutively\.\)<a name="service-event-messages-3"></a>

This service is registered with a load balancer and the load balancer health checks are failing\. For more information, see [Troubleshooting Service Load Balancers](troubleshoot-service-load-balancers.md)\.

### \(service *service\-name*\) is unable to consistently start tasks successfully\.<a name="service-event-messages-4"></a>

This service contains tasks that have failed to start after consecutive attempts\. At this point, the service scheduler begins to incrementally increase the time between retries\. You should troubleshoot why your tasks are failing to launch\. For more information, see [Service Throttle Logic](service-throttle-logic.md)\.

After the service is updated, for example with an updated task definition, the service scheduler resumes normal behavior\.