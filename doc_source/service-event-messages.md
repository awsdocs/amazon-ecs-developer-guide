# Service event messages<a name="service-event-messages"></a>

When troubleshooting a problem with a service, the first place you should check for diagnostic information is the service event log\. You can view service events using the `DescribeServices` API, the AWS CLI, or by using the AWS Management Console\.

When viewing service event messages using the Amazon ECS API, only the events from the service scheduler are returned\. These include the most recent task placement and instance health events\. However, the Amazon ECS console displays service events from the following sources\.
+ Task placement and instance health events from the Amazon ECS service scheduler\. These events will have a prefix of **service *\(service\-name\)***\. To ensure that this event view is helpful, we only show the `100` most recent events and duplicate event messages are omitted until either the cause is resolved or six hours passes\. If the cause is not resolved within six hours, you will receive another service event message for that cause\.
+ Service Auto Scaling events\. These events will have a prefix of **Message**\. The `10` most recent scaling events are shown\. These events only occur when a service is configured with an Application Auto Scaling scaling policy\.

Use the following steps to view your current service event messages\.

------
#### [ New console ]

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose the **Services** tab\. 

1. Choose the service to inspect\.

1. In the **Notifications** section, view the messages\.

------
#### [ Classic console ]

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the **Clusters** page, select the cluster where your stopped task resides\.

1. On the **Cluster : *clustername*** page, choose **Tasks**\.

1. In the **Desired task status** table header, choose **Stopped**, and then select the stopped task to inspect\. The most recent stopped tasks are listed first\.

