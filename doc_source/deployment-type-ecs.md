# Rolling update<a name="deployment-type-ecs"></a>

When the *rolling update* \(`ECS`\) deployment type is used for your service, when a new service deployment is started the Amazon ECS service scheduler replaces the currently running tasks with new tasks\. The number of tasks that Amazon ECS adds or removes from the service during a rolling update is controlled by the deployment configuration\. The deployment configuration consists of the `minimumHealthyPercent` and `maximumPercent` values which are defined when the service is created, but can also be updated on an existing service\.

The `minimumHealthyPercent` represents the lower limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for the service\. This value is rounded up\. For example if the minimum healthy percent is `50` and the desired task count is four, then the scheduler can stop two existing tasks before starting two new tasks\. Likewise, if the minimum healthy percent is 75% and the desired task count is two, then the scheduler can't stop any tasks due to the resulting value also being two\.

The `maximumPercent` represents the upper limit on the number of tasks that should be running for a service during a deployment or when a container instance is draining, as a percent of the desired number of tasks for a service\. This value is rounded down\. For example if the maximum percent is `200` and the desired task is four then scheduler can start four new tasks before stopping four existing tasks\. Likewise, if the maximum percent is `125` and the desired task count is three, the scheduler can't start any tasks due to the resulting value also being three\.

**Important**  
When setting a minimum healthy percent or a maximum percent, you should ensure that the scheduler can stop or start at least one task when a deployment is triggered\. If your service has a deployment that is stuck due to an invalid deployment configuration, a service event message will be sent\. For more information, see [service \(*service\-name*\) was unable to stop or start tasks during a deployment because of the service deployment configuration\. Update the minimumHealthyPercent or maximumPercent value and try again\.](service-event-messages.md#service-event-messages-7)\.

When a new service deployment is started or when a deployment is completed, Amazon ECS sends a service deployment state change event to EventBridge\. This provides a programmatic way to monitor the status of your service deployments\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.

To create a new Amazon ECS service that uses the rolling update deployment type, see [Creating a service](create-service.md)\.

## Using the deployment circuit breaker<a name="deployment-circuit-breaker"></a>

By default, when a service using the rolling update deployment type starts a new deployment, the service scheduler will launch new tasks until the desired count is reached\. You can optionally enable deployment circuit breaker logic on the service, which will cause the deployment to transition to a failed state if it can't reach a steady state\. The deployment circuit breaker logic can also trigger Amazon ECS to roll back to the last completed deployment upon a deployment failure\.

The deployment circuit breaker can only be configured for a service using the Amazon ECS API, AWS CLI, or SDK\. The following `create-service` AWS CLI example shows how to create a service when the deployment circuit breaker enabled with rollback\.

```
aws ecs create-service \
     --service-name MyService \
     --deployment-controller type=ECS \
     --desired-count 2 \
     --deployment-configuration "deploymentCircuitBreaker={enable=true,rollback=true}" \
     --task-definition sample-fargate:1 \
     --launch-type FARGATE \
     --platform-version 1.4.0 \
     --network-configuration "awsvpcConfiguration={subnets=[subnet-12344321],securityGroups=[sg-12344321],assignPublicIp=ENABLED}"
```

The following should be considered when enabling the deployment circuit breaker logic on a service\.
+ The deployment circuit breaker is only supported for Amazon ECS services that use the rolling update \(`ECS`\) deployment controller and don't use a Classic Load Balancer\.
+ If a service deployment has at least one successfully running task, the circuit breaker logic will not trigger regardless of the deployment having any previous or future failed tasks\.
+ There are two new parameters added to the response of a `DescribeServices` API action that provide insight into the state of a deployment, the `rolloutState` and `rolloutStateReason`\. When a new deployment is started, the rollout state begins in an `IN_PROGRESS` state\. When the service reaches a steady state, the rollout state transitions to `COMPLETED`\. If the service fails to reach a steady state and circuit breaker is enabled, the deployment will transition to a `FAILED` state\. A deployment in a `FAILED` state won't launch any new tasks\.
+ In addition to the service deployment state change events Amazon ECS sends for deployments that have started and have completed, Amazon ECS also sends an event when a deployment with circuit breaker enabled fails\. These events provide details about why a deployment failed or if a deployment was started because of a rollback\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.
+ If a new deployment is started because a previous deployment failed and rollback was enabled, the `reason` field of the service deployment state change event will indicate the deployment was started because of a rollback\.