# Service throttle logic<a name="service-throttle-logic"></a>

The Amazon ECS service scheduler includes logic that throttles how often service tasks are launched if they repeatedly fail to start\.

If tasks for a service repeatedly fail to enter the `RUNNING` state \(progressing directly from a `PENDING` to a `STOPPED` status\), then the time between subsequent restart attempts is incrementally increased up to a maximum of 15 minutes\. This maximum period is subject to change in the future\. This behavior reduces the effect that failing tasks have on your Amazon ECS cluster resources or Fargate infrastructure costs\. If your service initiates the throttle logic, you receive the following [service event message](service-event-messages.md#service-event-messages-5):

```
(service service-name) is unable to consistently start tasks successfully.
```

Amazon ECS doesn't ever stop a failing service from retrying\. It also doesn't attempt to modify it in any way other than increasing the time between restarts\. The service throttle logic doesn't provide any user\-tunable parameters\.

If you update your service to use a new task definition, your service returns to a normal, non\-throttled state immediately\. For more information, see [Updating a service using the new console](update-service-console-v2.md)\.

The following are some common causes that initiate this logic:
+ A lack of resources to host your task with, such as ports, memory, or CPU units in your cluster\. In this case, you also see the [insufficient resource service event message](service-event-messages.md#service-event-messages-1)\.
+ The Amazon ECS container agent can't pull your task Docker image\. This might be because a bad container image name, image, or tag, or a lack of private registry authentication or permissions\. In this case, you also see `CannotPullContainerError` in your [stopped task errors](stopped-task-errors.md)\.
+ Insufficient disk space on your container instance to create the container\. In this case, you also see `CannotCreateContainerError` in your [stopped task errors](stopped-task-errors.md)\. For more information, see [`CannotCreateContainerError: API error (500): devmapper`](CannotCreateContainerError.md)\.

**Important**  
Tasks that are stopped after they reach the `RUNNING` state don't start the throttle logic or the associated service event message\. For example, assume that failed Elastic Load Balancing health checks for a service cause a task to be flagged as unhealthy, and Amazon ECS deregisters it and stops the task\. At this point, the tasks aren't throttled\. Even if a task's container command immediately exits with a non\-zero exit code, the task already moved to the `RUNNING` state\. Tasks that fail immediately because command errors don't cause the throttle or the service event message\.