1. In the **Details** section, inspect the **Stopped reason** field to see the reason that the task was stopped\.  
![\[Stopped task reason\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_task_reason.png)

   Some possible reasons and their explanations are listed below:  
Task failed ELB health checks in \(elb elb\-name\)  
The current task failed the Elastic Load Balancing health check for the load balancer that's associated with the task's service\. For more information, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.  
Scaling activity initiated by \(deployment deployment\-id\)  
When you reduce the desired count of a stable service, some tasks must be stopped to reach the desired number\. Tasks that are stopped by downscaling services have this stopped reason\.   
Host EC2 \(instance *id*\) stopped/terminated  
If you stop or terminate a container instance with running tasks, then the tasks are given this stopped reason\.  
Container instance deregistration forced by user  
If you force the deregistration of a container instance with running tasks, then the tasks are given this stopped reason\.  
Essential container in task exited  
If a container marked as `essential` in task definitions exits or dies, that can cause a task to stop\. When an essential container exiting is the cause of a stopped task, the [Step 6](stopped-task-errors.md#status-reason-step) can provide more diagnostic information about why the container stopped\.

1. If you have a container that has stopped, expand the container and inspect the **Status reason** row to see what caused the task state to change\.  
![\[Stopped container error\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/stopped_container_status_reason.png)

   In the previous example, the container image name can't be found\. This can happen if you misspell the image name\.

   If this inspection doesn't provide enough information, you can connect to the container instance with SSH and inspect the Docker container locally\. For more information, see [Inspect Docker Containers](docker-diags.md#docker-inspect)\.

------
#### [ AWS CLI ]

Use the [describe\-services](https://docs.aws.amazon.com/cli/latest/reference/ecs/describe-services.html) command to view the service event messages for a specified service\.

The following AWS CLI example describes the *service\-name* service in the *default* cluster, which will provide the latest service event messages\.

```
aws ecs describe-services \
    --cluster default \
    --services service-name \
    --region us-west-2
```

------

## Service event messages<a name="service-event-messages-list"></a>

The following are examples of service event messages you may see in the Amazon ECS console\.

### service \(*service\-name*\) has reached a steady state\.<a name="service-event-messages-steady"></a>

The service scheduler will send a `service service-name) has reached a steady state.` service event when the service is healthy and at the desired number of tasks, thus reaching a steady state\.

The service scheduler reports the status periodically, so you might receive this message multiple times\.

### service \(*service\-name*\) was unable to place a task because no container instance met all of its requirements\.<a name="service-event-messages-1"></a>

The service scheduler will send this event message when it could not find the available resources to add another task\. The possible causes for this are:

No container instances were found in your cluster  
If no container instances are registered in the cluster you attempt to run a task in, you will receive this error\. You should add container instances to your cluster\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

Not enough ports  
If your task uses fixed host port mapping \(for example, your task uses port 80 on the host for a web server\), you must have at least one container instance per task, because only one container can use a single host port at a time\. You should add container instances to your cluster or reduce your number of desired tasks\.

Too many ports registered  
The closest matching container instance for task placement can not exceed the maximum allowed reserved port limit of 100 host ports per container instance\. Using dynamic host port mapping may remediate the issue\.

Port already in\-use  
The task definition of this task uses the same port in its port mapping as an task already running on the container instance that was chosen to run on\. The service event message would have the chosen container instance ID as part of the message below\.  

```
The closest matching container-instance is already using a port required by your task.
```

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
+ If you **have** opted in to the `awsvpcTrunking` account setting and you **have** launched new container instances using a supported instance type after opting in, additional ENIs are available\. For more information, see [Supported Amazon EC2 instance types](container-instance-eni.md#eni-trunking-supported-instance-types)\.
For more information about opting in to the `awsvpcTrunking` account setting, see [Elastic network interface trunking](container-instance-eni.md)\.  
You can add container instances to your cluster to provide more available network adapters\.

Container instance missing required attribute  
Some task definition parameters require a specific Docker remote API version to be installed on the container instance\. Others, such as the logging driver options, require the container instances to register those log drivers with the `ECS_AVAILABLE_LOGGING_DRIVERS` agent configuration variable\. If your task definition contains a parameter that requires a specific container instance attribute, and you do not have any available container instances that can satisfy this requirement, the task cannot be placed\.  
A common cause of this error is if your service is using tasks that use the `awsvpc` network mode and the EC2 launch type and the cluster you specified does not have a container instance registered to it in the same subnet that was specified in the `awsvpcConfiguration` when the service was created\.  
For more information on which attributes are required for specific task definition parameters and agent configuration variables, see [Task definition parameters](task_definition_parameters.md) and [Amazon ECS container agent configuration](ecs-agent-config.md)\.

### service \(*service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* has insufficient CPU units available\.<a name="service-event-messages-2"></a>

The closest matching container instance for task placement does not container enough CPU units to meet the requirements in the task definition\. Review the CPU requirements in both the task size and container definition parameters of the task definition\.

### service \(*service\-name*\) was unable to place a task because no container instance met all of its requirements\. The closest matching container\-instance *container\-instance\-id* encountered error "AGENT"\.<a name="service-event-messages-3"></a>

The Amazon ECS container agent on the closest matching container instance for task placement is disconnected\. If you can connect to the container instance with SSH, you can examine the agent logs; for more information, see [Amazon ECS Container Agent Log](logs.md#agent-logs)\. You should also verify that the agent is running on the instance\. If you are using the Amazon ECS\-optimized AMI, you can try stopping and restarting the agent with the following command\.
+ For the Amazon ECS\-optimized Amazon Linux 2 AMI

  ```
  sudo systemctl restart ecs
  ```
+ For the Amazon ECS\-optimized Amazon Linux AMI

  ```
  sudo stop ecs && sudo start ecs
  ```

### service \(*service\-name*\) \(instance *instance\-id*\) is unhealthy in \(elb *elb\-name*\) due to \(reason Instance has failed at least the UnhealthyThreshold number of health checks consecutively\.\)<a name="service-event-messages-4"></a>

This service is registered with a load balancer and the load balancer health checks are failing\. For more information, see [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)\.

### service \(*service\-name*\) is unable to consistently start tasks successfully\.<a name="service-event-messages-5"></a>

This service contains tasks that have failed to start after consecutive attempts\. At this point, the service scheduler begins to incrementally increase the time between retries\. You should troubleshoot why your tasks are failing to launch\. For more information, see [Service throttle logic](service-throttle-logic.md)\.

After the service is updated, for example with an updated task definition, the service scheduler resumes normal behavior\.

### service \(*service\-name*\) operations are being throttled\. Will try again later\.<a name="service-event-messages-6"></a>

This service is unable to launch more tasks due to API throttling limits\. Once the service scheduler is able to launch more tasks, it will resume\.

To request an API rate limit quota increase, open the [AWS Support Center](https://console.aws.amazon.com/support/home#/) page, sign in if necessary, and choose **Create case**\. Choose **Service limit increase**\. Complete and submit the form\.

### service \(*service\-name*\) was unable to stop or start tasks during a deployment because of the service deployment configuration\. Update the minimumHealthyPercent or maximumPercent value and try again\.<a name="service-event-messages-7"></a>

This service is unable to stop or start tasks during a service deployment due to the deployment configuration\. The deployment configuration consists of the `minimumHealthyPercent` and `maximumPercent` values which are defined when the service is created, but can also be updated on an existing service\.

The `minimumHealthyPercent` represents the lower limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for the service\. This value is rounded up\. For example if the minimum healthy percent is `50` and the desired task count is four, then the scheduler can stop two existing tasks before starting two new tasks\. Likewise, if the minimum healthy percent is 75% and the desired task count is two, then the scheduler can't stop any tasks due to the resulting value also being two\.

The `maximumPercent` represents the upper limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for a service\. This value is rounded down\. For example if the maximum percent is `200` and the desired task count is four then the scheduler can start four new tasks before stopping four existing tasks\. Likewise, if the maximum percent is `125` and the desired task count is three, the scheduler can't start any tasks due to the resulting value also being three\.

When setting a minimum healthy percent or a maximum percent, you should ensure that the scheduler can stop or start at least one task when a deployment is triggered\.

### service *service\-name*\) was unable to place a task\. Reason: You've reached the limit on the number of tasks you can run concurrently<a name="service-event-messages-8"></a>

You can request a quota increase for the resource that caused the error\. For more information, see [Amazon ECS service quotas](service-quotas.md)\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

### service \(*service\-name*\) was unable to place a task\. Reason: Internal error\.<a name="service-event-messages-9"></a>

The service is unable to start a task due to a subnet being in an unsupported Availability Zone\.

For information about the supported Fargate Regions and Availability Zones, see [Supported Regions for Amazon ECS on AWS Fargate](AWS_Fargate-Regions.md)\.

For information about how to view the subnet Availability Zone, see [View your subnet ](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#view-subnet)in the *Amazon VPC User Guide*\.

### service *service\-name*\) was unable to place a task\. Reason: The requested CPU configuration is above your limit\.<a name="service-event-messages-10"></a>

You can request a quota increase for the resource that caused the error\. For more information, see [Amazon ECS service quotas](service-quotas.md)\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

### service *service\-name*\) was unable to place a task\. Reason: The requested MEMORY configuration is above your limit\.<a name="service-event-messages-11"></a>

You can request a quota increase for the resource that caused the error\. For more information, see [Amazon ECS service quotas](service-quotas.md)\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.