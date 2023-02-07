# Deployment circuit breaker<a name="deployment-circuit-breaker"></a>

A deployment transitions to the failed state when the deployment circuit breaker logic determines that the tasks cannot reach a steady state\. 

The deployment circuit breaker logic has an option that will automatically roll back a failed deployment to the last working state\.

Consider the following when you use the deployment circuit breaker method on a service\.
+ If a service deployment has at least one successfully running task at the time the service is updated, the circuit breaker logic will not start regardless of the deployment having any previous or future failed tasks\.
+ The `DescribeServices` response provides insight into the state of a deployment, the `rolloutState` and `rolloutStateReason`\. When a new deployment is started, the rollout state begins in an `IN_PROGRESS` state\. When the service reaches a steady state, the rollout state transitions to `COMPLETED`\. If the service fails to reach a steady state and circuit breaker is turned on, the deployment will transition to a `FAILED` state\. A deployment in a `FAILED` state doesn't launch any new tasks\.
+ In addition to the service deployment state change events Amazon ECS sends for deployments that have started and have completed, Amazon ECS also sends an event when a deployment with circuit breaker turned on fails\. These events provide details about why a deployment failed or if a deployment was started because of a rollback\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.
+ If a new deployment is started because a previous deployment failed and a rollback occurred, the `reason` field of the service deployment state change event indicates the deployment was started because of a rollback\.
+ The deployment circuit breaker is only supported for Amazon ECS services that use the rolling update \(`ECS`\) deployment controller\.
+ You must use the new Amazon ECS console, or the AWS CLI when you use the deployment circuit breaker with the CloudWatch option\. For more information, see [Create a service using defined parameters](create-service-console-v2.md#create-custom-service) and [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html) in the *AWS Command Line Interface Reference*\.

The following `create-service` AWS CLI example shows how to create a Linux service when the deployment circuit breaker is used with rollback\.

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

## Failure threshold<a name="failure-threshold"></a>

The deployment circuit breaker calculates the threshold value, and then uses the value to determine when to move the deployment to a `FAILED` state\.

The deployment circuit breaker has a minimum threshold of 10 and a maximum threshold of 200\. and uses the values in the following formula to determine the deployment failure\.

```
Minimum threshold <= 0.5 * desired task count => maximum threshold
```

When the result of the calculation is less than the minimum of 10, the failure threshold is set to 10\. When the result of the calculation is greater than the maximum of 200, the failure threshold is set to 200\.

**Note**  
You cannot change either of the threshold values\.

There are two stages for the deployment status check\.

1. The deployment circuit breaker monitors tasks that are part of the deployment and checks for tasks that are in the `RUNNING` state\. The scheduler ignores the failure criteria when a task in the current deployment is in the `RUNNING` state and proceeds to the next stage\. When tasks fail to reach in the `RUNNING` state, the deployment circuit breaker increases the failure count by one\. When the failure count equals the threshold, the deployment is marked as `FAILED`\.

1. This stage is entered when there are one of more tasks in the `RUNNING` state\. The deployment circuit breaker performs health checks on the following resources for the tasks in the current deployment:
   + Elastic Load Balancing load balancers
   + AWS Cloud Map service
   + Amazon ECS container health checks

   When a health check fails for the task, the deployment circuit breaker increases the failure count by one\. When the failure count equals the threshold, the deployment is marked as `FAILED`\.

The following table provides some examples\.


| Desired task count | Calculation | Threshold | 
| --- | --- | --- | 
|  1  |  <pre>10 <= 0.5 * 1 => 200</pre>  | 10 \(the calculated value is less than the minimum\) | 
|  25  |  <pre>10 <= 0.5 * 25 => 200</pre>  | 13 \(the value is rounded up\) | 
|  400  |  <pre>10 <= 0.5 * 400 => 200</pre>  | 200 | 
|  800  |  <pre>10 <= 0.5 * 800 => 200</pre>  | 200 \(the calculated value is greater than the maximum\) | 

For additional examples about how to use the rollback option, see [Announcing Amazon ECS deployment circuit breaker](https://aws.amazon.com/blogs/containers/announcing-amazon-ecs-deployment-circuit-breaker/)\.