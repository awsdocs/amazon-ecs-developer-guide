# Amazon ECS Events<a name="ecs_cwe_events"></a>

Amazon ECS sends two types of events to CloudWatch Events: container instance events and task events\. Container instance events are only sent if you are using the EC2 launch type for our tasks\. For tasks using the Fargate launch type you only receive task state events\. Amazon ECS tracks the state of container instances and tasks\. If either resources changes, an event is triggered\. These events are classified as either container instance state change events or task state change events\. These events and their possible causes are described in greater detail in the following sections\.

**Note**  
Amazon ECS may add other event types, sources, and details in the future\. If you are programmatically deserializing event JSON data, make sure that your application is prepared to handle unknown properties to avoid issues if and when these additional properties are added\.

In some cases, multiple events are triggered for the same activity\. For example, when a task is started on a container instance, a task state change event is triggered for the new task\. A container instance state change event is triggered to account for the change in available resources \(such as CPU, memory, and available ports\) on the container instance\. Likewise, if a container instance is terminated, events are triggered for the container instance, the container agent connection status, and every task that was running on the container instance\.

Events contain two `version` fields; one in the main body of the event, and one in the `detail` object of the event\.
+ The version in the main body of the event is set to `0` on all events\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.
+ The version in the `detail` object of the event describes the version of the associated resource\. Each time a resource changes state, this version is incremented\. Because events can be sent multiple times, this field allows you to identify duplicate events \(they have the same version in the `detail` object\)\. If you are replicating your Amazon ECS container instance and task state with CloudWatch Events, you can compare the version of a resource reported by the Amazon ECS APIs with the version reported in CloudWatch Events for the resource \(inside the `detail` object\) to verify that the version in your event stream is current\.

**Topics**
+ [Container Instance State Change Events](#ecs_container_instance_events)
+ [Task State Change Events](#ecs_task_events)

## Container Instance State Change Events<a name="ecs_container_instance_events"></a>

The following scenarios trigger container instance state change events:

You call the `StartTask`, `RunTask`, or `StopTask` API operations \(either directly, or with the AWS Management Console or SDKs\)\.  
Placing or stopping tasks on a container instance modifies the available resources on the container instance \(such as CPU, memory, and available ports\)\.

The Amazon ECS service scheduler starts or stops a task\.  
Placing or stopping tasks on a container instance modifies the available resources on the container instance \(such as CPU, memory, and available ports\)\.

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation with a `STOPPED` status for a task with a desired status of `RUNNING`\.  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes\. If a task that is supposed to be `RUNNING` is transitioned to `STOPPED`, the agent releases the resources that were allocated to the stopped task \(such as CPU, memory, and available ports\)\.

You deregister the container instance with the `DeregisterContainerInstance` API operation \(either directly, or with the AWS Management Console or SDKs\)\.  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\.

A task was stopped when EC2 instance was stopped\.   
When you stop a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\.

The Amazon ECS container agent registers a container instance for the first time\.   
The first time the Amazon ECS container agent registers a container instance \(at launch or when first run manually\), this creates a state change event for the instance\.

The Amazon ECS container agent connects or disconnects from Amazon ECS\.  
When the Amazon ECS container agent connects or disconnects from the Amazon ECS backend, it changes the `agentConnected` status of the container instance\.  
The Amazon ECS container agent periodically disconnects and reconnects \(several times per hour\) as a part of its normal operation, so agent connection events should be expected and they are not an indication that there is an issue with the container agent or your container instance\.

You upgrade the Amazon ECS container agent on an instance\.  
The container instance detail contains an object for the container agent version\. If you upgrade the agent, this version information changes and triggers an event\.

**Example Container Instance State Change Event**  
Container instance state change events are delivered in the following format \(the `detail` section below resembles the [ContainerInstance](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerInstance.html) object that is returned from a [DescribeContainerInstances](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeContainerInstances.html) API operation in the *Amazon Elastic Container Service API Reference*\)\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.  

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

## Task State Change Events<a name="ecs_task_events"></a>

The following scenarios trigger task state change events:

You call the `StartTask`, `RunTask`, or `StopTask` API operations \(either directly, or with the AWS Management Console, AWS CLI, or SDKs\)\.  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\.

The Amazon ECS service scheduler starts or stops a task\.  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\.

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation\.  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes \(for example, from `PENDING` to `RUNNING`, or from `RUNNING` to `STOPPED`\.

You force deregistration of the underlying container instance with the `DeregisterContainerInstance` API operation and the `force` flag \(either directly, or with the AWS Management Console or SDKs\)\.  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\. If tasks are running on the container instance, the `force` flag must be set to allow deregistration\. This stops all tasks on the instance\.

The underlying container instance is stopped or terminated\.   
When you stop or terminate a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\.

A container in the task changes state\.  
The Amazon ECS container agent monitors the state of containers within tasks\. For example, if a container that is running within a task stops, this container state change triggers an event\.

**Example Task State Change Event**  
Task state change events are delivered in the following format \(the `detail` section below resembles the [Task](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Task.html) object that is returned from a [DescribeTasks](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html) API operation in the *Amazon Elastic Container Service API Reference*\)\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.  
For a tutorial walkthrough for setting up a simple AWS Lambda function that listens for Amazon ECS task events and writes them out to a CloudWatch Logs log stream, see [Tutorial: Listening for Amazon ECS CloudWatch Events](ecs_cwet.md)\.  
For a tutorial walkthrough on creating an SNS topic to email you when a task state change event occurs, see [Tutorial: Sending Amazon Simple Notification Service Alerts for Task Stopped Events](ecs_cwet2.md)\.  

```
{
  "version": "0",
  "id": "9bcdac79-b31f-4d3d-9410-fbd727c29fab",
  "detail-type": "ECS Task State Change",
  "source": "aws.ecs",
  "account": "111122223333",
  "time": "2016-12-06T16:41:06Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6f1cebef"
  ],
  "detail": {
    "clusterArn": "arn:aws:ecs:us-east-1:111122223333:cluster/default",
    "containerInstanceArn": "arn:aws:ecs:us-east-1:111122223333:container-instance/b54a2a04-046f-4331-9d74-3f6d7f6ca315",
    "containers": [
      {
        "containerArn": "arn:aws:ecs:us-east-1:111122223333:container/3305bea1-bd16-4217-803d-3e0482170a17",
        "exitCode": 0,
        "lastStatus": "STOPPED",
        "name": "xray",
        "taskArn": "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6f1cebef"
      }
    ],
    "createdAt": "2016-12-06T16:41:05.702Z",
    "desiredStatus": "RUNNING",
    "group": "task-group",
    "lastStatus": "RUNNING",
    "overrides": {
      "containerOverrides": [
        {
          "name": "xray"
        }
      ]
    },
    "startedAt": "2016-12-06T16:41:06.8Z",
    "startedBy": "ecs-svc/9223370556150183303",
    "updatedAt": "2016-12-06T16:41:06.975Z",
    "taskArn": "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6f1cebef",
    "taskDefinitionArn": "arn:aws:ecs:us-east-1:111122223333:task-definition/xray:2",
    "version": 4
  }
}
```