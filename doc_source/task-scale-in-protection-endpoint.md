# Task scale\-in protection endpoint<a name="task-scale-in-protection-endpoint"></a>

**Important**  
  
If you are using Amazon ECS tasks hosted on AWS Fargate, see [Task scale\-in protection endpoint](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task-scale-in-protection-endpoint.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

Amazon ECS task scale\-in protection enables you to protect your tasks from being terminated by scale\-in events from either [Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) or [deployments](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-types.html)\. You can set and get the status of task scale\-in protection in the task container application code\.

The Amazon ECS container agent uses AWS APIs [GetTaskProtection](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_GetTaskProtection.html) and [UpdateTaskProtection](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateTaskProtection.html) to offer task scale\-in protection\. For a list of failure reasons, see [API failure reasons](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/api_failures_messages.html)\.

For more information on task scale\-in protection, see [Task scale\-in protection](task-scale-in-protection.md)

## Task scale\-in protection endpoint path<a name="task-scale-in-protection-path"></a>

The following task scale\-in protection endpoint path is available to containers: `$ECS_AGENT_URI/task-protection/v1/state`

A PUT request to this URI from within a container will set task scale\-in protection\. A GET request to this URI will get the current protection status of a task\.

## Task scale\-in protection endpoint request<a name="task-scale-in-protection-request"></a>

You can set task scale\-in protection using the `(${ECS_AGENT_URI/task-protection/v1/state})` endpoint with the following request parameters\.

### Protect<a name="w193aac22c30c11c13b5"></a>

`ProtectionEnabled`  
Specify `true` to mark a task for protection and `false` to unset protection, making it eligible for termination\.  
Type: Boolean  
Required: Yes

`ExpiresInMinutes`  
If you set `protectionEnabled` to `true`, you can use the `expiresInMinutes` parameter to specify how long your task must be protected\. You can specify a minimum of 1 minute to up to 2,880 minutes \(48 hours\)\. During this time period, your task will not be terminated by scale\-in events from Service Auto Scaling or deployments\. After this time period lapses, the `protectionEnabled` parameter will be reset to `false`  
If you don’t specify the time, then the task is automatically protected for 120 minutes \(2 hours\)\.  
Type: Integer  
Required: No

The following examples show how to set task protection with different durations\.

#### Example 1<a name="w193aac22c30c11c13b5b7"></a>

This example shows how to protect a task with the default time period of 2 hours\.

```
PUT $ECS_AGENT_URI/task-protection/v1/state -d 
'{"ProtectionEnabled":true}'
```

#### Example 2<a name="w193aac22c30c11c13b5b9"></a>

This example shows how to protect a task for 60 minutes using the `expiresInMinutes` parameter\.

```
PUT $ECS_AGENT_URI/task-protection/v1/state -d 
'{"ProtectionEnabled":true,"ExpiresInMinutes":60}'
```

#### Example 3<a name="w193aac22c30c11c13b5c11"></a>

This example shows how to protect a task for 24 hours using the `expiresInMinutes` parameter\.

```
PUT $ECS_AGENT_URI/task-protection/v1/state -d 
'{"ProtectionEnabled":true,"ExpiresInMinutes":1440}'
```

## Task scale\-in protection endpoint response<a name="task-scale-in-protection-response"></a>

The following information is returned from the task scale\-in protection endpoint \(`${ECS_AGENT_URI/task-protection/v1/state}`\) JSON response\.

### Protect<a name="w193aac22c30c11c15b5"></a>

`ExpirationDate`  
The epoch time when protection for the task will expire\. If the task is not protected, this value will be null\.

`ProtectionEnabled`  
The protection status of the task\. If scale\-in protection is enabled for a task, the value is `true`\. Otherwise, it is `false`\.

`TaskArn`  
The full Amazon Resource Name \(ARN\) of the task that the container belongs to\.

The following example shows the details returned for a protected task\.

```
GET $ECS_AGENT_URI/task-protection/v1/state
```

```
{
    "protection":{
        "ExpirationDate":"2022-10-06T02:29:16Z",
        "ProtectionEnabled":true,
        "TaskArn":"arn:aws:ecs:us-west-2:111122223333:task/1234567890abcdef0"
    }
}
```

### Failure<a name="w193aac22c30c11c15b7"></a>

The following information is returned when a failure occurs\.

`Arn`  
The full Amazon Resource Name \(ARN\) of the task\.

`Detail`  
The details related to the failure\.

`Reason`  
The reason for the failure\.

The following example shows the details returned for a task that is not protected\.

```
{
    "failure":{
        "Arn":"arn:aws:ecs:us-west-2:111122223333:task/1234567890abcdef0",
        "Detail":null,
        "Reason":"TASK_NOT_VALID"
    }
}
```

### Error<a name="w193aac22c30c11c15b9"></a>

The following information is returned when an exception occurs\.

`requestID`  
The AWS request ID for the Amazon ECS API call that results in an exception\.

`Arn`  
The full Amazon Resource Name \(ARN\) of the task or service\.

`Code`  
The error code\.

`Message`  
The error message\.  
If a `RequestError` or `RequestTimeout` error appears, it is likely that its a networking issue\. Try using VPC endpoints for Amazon ECS\.

The following example shows the details returned when an error occurs\.

```
{
    "requestID":"12345-abc-6789-0123-abc",
    "error":{
        "Arn":"arn:aws:ecs:us-west-2:555555555555:task/my-cluster-name/1234567890abcdef0",
        "Code":"AccessDeniedException",
        "Message":"User: arn:aws:sts::444455556666:assumed-role/my-ecs-task-role/1234567890abcdef0 is not authorized to perform: ecs:GetTaskProtection on resource: arn:aws:ecs:us-west-2:555555555555:task/test/1234567890abcdef0 because no identity-based policy allows the ecs:GetTaskProtection action"
    }    
}
```

The following error appears if the Amazon ECS agent is unable to get a response from the Amazon ECS endpoint for reasons such as network issues or the Amazon ECS control plane is down\.

```
{
  "error": {
    "Arn": "arn:aws:ecs:us-west-2:555555555555:task/my-cluster-name/1234567890abcdef0",
    "Code": "RequestCanceled",
    "Message": "Timed out calling Amazon ECS Task Protection API"
  }
}
```

The following error appears when the Amazon ECS agent gets a throttling exception from Amazon ECS\.

```
{
  "requestID": "12345-abc-6789-0123-abc",
  "error": {
    "Arn": "arn:aws:ecs:us-west-2:555555555555:task/my-cluster-name/1234567890abcdef0",
    "Code": "ThrottlingException",
    "Message": "Rate exceeded"
  }
}
```