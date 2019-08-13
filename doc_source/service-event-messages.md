# Service Event Messages<a name="service-event-messages"></a>

If you are troubleshooting a problem with a service, the first place you should check for diagnostic information is the service event log\.

When viewing service event messages in the Amazon ECS console, duplicate service event messages are omitted until either the cause is resolved or six hours passes\. If the cause is not resolved, you will receive another service event message after six hours has passed\.

**To check the service event log in the Amazon ECS console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster in which your service resides\.

1. On the **Cluster : *clustername*** page, select the service to inspect\.

1. On the **Service : *servicename*** page, choose **Events**\.  
![\[Service event messages\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/events.png)

1. Examine the **Message** column for errors or other helpful information\.

## Service Event Messages<a name="service-event-messages-list"></a>

The following are examples of service event messages you may see in the console:
+ [\(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\.](#service-event-messages-1)
+ [\(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* has insufficient CPU units available\.](#service-event-messages-2)
+ [\(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* encountered error "AGENT"\.](#service-event-messages-3)
+ [\(service *service\-name*\) \(instance *instance\-id*\) is unhealthy in \(elb *elb\-name*\) due to \(reason Instance has failed at least the UnhealthyThreshold number of health checks consecutively\.\)](#service-event-messages-4)
+ [\(service *service\-name*\) is unable to consistently start tasks successfully\.](#service-event-messages-5)

### \(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\.<a name="service-event-messages-1"></a>

In the above image, this service could not find the available resources to add another task\. The possible causes for this are:

No container instances were found in your cluster  
If no container instances are registered in the cluster you attempt to run a task in, you will receive this error\. You should add container instances to your cluster\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

Not enough ports  
If your task uses fixed host port mapping \(for example, your task uses port 80 on the host for a web server\), you must have at least one container instance per task, because only one container can use a single host port at a time\. You should add container instances to your cluster or reduce your number of desired tasks\.

Not enough memory  
If your task definition specifies 1000 MiB of memory, and the container instances in your cluster each have 1024 MiB of memory, you can only run one copy of this task per container instance\. You can experiment with less memory in your task definition so that you could launch more than one task per container instance, or launch more container instances into your cluster\.  
If you are trying to maximize your resource utilization by providing your tasks as much memory as possible for a particular instance type, see [Container Instance Memory Management](memory-management.md)\.

Not enough CPU  
A container instance has 1,024 CPU units for every CPU core\. If your task definition specifies 1,000 CPU units, and the container instances in your cluster each have 1,024 CPU units, you can only run one copy of this task per container instance\. You can experiment with fewer CPU units in your task definition so that you could launch more than one task per container instance, or launch more container instances into your cluster\.

Not enough available ENI attachment points  
Tasks that use the `awsvpc` network mode each receive their own elastic network interface \(ENI\), which is attached to the container instance that hosts it\. Amazon EC2 instances have a limit to the number of ENIs that can be attached to them and there are no container instances in the cluster that have ENI capacity available\.   
The ENI limit for individual container instances depends on the following conditions:  
+ If you **have not** opted in to the `awsvpcTrunking` account setting, the ENI limit for each container instance depends on the instance type\. For more information, see [IP Addresses Per Network Interface Per Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ If you **have** opted in to the `awsvpcTrunking` account setting but you **have not** launched new container instances using a supported instance type after opting in, the ENI limit for each container instance will still be at the default value\. For more information, see [IP Addresses Per Network Interface Per Instance Type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ If you **have** opted in to the `awsvpcTrunking` account setting and you **have** launched new container instances using a supported instance type after opting in, additional ENIs are available\. For more information, see [Supported Amazon EC2 Instance Types](container-instance-eni.md#eni-trunking-supported-instance-types)\.
For more information about opting in to the `awsvpcTrunking` account setting, see [Elastic Network Interface Trunking](container-instance-eni.md)\.  
You can add container instances to your cluster to provide more available network adapters\.

Container instance missing required attribute  
Some task definition parameters require a specific Docker remote API version to be installed on the container instance\. Others, such as the logging driver options, require the container instances to register those log drivers with the `ECS_AVAILABLE_LOGGING_DRIVERS` agent configuration variable\. If your task definition contains a parameter that requires a specific container instance attribute, and you do not have any available container instances that can satisfy this requirement, the task cannot be placed\.  
A common cause of this error is if your service is using tasks that use the `awsvpc` network mode and the EC2 launch type and the cluster you specified does not have a container instance registered to it in the same subnet that was specified in the `awsvpcConfiguration` when the service was created\.  
For more information on which attributes are required for specific task definition parameters and agent configuration variables, see [Task Definition Parameters](task_definition_parameters.md) and [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.  
Windows container instances with Amazon ECS container agent versions earlier than 1\.17\.0 do not support the `awslogs` log driver by default\. If you are unable to use the `awslogs` log driver with your Windows container instances, ensure that you are using the latest Amazon ECS\-optimized Windows AMI\.

### \(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* has insufficient CPU units available\.<a name="service-event-messages-2"></a>

The closest matching container instance for task placement does not container enough CPU units to meet the requirements in the task definition\. Review the CPU requirements in both the task size and container definition parameters of the task definition\.

### \(service *service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* encountered error "AGENT"\.<a name="service-event-messages-3"></a>

The Amazon ECS container agent on the closest matching container instance for task placement is disconnected\. If you can connect to the container instance with SSH, you can examine the agent logs; for more information, see [Amazon ECS Container Agent Log](logs.md#agent-logs)\. You should also verify that the agent is running on the instance\. If you are using the Amazon ECS\-optimized AMI, you can try stopping and restarting the agent with the following command:
+ For the Amazon ECS\-optimized Amazon Linux 2 AMI:

  ```
  sudo systemctl restart ecs
  ```
+ For the Amazon ECS\-optimized Amazon Linux AMI:

  ```
  sudo stop ecs && sudo start ecs
  ```

### \(service *service\-name*\) \(instance *instance\-id*\) is unhealthy in \(elb *elb\-name*\) due to \(reason Instance has failed at least the UnhealthyThreshold number of health checks consecutively\.\)<a name="service-event-messages-4"></a>

This service is registered with a load balancer and the load balancer health checks are failing\. For more information, see [Troubleshooting Service Load Balancers](troubleshoot-service-load-balancers.md)\.

### \(service *service\-name*\) is unable to consistently start tasks successfully\.<a name="service-event-messages-5"></a>

This service contains tasks that have failed to start after consecutive attempts\. At this point, the service scheduler begins to incrementally increase the time between retries\. You should troubleshoot why your tasks are failing to launch\. For more information, see [Service Throttle Logic](service-throttle-logic.md)\.

After the service is updated, for example with an updated task definition, the service scheduler resumes normal behavior\.