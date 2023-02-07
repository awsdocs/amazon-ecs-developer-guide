# CloudWatch alarms<a name="deployment-alarm-failure"></a>

You can configure Amazon ECS to set the deployment to failed when it detects that a specified CloudWatch alarm has gone into the `ALARM` state\.

You can optionally set the configuration to roll back a failed deployment to the last completed deployment\.

The following `create-service` AWS CLI example shows how to create a Linux service when the deployment alarms are used with the rollback option\.

```
aws ecs create-service \
     --service-name MyService \
     --deployment-controller type=ECS \
     --desired-count 2 \
     --deployment-configuration "alarms={alarmNames=[alarm1Name,alarm2Name],enable=true,rollback=true}" \
     --task-definition sample-fargate:1 \
     --launch-type FARGATE \
     --platform-os LINUX \
     --platform-version 1.4.0 \
     --network-configuration "awsvpcConfiguration={subnets=[subnet-12344321],securityGroups=[sg-12344321],assignPublicIp=ENABLED}"
```

Consider the following when you use the Amazon CloudWatch alarms method on a service\.
+ The `deploymentConfiguration` request parameter now contains the `alarms` data type\. You can specify the alarm names, whether to use the method, and whether to initiate a rollback when the alarms indicate a deployment failure\. For more information, see [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html) in the *Amazon Elastic Container Service API Reference*\.
+ The `DescribeServices` response provides insight into the state of a deployment, the `rolloutState` and `rolloutStateReason`\. When a new deployment starts, the rollout state begins in an `IN_PROGRESS` state\. When the service reaches a steady state and the bake time is complete, the rollout state transitions to `COMPLETED`\. If the service fails to reach a steady state and the alarm has gone into the `ALARM` state, the deployment will transition to a `FAILED` state\. A deployment in a `FAILED` state won't launch any new tasks\.
+ In addition to the service deployment state change events Amazon ECS sends for deployments that have started and have completed, Amazon ECS also sends an event when a deployment that uses alarms fails\. These events provide details about why a deployment failed or if a deployment was started because of a rollback\. For more information, see [Service deployment state change events](ecs_cwe_events.md#ecs_service_deployment_events)\.
+ If a new deployment is started because a previous deployment failed and rollback was turned on, the `reason` field of the service deployment state change event will indicate the deployment was started because of a rollback\.
+ If you use the deployment circuit breaker and the Amazon CloudWatch alarms to detect failures, either one can initiate a deployment failure as soon as the criteria for either method is met\. A rollback occurs when you use the rollback option for the method that initiated the deployment failure\.
+ The Amazon CloudWatch alarms is only supported for Amazon ECS services that use the rolling update \(`ECS`\) deployment controller\.
+ You can configure this option by using the new Amazon ECS console, or the AWS CLI\. For more information, see [Create a service using defined parameters](create-service-console-v2.md#create-custom-service) and [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html) in the *AWS Command Line Interface Reference*\.
+ You might notice that the deployment status remains `IN_PROGRESS` for a prolonged amount of time\. The reason for this is that Amazon ECS does not change the status until it has deleted the active deployment, and this does not happen until after the bake time\. Depending on your alarm configuration, the deployment might appear to take several minutes longer than it does when you don't use alarms \(even though the new primary task set is scaled up and the old deployment is scaled down\)\. If you use CloudFormation timeouts, consider increasing the timeouts\. For more information, see [Creating wait conditions in a template](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-waitcondition.html) in the *AWS CloudFormation User Guide*\.
+ Amazon ECS calls `DescribeAlarms` to poll the alarms\. The calls to `DescribeAlarms` count toward the CloudWatch service quotas associated with your account\. If you have other AWS services that call `DescribeAlarms`, there might be an impact on Amazon ECS to poll the alarms\. For example, if another service makes enough `DescribeAlarms` calls to reach the quota, that service is throttled and Amazon ECS' is also throttled and unable to poll alarms\. If an alarm is generated during the throttling period, Amazon ECS' might miss the alarm and the roll back might not occur\. There is no other impact on the deployment\. For more information on CloudWatch service quotas, see [CloudWatch service quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_limits.htm) in the *CloudWatch User Guide*\.
+ If an alarm is in the `ALARM` state at the beginning of a deployment, Amazon ECS will not monitor alarms for the duration of that deployment \(Amazon ECS ignores the alarm configuration\)\. This behavior address the case where you want to start a new deployment to fix an initial deployment failure\.

## Recommended alarms<a name="ecs-deployment-alarms"></a>

We recommend that you use the following alarm metrics:
+ If you use an Application Load Balancer, use the `HTTPCode_ELB_5XX_Count` and `HTTPCode_ELB_4XX_Count` Application Load Balancer metrics\. These metrics check for HTTP spikes\. For more information about the Application Load Balancer metrics, see [CloudWatch metrics for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html) in the *User Guide for Application Load Balancers*\.
+ If you have an existing application, use the `CPUUtilization` and `MemoryUtilization` metrics\. These metrics check for the percentage of CPU and memory that the cluster or service uses\. For more information, see [Using CloudWatch metrics](cloudwatch-metrics.md#enable_cloudwatch)\.
+ If you use Amazon Simple Queue Service queues in your tasks, use `ApproximateNumberOfMessagesDelayed` Amazon SQS metric\. This metric checks for number of messages in the queue that are delayed and not available for reading immediately\. For more information about Amazon SQS metrics, see [Available CloudWatch metrics for Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html) in the *Amazon Simple Queue Service Developer Guide*\.