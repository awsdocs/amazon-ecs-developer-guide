# Task scale\-in protection<a name="task-scale-in-protection"></a>

You can use Amazon ECS task scale\-in protection to protect your tasks from being terminated by scale\-in events from either [Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) or [deployments](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-types.html)\.

Certain applications require a mechanism to safeguard mission\-critical tasks from termination by scale\-in events during times of low utilization or during service deployments\. For example:
+ You have a queue\-processing asynchronous application such as a video transcoding job where some tasks need to run for hours even when cumulative service utilization is low\.
+ You have a gaming application that runs game servers as Amazon ECS tasks that need to continue running even if all users have logged\-out to reduce startup latency of a server reboot\.
+ When you deploy a new code version, you need tasks to continue running because it would be expensive to reprocess\.

To protect tasks belonging to your Amazon ECS service from terminating in a scale\-in event, set the `protectionEnabled` attribute to `true`\. By default, task are protected for 2 hours\. You can customize the protection period by using the `expiresInMinutes` attribute\. You can protect your tasks for a minimum of 1 minute and up to a maximum of 2880 minutes \(48 hours\)\.

After a task finishes its requisite work, you can set the `protectionEnabled` attribute to `false`, allowing the task to be terminated by subsequent scale\-in events\.

## Task scale\-in protection mechanisms<a name="task-scale-in-protection-mechanisms"></a>

You can set and get task scale\-in protection using either the Amazon ECS container agent endpoint or the Amazon ECS API\.

### Set task scale\-in protection<a name="set-task-scale-in-protection"></a>

You can set task scale\-in protection in the following ways:
+ **Amazon ECS container agent endpoint**

  We recommend using the Amazon ECS container agent endpoint for tasks that can self\-determine the need to be protected\. Use this approach for queue\-based or job\-processing workloads\.

  When a container starts processing work, for example by consuming an SQS message, you can set the `ProtectionEnabled` attribute through the task scale\-in protection endpoint path `$ECS_AGENT_URI/task-protection/v1/state` from within the container\. Amazon ECS will not terminate this task during scale\-in events\. After your task finishes its work, you can unset the `ProtectionEnabled` attribute using the same endpoint, making the task eligible for termination during subsequent scale\-in events\.

  For more information on using the Amazon ECS container agent endpoint, see [Task scale\-in protection endpoint](task-scale-in-protection-endpoint.md)\.
+ **Amazon ECS API**

  You can use the Amazon ECS API to set task scale\-in protection if your application has a component that tracks the status of active tasks\. Use the `UpdateTaskProtection` API to mark one or more tasks as protected\.

  An example of this approach would be if your application is hosting game server sessions as Amazon ECS tasks\. When a user logs in to a session on the server \(task\), you can mark the task as protected\. After the user logs out, you can either unset protection specifically for this task or periodically unset protection for similar tasks that no longer have active sessions, depending on your requirement to keep idle servers\.

  For more information on using the `UpdateTaskProtection` API, see [UpdateTaskProtection](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateTaskProtection.html) in the Amazon Elastic Container Service API Reference\.

You can combine both approaches\. For example, use the Amazon ECS agent endpoint to set task protection from within a container and use the Amazon ECS API to unset task protection from your external controller service\.

### Get task protection status<a name="get-task-scale-in-protection"></a>

To get the protection status of tasks in an Amazon ECS service, you can do one of the following:
+ **Amazon ECS container agent endpoint**

  Configure the container definition to use the Amazon task scale\-in protection endpoint path\. For more information, see [Task scale\-in protection endpoint](task-scale-in-protection-endpoint.md)\.
+ **Amazon ECS API**

  Use the `GetTaskProtection` API\. For more information, see [GetTaskProtection](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_GetTaskProtection.html) in the Amazon Elastic Container Service API Reference\.

## Task scale\-in protection considerations<a name="task-scale-in-protection-considerations"></a>

Consider the following points before using task scale\-in protection:
+ If you can, use the Amazon ECS agent endpoint instead of the API, as the Amazon ECS agent has inbuilt retry mechanisms and a simpler interface\.
+ You can reset the task scale\-in protection expiration period by invoking the API on a task that already has protection turned on\.
+ Determine how long a task would need to complete its requisite work and set the `expiresInMinutes` property accordingly\. If you set the protection expiration longer than necessary, then you will incur costs and face delays in the deployment of new tasks\.
+ Deployment considerations:
  + If the Service is using a rolling update, new tasks will be created but tasks running older version will not be terminated until `protectionEnabled` is unset or expires\. You can adjust the `maximumPercentage` parameter in deployment configuration to a value that allows new tasks to be created when old tasks are protected\.
  + If a Blue/Green update is applied, the Blue stack with protected tasks will not be removed if tasks have `protectionEnabled`\. Traffic will be diverted to the new tasks that come up and older tasks will only be removed when `protectionEnabled` is unset or expires\. Depending on the timeout of the CodeDeploy or Cloud Formation updates, the deployment may timeout and the older Blue tasks may still be present\.
  + If you use CloudFormation, the update\-stack has a 3 hour timeout\. Therefore, if you set your task protection for longer than 3 hours, then your CloudFormation deployment may result in failure and rollback\.

    During the time your old tasks are protected, the CloudFormation stack shows `UPDATE_IN_PROGRESS`, if task scale\-in protection is unset or expires within the 3 hour window, your deployment will succeed and move to the `UPDATE_COMPLETE` status\. If the deployment is stuck in `UPDATE_IN_PROGRESS` for more than 3 hours, it will fail and show `UPDATE_FAILED` state, and will then be rolled back to old task set\.
  + Amazon ECS will vend service events if protected tasks are keeping a deployment \(rolling or blue/green\) from reaching steady state, so that you can take remedial actions\. While trying to update the protection status of a task, if you receive a `DEPLOYMENT_BLOCKED` error message, it means the service has more protected tasks than the desired count of tasks for the service\. To resolve this error, do one the following:
    + Wait for the current task protection to expire\. Then set task protection\.
    + Determine which tasks can be stopped\. Then use the `update-task-protection` API with the `protectionEnabled` option set to `False` for these tasks\.
    + Increase the desired task count of the service to more than the number of protected tasks\.

## IAM permissions required for task scale\-in protection<a name="task-scale-in-protection-iam"></a>

If you plan to use the Amazon ECS container agent API to get or update task protection, the task must have the Amazon ECS task role with the following permissions:
+ `ecs:GetTaskProtection`: Allows the Amazon ECS container agent to call the Amazon ECS GetTaskProtection API when a GET request is made to the Agent API endpoint\.
+ `ecs:UpdateTaskProtection`: Allows the Amazon ECS container agent to call the Amazon ECS UpdateTaskProtection API when a PUT request is made to the Agent API endpoint\.