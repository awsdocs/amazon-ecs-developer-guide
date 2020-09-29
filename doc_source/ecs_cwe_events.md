# Amazon ECS events<a name="ecs_cwe_events"></a>

Amazon ECS sends three types of events to EventBridge: container instance state change events, task state change events, and service action events\. If these resources change, an event is triggered\. These events and their possible causes are described in greater detail in the following sections\.

**Note**  
Amazon ECS may add other event types, sources, and details in the future\. If you are programmatically deserializing event JSON data, make sure that your application is prepared to handle unknown properties to avoid issues if and when these additional properties are added\.

In some cases, multiple events are triggered for the same activity\. For example, when a task is started on a container instance, a task state change event is triggered for the new task\. A container instance state change event is triggered to account for the change in available resources, such as CPU, memory, and available ports, on the container instance\. Likewise, if a container instance is terminated, events are triggered for the container instance, the container agent connection status, and every task that was running on the container instance\.

Container state change and task state change events contain two `version` fields: one in the main body of the event, and one in the `detail` object of the event\. The following describes the differences between these two fields:
+ The `version` field in the main body of the event is set to `0` on all events\. For more information about EventBridge parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.
+ The `version` field in the `detail` object of the event describes the version of the associated resource\. Each time a resource changes state, this version is incremented\. Because events can be sent multiple times, this field allows you to identify duplicate events\. Duplicate events have the same version in the `detail` object\. If you are replicating your Amazon ECS container instance and task state with EventBridge, you can compare the version of a resource reported by the Amazon ECS APIs with the version reported in EventBridge for the resource \(inside the `detail` object\) to verify that the version in your event stream is current\.

Service action events only contain the `version` field in the main body\.

## Container instance state change events<a name="ecs_container_instance_events"></a>

The following scenarios trigger container instance state change events:

You call the `StartTask`, `RunTask`, or `StopTask` API operations, either directly or with the AWS Management Console or SDKs\.  
Placing or stopping tasks on a container instance modifies the available resources on the container instance, such as CPU, memory, and available ports\.

The Amazon ECS service scheduler starts or stops a task\.  
Placing or stopping tasks on a container instance modifies the available resources on the container instance, such as CPU, memory, and available ports\.

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation with a `STOPPED` status for a task with a desired status of `RUNNING`\.  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes\. If a task that is supposed to be `RUNNING` is transitioned to `STOPPED`, the agent releases the resources that were allocated to the stopped task, such as CPU, memory, and available ports\.

You deregister the container instance with the `DeregisterContainerInstance` API operation, either directly or with the AWS Management Console or SDKs\.  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\.

A task was stopped when an EC2 instance was stopped\.   
When you stop a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\.

The Amazon ECS container agent registers a container instance for the first time\.   
The first time the Amazon ECS container agent registers a container instance \(at launch or when first run manually\), this creates a state change event for the instance\.

The Amazon ECS container agent connects or disconnects from Amazon ECS\.  
When the Amazon ECS container agent connects or disconnects from the Amazon ECS backend, it changes the `agentConnected` status of the container instance\.  
The Amazon ECS container agent disconnects and reconnects several times per hour as a part of its normal operation, so agent connection events should be expected\. These events are not an indication that there is an issue with the container agent or your container instance\.

You upgrade the Amazon ECS container agent on an instance\.  
The container instance detail contains an object for the container agent version\. If you upgrade the agent, this version information changes and triggers an event\.

**Example Container instance state change event**  
Container instance state change events are delivered in the following format\. The `detail` section below resembles the [ContainerInstance](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerInstance.html) object that is returned from a [DescribeContainerInstances](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeContainerInstances.html) API operation in the *Amazon Elastic Container Service API Reference*\. For more information about EventBridge parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.  

