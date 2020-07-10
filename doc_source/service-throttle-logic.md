# Service throttle logic<a name="service-throttle-logic"></a>

The Amazon ECS service scheduler includes logic that throttles how often service tasks are launched if they repeatedly fail to start\.

If tasks for an ECS service repeatedly fail to enter the `RUNNING` state \(progressing directly from `PENDING` to `STOPPED`\), then the time between subsequent restart attempts is incrementally increased up to a maximum of 15 minutes\. This maximum period is subject to change in the future and should not be considered permanent\. This behavior reduces the effect that unstartable tasks have on your Amazon ECS cluster resources or Fargate infrastructure costs\. If your service triggers the throttle logic, you receive the following [service event message](service-event-messages.md#service-event-messages-5):

```
(service service-name) is unable to consistently start tasks successfully.
```

Amazon ECS does not ever stop a failing service from retrying, nor does it attempt to modify it in any way other than increasing the time between restarts\. The service throttle logic does not provide any user\-tunable parameters\.

If you update your service to use a new task definition, your service returns to a normal, non\-throttled state immediately\. For more information, see [Updating a service](update-service.md)\.

The following are some common causes that trigger this logic:
+ A lack of resources with which to host your task, such as ports, memory, or CPU units in your cluster\. In this case, you also see the [insufficient resource service event message](service-event-messages.md#service-event-messages-1)\.
+ The Amazon ECS container agent is unable to pull your task Docker image\. This could be due to a bad container image name, image, or tag, or a lack of private registry authentication or permissions\. In this case, you also see `CannotPullContainerError` in your [stopped task errors](stopped-task-errors.md)\.
+ Insufficient disk space on your container instance to create the container\. In this case, you also see `CannotCreateContainerError` in your [stopped task errors](stopped-task-errors.md)\. For more information, see [`CannotCreateContainerError: API error (500): devmapper`](CannotCreateContainerError.md)\.

**Important**  
Tasks that are stopped after they reach the `RUNNING` state do not trigger the throttle logic or the associated service event message\. For example, if failed Elastic Load Balancing health checks for a service cause a task to be flagged as unhealthy, and Amazon ECS deregisters it and kills the task, this does not trigger the throttle\. Even if a task's container command immediately exits with a non\-zero exit code, the task has already moved to the `RUNNING` state\. Tasks that fail immediately due to command errors do not trigger the throttle or the service event message\.