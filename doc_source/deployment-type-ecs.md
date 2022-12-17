# Rolling update<a name="deployment-type-ecs"></a>

When you start a service which uses the *rolling update* \(`ECS`\) deployment type, the Amazon ECS service scheduler replaces the currently running tasks with new tasks\. The number of tasks that Amazon ECS adds or removes from the service during a rolling update is controlled by the deployment configuration\. The deployment configuration consists of the `minimumHealthyPercent` and `maximumPercent` values which are defined when the service is created, but can also be updated on an existing service\.

The `minimumHealthyPercent` represents the lower limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for the service\. This value is rounded up\. For example if the minimum healthy percent is `50` and the desired task count is four, then the scheduler can stop two existing tasks before starting two new tasks\. Likewise, if the minimum healthy percent is 75% and the desired task count is two, then the scheduler can't stop any tasks due to the resulting value also being two\.

The `maximumPercent` represents the upper limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for a service\. This value is rounded down\. For example if the maximum percent is `200` and the desired task count is four then the scheduler can start four new tasks before stopping four existing tasks\. Likewise, if the maximum percent is `125` and the desired task count is three, the scheduler can't start any tasks due to the resulting value also being three\.

**Important**  
When setting a minimum healthy percent or a maximum percent, you should ensure that the scheduler can stop or start at least one task when a deployment is initiated\. If your service has a deployment that is stuck due to an invalid deployment configuration, a service event message will be sent\. For more information, see [service \(*service\-name*\) was unable to stop or start tasks during a deployment because of the service deployment configuration\. Update the minimumHealthyPercent or maximumPercent value and try again\.](service-event-messages.md#service-event-messages-7)\.

When a new service deployment is started or when a deployment is completed, Amazon ECS sends a service deployment state change event to EventBridge\. This provides a programmatic way to monitor the status of your service deployments\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.

In order to take advantage of all the features, use the new console or the AWS CLI to deploy your service\.

## Failure detection methods<a name="deployment-failure-detection"></a>

The rolling update deployment has two methods which provide a way for you to quickly identify when a deployment has failed, and then to optionally roll back the failure to the last working deployment\.
+ [Deployment circuit breaker](deployment-circuit-breaker.md)
+ [CloudWatch alarms](deployment-alarm-failure.md)

You can use either method, or both methods together\. When you use both methods together, the deployment is set to failed as soon as the failure criteria for either failure method is met\.

Use the following guidelines to help determine which method to use:
+ Circuit breaker \- Use this method when you want to stop a deployment when the tasks can't start\.
+ CloudWatch alarms \- Use this method when you want to stop a deployment based on application metrics\.