```
{
  "version": "0",
  "id": "8952ba83-7be2-4ab5-9c32-6687532d15a2",
  "detail-type": "ECS Container Instance State Change",
  "source": "aws.ecs",
  "account": "111122223333",
  "time": "2016-12-06T16:41:06Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:ecs:us-east-1:111122223333:container-instance/b54a2a04-046f-4331-9d74-3f6d7f6ca315"
  ],
  "detail": {
    "agentConnected": true,
    "attributes": [
      {
        "name": "com.amazonaws.ecs.capability.logging-driver.syslog"
      },
      {
        "name": "com.amazonaws.ecs.capability.task-iam-role-network-host"
      },
      {
        "name": "com.amazonaws.ecs.capability.logging-driver.awslogs"
      },
      {
        "name": "com.amazonaws.ecs.capability.logging-driver.json-file"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.17"
      },
      {
        "name": "com.amazonaws.ecs.capability.privileged-container"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.18"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.19"
      },
      {
        "name": "com.amazonaws.ecs.capability.ecr-auth"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.20"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.21"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.22"
      },
      {
        "name": "com.amazonaws.ecs.capability.docker-remote-api.1.23"
      },
      {
        "name": "com.amazonaws.ecs.capability.task-iam-role"
      }
    ],
    "clusterArn": "arn:aws:ecs:us-east-1:111122223333:cluster/default",
    "containerInstanceArn": "arn:aws:ecs:us-east-1:111122223333:container-instance/b54a2a04-046f-4331-9d74-3f6d7f6ca315",
    "ec2InstanceId": "i-f3a8506b",
    "registeredResources": [
      {
        "name": "CPU",
        "type": "INTEGER",
        "integerValue": 2048
      },
      {
        "name": "MEMORY",
        "type": "INTEGER",
        "integerValue": 3767
      },
      {
        "name": "PORTS",
        "type": "STRINGSET",
        "stringSetValue": [
          "22",
          "2376",
          "2375",
          "51678",
          "51679"
        ]
      },
      {
        "name": "PORTS_UDP",
        "type": "STRINGSET",
        "stringSetValue": []
      }
    ],
    "remainingResources": [
      {
        "name": "CPU",
        "type": "INTEGER",
        "integerValue": 1988
      },
      {
        "name": "MEMORY",
        "type": "INTEGER",
        "integerValue": 767
      },
      {
        "name": "PORTS",
        "type": "STRINGSET",
        "stringSetValue": [
          "22",
          "2376",
          "2375",
          "51678",
          "51679"
        ]
      },
      {
        "name": "PORTS_UDP",
        "type": "STRINGSET",
        "stringSetValue": []
      }
    ],
    "status": "ACTIVE",
    "version": 14801,
    "versionInfo": {
      "agentHash": "aebcbca",
      "agentVersion": "1.13.0",
      "dockerVersion": "DockerVersion: 1.11.2"
    },
    "updatedAt": "2016-12-06T16:41:06.991Z"
  }
}
```

## Task state change events<a name="ecs_task_events"></a>

The following scenarios trigger task state change events:

You call the `StartTask`, `RunTask`, or `StopTask` API operations, either directly or with the AWS Management Console, AWS CLI, or SDKs\.  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\.

The Amazon ECS service scheduler starts or stops a task\.  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\.

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation\.  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes\. State changes might include changes from `PENDING` to `RUNNING` or from `RUNNING` to `STOPPED`\.

You force deregistration of the underlying container instance with the `DeregisterContainerInstance` API operation and the `force` flag, either directly or with the AWS Management Console or SDKs\.  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\. If tasks are running on the container instance, the `force` flag must be set to allow deregistration\. This stops all tasks on the instance\.

The underlying container instance is stopped or terminated\.  
When you stop or terminate a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\.

A container in the task changes state\.  
The Amazon ECS container agent monitors the state of containers within tasks\. For example, if a container that is running within a task stops, this container state change triggers an event\.

A task using the Fargate Spot capacity provider receives a termination notice\.  
When a task is using the `FARGATE_SPOT` capacity provider and is stopped due to a Spot interruption, a task state change event is triggered\.

**Example Task state change event**  
Task state change events are delivered in the following format\. The `detail` section below resembles the [Task](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Task.html) object that is returned from a [DescribeTasks](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html) API operation in the *Amazon Elastic Container Service API Reference*\. If your containers are using an image hosted with Amazon ECR, the `imageDigest` field is returned\.  
The values for the `createdAt`, `connectivityAt`, `pullStartedAt`, `startedAt`, `pullStoppedAt`, and `updatedAt` fields are UNIX timestamps in the response of a DescribeTasks action whereas in the task state change event they are ISO string timestamps\.
For more information about CloudWatch Events parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.  

