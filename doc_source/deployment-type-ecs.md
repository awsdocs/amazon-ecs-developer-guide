# Rolling update<a name="deployment-type-ecs"></a>

When the *rolling update* \(`ECS`\) deployment type is used for your service, when a new service deployment is started the Amazon ECS service scheduler replaces the currently running tasks with new tasks\. The number of tasks that Amazon ECS adds or removes from the service during a rolling update is controlled by the deployment configuration\. The deployment configuration consists of the `minimumHealthyPercent` and `maximumPercent` values which are defined when the service is created, but can also be updated on an existing service\.

The `minimumHealthyPercent` represents the lower limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for the service\. This value is rounded up\. For example if the minimum healthy percent is `50` and the desired task count is four, then the scheduler can stop two existing tasks before starting two new tasks\. Likewise, if the minimum healthy percent is 75% and the desired task count is two, then the scheduler can't stop any tasks due to the resulting value also being two\.

The `maximumPercent` represents the upper limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for a service\. This value is rounded down\. For example if the maximum percent is `200` and the desired task count is four then the scheduler can start four new tasks before stopping four existing tasks\. Likewise, if the maximum percent is `125` and the desired task count is three, the scheduler can't start any tasks due to the resulting value also being three\.

**Important**  
When setting a minimum healthy percent or a maximum percent, you should ensure that the scheduler can stop or start at least one task when a deployment is triggered\. If your service has a deployment that is stuck due to an invalid deployment configuration, a service event message will be sent\. For more information, see [service \(*service\-name*\) was unable to stop or start tasks during a deployment because of the service deployment configuration\. Update the minimumHealthyPercent or maximumPercent value and try again\.](service-event-messages.md#service-event-messages-7)\.

When a new service deployment is started or when a deployment is completed, Amazon ECS sends a service deployment state change event to EventBridge\. This provides a programmatic way to monitor the status of your service deployments\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.

To create a new Amazon ECS service that uses the rolling update deployment type, see [Creating an Amazon ECS service](create-service.md)\.

## Using the deployment circuit breaker<a name="deployment-circuit-breaker"></a>

By default, when a service using the rolling update deployment type starts a new deployment, the service scheduler will launch new tasks until the desired count is reached\. You can optionally use deployment circuit breaker logic on the service, which will cause the deployment to transition to a failed state if it can't reach a steady state\. The deployment circuit breaker logic can also trigger Amazon ECS to roll back to the last completed deployment upon a deployment failure\.

The following `create-service` AWS CLI example shows how to create a Linux service when the deployment circuit breaker enabled with rollback\.

```
aws ecs create-service \
     --service-name MyService \
     --deployment-controller type=ECS \
     --desired-count 2 \
     --deployment-configuration "deploymentCircuitBreaker={enable=true,rollback=true}" \
     --task-definition sample-fargate:1 \
     --launch-type FARGATE \
     --platform-os LINUX \
     --platform-version 1.4.0 \
     --network-configuration "awsvpcConfiguration={subnets=[subnet-12344321],securityGroups=[sg-12344321],assignPublicIp=ENABLED}"
```

The following should be considered when enabling the deployment circuit breaker logic on a service\.
+ The deployment circuit breaker is only supported for Amazon ECS services that use the rolling update \(`ECS`\) deployment controller and don't use a Classic Load Balancer\.
+ If a service deployment has at least one successfully running task, the circuit breaker logic will not trigger regardless of the deployment having any previous or future failed tasks\.
+ There are two new parameters added to the response of a `DescribeServices` API action that provide insight into the state of a deployment, the `rolloutState` and `rolloutStateReason`\. When a new deployment is started, the rollout state begins in an `IN_PROGRESS` state\. When the service reaches a steady state, the rollout state transitions to `COMPLETED`\. If the service fails to reach a steady state and circuit breaker is enabled, the deployment will transition to a `FAILED` state\. A deployment in a `FAILED` state won't launch any new tasks\.
+ In addition to the service deployment state change events Amazon ECS sends for deployments that have started and have completed, Amazon ECS also sends an event when a deployment with circuit breaker enabled fails\. These events provide details about why a deployment failed or if a deployment was started because of a rollback\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.
+ If a new deployment is started because a previous deployment failed and rollback was enabled, the `reason` field of the service deployment state change event will indicate the deployment was started because of a rollback\.

### Failure threshold<a name="failure-threshold"></a>

The deployment circuit breaker calculates the threshold value, and then uses the value to determine when to move the deployment to a `FAILED` state\.

The deployment circuit breaker has a a minimum threshold of 10 and a maximum threshold of 200\. and uses the values in the following formula to determine the deployment failure\.

```
Minimum threshold <= 0.5 * desired service count => maximum threshold
```

When the result of the calculation is less than the minimum of 10, the failure threshold is set to 10\. When the result of the calculation is greater than the maximum of 200, the failure threshold is set to 200\.

**Note**  
You cannot change either of the threshold values\.

There are two stages for the deployment status check\.

1. The deployment circuit breaker monitors tasks that are part of the deployment and checks for tasks that are in the `RUNNING` state\. The scheduler ignores the failure criteria when a task in the current deployment is in the `RUNNING` state and proceeds to the next stage\. When tasks fail to reach in the `RUNNING` state, the deployment circuit breaker increases the failure count by one\. When the failure count equals the threshold, the deployment is marked as `FAILED`\.

1. This stage is entered when there are one of more tasks in the `RUNNING` state\. The deployment circuit breaker performs health checks on the following resources for the tasks in the current deployment:
   + Elastic Load Balancing
   + AWS Cloud Map

   When a health check fails for the task, the deployment circuit breaker increases the failure count by one\. When the failure count equals the threshold, the deployment is marked as `FAILED`\.

The following table provides some examples\.


| Desired service count | Calculation | Threshold | 
| --- | --- | --- | 
|  1  |  <pre>10 <= 0.5 * 1 => 200</pre>  | 10 \(the calculated value is less than the minimum\) | 
|  25  |  <pre>10 <= 0.5 * 25 => 200</pre>  | 13 \(the value is rounded up\) | 
|  400  |  <pre>10 <= 0.5 * 200 => 200</pre>  | 200 | 
|  800  |  <pre>10 <= 0.5 * 800 => 200</pre>  | 200 \(the calculated value is greater than the maximum\) | 

For additional examples about using the rollback option, see [Announcing Amazon ECS deployment circuit breaker](https://aws.amazon.com/blogs/containers/announcing-amazon-ecs-deployment-circuit-breaker/)\.