```
{
    "version": "0",
    "id": "3317b2af-7005-947d-b652-f55e762e571a",
    "detail-type": "ECS Task State Change",
    "source": "aws.ecs",
    "account": "111122223333",
    "time": "2020-01-23T17:57:58Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:ecs:us-west-2:111122223333:task/FargateCluster/c13b4cb40f1f4fe4a2971f76ae5a47ad"
    ],
    "detail": {
        "attachments": [
            {
                "id": "1789bcae-ddfb-4d10-8ebe-8ac87ddba5b8",
                "type": "eni",
                "status": "ATTACHED",
                "details": [
                    {
                        "name": "subnetId",
                        "value": "subnet-abcd1234"
                    },
                    {
                        "name": "networkInterfaceId",
                        "value": "eni-abcd1234"
                    },
                    {
                        "name": "macAddress",
                        "value": "0a:98:eb:a7:29:ba"
                    },
                    {
                        "name": "privateIPv4Address",
                        "value": "10.0.0.139"
                    }
                ]
            }
        ],
        "availabilityZone": "us-west-2c",
        "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/FargateCluster",
        "containers": [
            {
                "containerArn": "arn:aws:ecs:us-west-2:111122223333:container/cf159fd6-3e3f-4a9e-84f9-66cbe726af01",
                "lastStatus": "RUNNING",
                "name": "FargateApp",
                "image": "111122223333.dkr.ecr.us-west-2.amazonaws.com/hello-repository:latest",
                "imageDigest": "sha256:74b2c688c700ec95a93e478cdb959737c148df3fbf5ea706abe0318726e885e6",
                "runtimeId": "ad64cbc71c7fb31c55507ec24c9f77947132b03d48d9961115cf24f3b7307e1e",
                "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/FargateCluster/c13b4cb40f1f4fe4a2971f76ae5a47ad",
                "networkInterfaces": [
                    {
                        "attachmentId": "1789bcae-ddfb-4d10-8ebe-8ac87ddba5b8",
                        "privateIpv4Address": "10.0.0.139"
                    }
                ],
                "cpu": "0"
            }
        ],
        "createdAt": "2020-01-23T17:57:34.402Z",
        "launchType": "FARGATE",
        "cpu": "256",
        "memory": "512",
        "desiredStatus": "RUNNING",
        "group": "family:sample-fargate",
        "lastStatus": "RUNNING",
        "overrides": {
            "containerOverrides": [
                {
                    "name": "FargateApp"
                }
            ]
        },
        "connectivity": "CONNECTED",
        "connectivityAt": "2020-01-23T17:57:38.453Z",
        "pullStartedAt": "2020-01-23T17:57:52.103Z",
        "startedAt": "2020-01-23T17:57:58.103Z",
        "pullStoppedAt": "2020-01-23T17:57:55.103Z",
        "updatedAt": "2020-01-23T17:57:58.103Z",
        "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/FargateCluster/c13b4cb40f1f4fe4a2971f76ae5a47ad",
        "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/sample-fargate:1",
        "version": 4,
        "platformVersion": "1.3.0"
    }
}
```

For a tutorial walkthrough of setting up a simple AWS Lambda function that listens for Amazon ECS task events and writes them out to a CloudWatch Logs log stream, see [Tutorial: Listening for Amazon ECS CloudWatch Events](ecs_cwet.md)\.

For a tutorial walkthrough of creating an SNS topic to email you when a task state change event occurs, see [Tutorial: Sending Amazon Simple Notification Service alerts for task stopped events](ecs_cwet2.md)\.

## Service action events<a name="ecs_service_events"></a>

Amazon ECS sends service action events with the detail type **ECS Service Action**\. Unlike the container instance and task state change events, the service action events do not include a version number in the `details` response field\. The following is an event pattern that is used to create an EventBridge rule for Amazon ECS service action events\. For more information, see [Creating an EventBridge Rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

```
{
    "source": [
        "aws.ecs"
    ],
    "detail-type": [
        "ECS Service Action"
    ]
}
```

Amazon ECS sends events with `INFO`, `WARN`, and `ERROR` event types\. The following are the service action events\.

### Service action events with `INFO` event type<a name="ecs_service_events_info_type"></a>

`SERVICE_STEADY_STATE`  
The service is healthy and at the desired number of tasks, thus reaching a steady state\.

`TASKSET_STEADY_STATE`  
The task set is healthy and at the desired number of tasks, thus reaching a steady state\.

`CAPACITY_PROVIDER_STEADY_STATE`  
A capacity provider associated with a service reaches a steady state\.

`SERVICE_DESIRED_COUNT_UPDATED`  
When the service scheduler updates the computed desired count for a service or task set\. This event is not sent when the desired count is manually updated by a user\.

### Service action events with `WARN` event type<a name="ecs_service_events_warn_type"></a>

`SERVICE_TASK_START_IMPAIRED`  
The service is unable to consistently start tasks successfully\.

`SERVICE_DISCOVERY_INSTANCE_UNHEALTHY`  
A service using service discovery contains an unhealthy task\. The service scheduler detects that a task within a service registry is unhealthy\.

### Service action events with `ERROR` event type<a name="ecs_service_events_error_type"></a>

`SERVICE_DAEMON_PLACEMENT_CONSTRAINT_VIOLATED`  
A task in a service using the `DAEMON` service scheduler strategy no longer meets the placement constraint strategy for the service\.

`ECS_OPERATION_THROTTLED`  
The service scheduler has been throttled due to the Amazon ECS API throttle limits\.

`SERVICE_DISCOVERY_OPERATION_THROTTLED`  
The service scheduler has been throttled due to the AWS Cloud Map API throttle limits\. This can occur on services configured to use service discovery\.

`SERVICE_TASK_PLACEMENT_FAILURE`  
The service scheduler is unable to place a task\. The cause will be described in the `reason` field\.  
A common cause for this service event being triggered is because of a lack of resources in the cluster to place the task\. For example, not enough CPU or memory capacity on the available container instances or no container instances being available\. Another common cause is when the Amazon ECS container agent is disconnected on the container instance, causing the scheduler to be unable to place the task\.

`SERVICE_TASK_CONFIGURATION_FAILURE`  
The service scheduler is unable to place a task due to a configuration error\. The cause will be described in the `reason` field\.  
A common cause of this service event being triggered is because tags were being applied to the service but the user or role had not opted in to the new Amazon Resource Name \(ARN\) format in the Region\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\. Another common cause is that Amazon ECS was unable to assume the task IAM role provided\.

**Example Service steady state event**  
Service steady state events are delivered in the following format\. For more information about EventBridge parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.  
For a tutorial walkthrough of setting up a simple AWS Lambda function that listens for Amazon ECS service action events and writes them out to a CloudWatch Logs log stream, see [Tutorial: Listening for Amazon ECS CloudWatch Events](ecs_cwet.md)\.  
For a tutorial walkthrough of creating an SNS topic to email you when a service event event occurs, see [Tutorial: Sending Amazon Simple Notification Service alerts for task stopped events](ecs_cwet2.md)\.  

```
{
    "version": "0",
    "id": "af3c496d-f4a8-65d1-70f4-a69d52e9b584",
    "detail-type": "ECS Service Action",
    "source": "aws.ecs",
    "account": "111122223333",
    "time": "2019-11-19T19:27:22Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:ecs:us-west-2:111122223333:service/default/servicetest"
    ],
    "detail": {
        "eventType": "INFO",
        "eventName": "SERVICE_STEADY_STATE",
        "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/default",
        "createdAt": "2019-11-19T19:27:22.695Z"
    }
}
```

**Example Capacity provider steady state event**  
Capacity provider steady state events are delivered in the following format\.  

```
{
    "version": "0",
    "id": "b9baa007-2f33-0eb1-5760-0d02a572d81f",
    "detail-type": "ECS Service Action",
    "source": "aws.ecs",
    "account": "111122223333",
    "time": "2019-11-19T19:37:00Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:ecs:us-west-2:111122223333:service/default/servicetest"
    ],
    "detail": {
        "eventType": "INFO",
        "eventName": "CAPACITY_PROVIDER_STEADY_STATE",
        "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/default",
        "capacityProviderArns": [
            "arn:aws:ecs:us-west-2:111122223333:capacity-provider/ASG-tutorial-capacity-provider"
        ],
        "createdAt": "2019-11-19T19:37:00.807Z"
    }
}
```

**Example Service task start impaired event**  
Service task start impaired events are delivered in the following format\.  

```
{
    "version": "0",
    "id": "57c9506e-9d21-294c-d2fe-e8738da7e67d",
    "detail-type": "ECS Service Action",
    "source": "aws.ecs",
    "account": "111122223333",
    "time": "2019-11-19T19:55:38Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:ecs:us-west-2:111122223333:service/default/servicetest"
    ],
    "detail": {
        "eventType": "WARN",
        "eventName": "SERVICE_TASK_START_IMPAIRED",
        "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/default",
        "createdAt": "2019-11-19T19:55:38.725Z"
    }
}
```

**Example Service task placement failure event**  
Service task placement failure events are delivered in the following format\. For more information about EventBridge parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.  
In the following example, the task was attempting to use the `FARGATE_SPOT` capacity provider but the service scheduler was unable to acquire any Fargate Spot capacity\.  

```
{
    "version": "0",
    "id": "ddca6449-b258-46c0-8653-e0e3a6d0468b",
    "detail-type": "ECS Service Action",
    "source": "aws.ecs",
    "account": "111122223333",
    "time": "2019-11-19T19:55:38Z",
    "region": "us-west-2",
    "resources": [
        "arn:aws:ecs:us-west-2:111122223333:service/default/servicetest"
    ],
    "detail": {
        "eventType": "ERROR",
        "eventName": "SERVICE_TASK_PLACEMENT_FAILURE",
        "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/default",
        "capacityProviderArns": [
            "arn:aws:ecs:us-west-2:111122223333:capacity-provider/FARGATE_SPOT"
        ],
        "reason": "RESOURCE:FARGATE",
        "createdAt": "2019-11-06T19:09:33.087Z"
    }
}
```