# Document History<a name="document_history"></a>

The following table describes the important changes to the documentation since the last release of Amazon ECS\. We also update the documentation frequently to address the feedback that you send us\.

+  **Current API version:** 2014\-11\-13

+  **Latest documentation update:** January 19, 2018


| Feature | API Version | Description | Release Date | 
| --- | --- | --- | --- | 
|  Amazon ECS CLI v1\.3\.0   |  2014\-11\-13  |  New version of the Amazon ECS CLI released, which added the following functionality: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/document_history.html) For more information about the updated ECS CLI syntax, see [Amazon ECS Command Line Reference](ECS_CLI_reference.md)\.  | 19 January 2018 | 
|  Docker 17\.09 support  | 2014\-11\-13 |  Added support for Docker 17\.09\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\.  |  18 January 2018  | 
|  Elastic Load Balancing health check initialization wait period  | 2014\-11\-13 |  Added ability to specify a wait period for health checks\. For more information, see [\(Optional\) Health Check Grace Period](create-service.md#service-health-check-grace-period)\.  |  27 December 2017  | 
|  New service scheduler behavior  |  2014\-11\-13  |  Updated information about the behavior for service tasks that fail to launch\. Documented new service event message that triggers when a service task has consecutive failures\. For more information about this updated behavior, see [Service Concepts](ecs_services.md#service_concepts)\.  | 11 January 2018 | 
|  Task\-level CPU and memory  | 2014\-11\-13 |  Added support for specifying CPU and memory at the task\-level in task definitions\. For more information, see [TaskDefinition](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TaskDefinition.html.html)\.  |  12 December 2017  | 
|  Amazon ECS console AWS CodePipeline integration  | 2014\-11\-13 |  Added Amazon ECS integration with CodePipeline\. CodePipeline supports Amazon ECS as a deployment option to help set up deployment pipelines\. For more information, see [Tutorial: Continuous Deployment with AWS CodePipeline](ecs-cd-pipeline.md)\.  |  12 December 2017  | 
|  Task execution role   | 2014\-11\-13 |  The Amazon ECS container agent makes calls to the Amazon ECS API actions on your behalf, so it requires an IAM policy and role for the service to know that the agent belongs to you\. The following actions are covered by the task execution role: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/document_history.html) For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.  |  7 December 2017  | 
|  Windows containers support GA  | 2014\-11\-13 |  Added support for Windows 2016 containers\. For more information, see [Windows Containers](ECS_Windows.md)\.  |  5 December 2017  | 
|  Amazon ECS CLI v1\.1\.0 with Fargate support  | 2014\-11\-13 |  New version of the Amazon ECS CLI released, which added the following features: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/document_history.html) For more information, see [ECS CLI changelog](https://github.com/aws/amazon-ecs-cli/blob/master/CHANGELOG.md)\.  |  29 November 2017  | 
|  AWS Fargate GA  | 2014\-11\-13 |  Added support for launching Amazon ECS services using the Fargate launch type\. For more information, see [Amazon ECS Launch Types](launch_types.md)\.  |  29 November 2017  | 
|  Amazon ECS name change  | 2014\-11\-13 |  Amazon Elastic Container Service is renamed \(previously Amazon EC2 Container Service\)\.  |  21 November 2017  | 
|  Task networking  | 2014\-11\-13 |  The task networking features provided by the awsvpc network mode give Amazon ECS tasks the same networking properties as Amazon EC2 instances\. When you use the awsvpc network mode in your task definitions, every task that is launched from that task definition gets its own elastic network interface, a primary private IP address, and an internal DNS hostname\. The task networking feature simplifies container networking and gives you more control over how containerized applications communicate with each other and other services within your VPCs\. For more information, see [Task Networking with the `awsvpc` Network Mode](task-networking.md)\.  |  14 November 2017  | 
|  Amazon ECS CLI v1\.0\.0   | 2014\-11\-13 |  New version of the Amazon ECS CLI released, which added the following features: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/document_history.html) For more information, see [ECS CLI changelog](https://github.com/aws/amazon-ecs-cli/blob/master/CHANGELOG.md)\.  |  7 November 2017  | 
|  Amazon ECS container metadata  | 2014\-11\-13 |  Amazon ECS containers are now able to access metadata such as their Docker container or image ID, networking configuration, or Amazon ARNs\. For more information, see [Amazon ECS Container Metadata](container-metadata.md)\.  |  2 November 2017  | 
|  Docker 17\.06 support  | 2014\-11\-13 |  Added support for Docker 17\.06\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\.  |  2 November 2017  | 
|  Support for Docker flags: device and init  | 2014\-11\-13 |  Added support for Docker's device and init features in task definitions using the `LinuxParameters` parameter \(`devices` and `initProcessEnabled`\)\. For more information, see [LinuxParameters](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LinuxParameters.html.html)\.  |  2 November 2017  | 
|  Support for Docker flags: cap\-add and cap\-drop  | 2014\-11\-13 |  Added support for Docker's cap\-add and cap\-drop features in task definitions using the `LinuxParameters` parameter \(`capabilities`\)\. For more information, see [LinuxParameters](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_LinuxParameters.html.html)\.  |  22 September 2017  | 
|  Network Load Balancer support  | 2014\-11\-13 |  Amazon ECS added support for Network Load Balancers in the Amazon ECS console\. For more information, see [Creating a Network Load Balancer](create-network-load-balancer.md)\.  |  7 September 2017  | 
|  RunTask overrides  | 2014\-11\-13 |  Added support for task definition overrides when running a task\. This allows you to run a task while changing a task definition without the need to create a new task definition revision\. For more information, see [Running Tasks](ecs_run_task.md)\.  |  27 June 2017  | 
|  Amazon ECS scheduled tasks  | 2014\-11\-13 |  Added support for scheduling tasks using cron\. For more information, see [Scheduled Tasks \(`cron`\)](scheduled_tasks.md)\.  |  7 June 2017  | 
|  Spot Instances in the Amazon ECS console  | 2014\-11\-13 |  Added support for creating Spot Fleet container instances within the Amazon ECS console\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.  |  6 June 2017  | 
|  Amazon ECS CLI v0\.5\.0   | 2014\-11\-13 |  New version of the Amazon ECS CLI released, which added the following features: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/document_history.html) For more information, see [ECS CLI changelog](https://github.com/aws/amazon-ecs-cli/blob/master/CHANGELOG.md)\.  |  3 April 2017  | 
|  Amazon SNS notification for new Amazon ECS\-optimized AMI releases  | 2014\-11\-13 |  Added ability to subscribe to SNS notifications about new Amazon ECS\-optimized AMI releases\. For more information, see [Subscribing to Amazon ECS–Optimized AMI Update Notifications](ECS-AMI-SubscribeTopic.md)\.  |  23 March 2017  | 
|  Microservices and batch jobs  | 2014\-11\-13 |  Added documentation for two common use cases for Amazon ECS: microservices and batch jobs\. For more information, see [Common Use Cases in Amazon ECS](common_use_cases.md)\.  |  February 2017  | 
|  Container instance draining  | 2014\-11\-13 |  Added support for container instance draining, which provides a method for removing container instances from a cluster\. For more information, see [Container Instance Draining](container-instance-draining.md)\.  |  24 January 2017  | 
|  Docker 1\.12 support  | 2014\-11\-13 |  Added support for Docker 1\.12\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\.  |  24 January 2017  | 
|  New task placement strategies  | 2014\-11\-13 |  Added support for task placement strategies: attribute\-based placement, bin pack, Availability Zone spread, and one per host\. For more information, see [Amazon ECS Task Placement Strategies](task-placement-strategies.md)\.  |  29 December 2016  | 
|  Windows container support in beta  | 2014\-11\-13 |  Added support for Windows 2016 containers \(beta\)\. For more information, see [Windows Containers](ECS_Windows.md)\.  |  20 December 2016  | 
| Blox OSS support | 2014\-11\-13 | Added support for Blox OSS, which allows for custom task schedulers\. For more information, see [Scheduling Amazon ECS Tasks](scheduling_tasks.md)\. | 1 December 2016 | 
| Amazon ECS event stream for CloudWatch Events | 2014\-11\-13 | Amazon ECS now sends container instance and task state changes to CloudWatch Events\. For more information, see [ Amazon ECS Event Stream for CloudWatch EventsCloudWatch Events  You can use Amazon ECS event stream for CloudWatch Events to receive near real\-time notifications regarding the current state of your Amazon ECS clusters\. If your tasks are using the Fargate launch type you can see the state of your tasks\. If your tasks are using the Standard launch type, you can see the state of both the container instances and the current state of all tasks running on those container instances\. Using CloudWatch Events, you can build custom schedulers on top of Amazon ECS that are responsible for orchestrating tasks across clusters, and to monitor the state of clusters in near real time\. You can eliminate scheduling and monitoring code that continuously polls the Amazon ECS service for status changes, and instead handle Amazon ECS state changes asynchronously using any CloudWatch Events target, such as AWS Lambda, Amazon Simple Queue Service, Amazon Simple Notification Service, and Amazon Kinesis Data Streams\. Events from Amazon ECS Event Stream are ensured to be delivered at least one time\. In the event that duplicate events are sent, the event provides enough information to identify duplicates\. For more information, see [Handling Events](ecs_cwet_handling.md) Events are relatively ordered, so that you can easily tell when an event occurred in relation to other events\.   Amazon ECS Events  Amazon ECS sends two types of events to CloudWatch Events: container instance events and task events\. Container instance events are only sent if you are using the Standard launch type for our tasks\. For tasks using the Fargate launch type you will only receive task state events\. Amazon ECS tracks the state of container instances and tasks\. If either of those resources changes, an event is triggered\. These events are classified as either container instance state change events or task state change events\. These events and their possible causes are described in greater detail in the following sections\.  Amazon ECS may add other event types, sources, and details in the future\. If you are programmatically deserializing event JSON data, make sure that your application is prepared to handle unknown properties to avoid issues if and when these additional properties are added\.  In some cases, multiple events are triggered for the same activity\. For example, when a task is started on a container instance, a task state change event is triggered for the new task, and a container instance state change event is triggered to account for the change in available resources \(such as CPU, memory, and available ports\) on the container instance\. Likewise, if a container instance is terminated, events are triggered for the container instance, the container agent connection status, and every task that was running on the container instance\. Events contain two `version` fields; one in the main body of the event, and one in the `detail` object of the event\.   The version in the main body of the event is set to `0` on all events\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.   The version in the `detail` object of the event describes the version of the associated resource\. Each time a resource changes state, this version is incremented\. Because events can be sent multiple times, this field allows you to identify duplicate events \(they will have the same version in the `detail` object\)\. If you are replicating your Amazon ECS container instance and task state with CloudWatch events, you can compare the version of a resource reported by the Amazon ECS APIs with the version reported in CloudWatch events for the resource \(inside the `detail` object\) to verify that the version in your event stream is current\.     Container Instance State Change Events   The following scenarios trigger container instance state change events:  

You call the `StartTask`, `RunTask`, or `StopTask` API operations \(either directly, or with the AWS Management Console or SDKs\)  
Placing or stopping tasks on a container instance modifies the available resources on the container instance \(such as CPU, memory, and available ports\)\. 

The Amazon ECS service scheduler starts or stops a task  
Placing or stopping tasks on a container instance modifies the available resources on the container instance \(such as CPU, memory, and available ports\)\. 

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation with a `STOPPED` status for a task with a desired status of `RUNNING`  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes\. If a task that is supposed to be `RUNNING` is transitioned to `STOPPED`, the agent releases the resources that were allocated to the stopped task \(such as CPU, memory, and available ports\)\. 

You deregister the container instance with the `DeregisterContainerInstance` API operation \(either directly, or with the AWS Management Console or SDKs\)  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\. 

A task was stopped when EC2 instance was stopped   
When you stop a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\. 

The Amazon ECS container agent registers a container instance for the first time   
The first time the Amazon ECS container agent registers a container instance \(at launch or when first run manually\), this creates a state change event for the instance\. 

The Amazon ECS container agent connects or disconnects from Amazon ECS  
When the Amazon ECS container agent connects or disconnects from the Amazon ECS back end, it changes the `agentConnected` status of the container instance\.  
The Amazon ECS container agent periodically disconnects and reconnects \(several times per hour\) as a part of its normal operation, so agent connection events should be expected and they are not an indication that there is an issue with the container agent or your container instance\. 

You upgrade the Amazon ECS container agent on an instance  
The container instance detail contains an object for the container agent version\. If you upgrade the agent, this version information changes and triggers an event\.  

**Example Container Instance State Change Event**  
Container instance state change events are delivered in the following format \(the `detail` section below resembles the [ContainerInstance](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerInstance.html) object that is returned from a [DescribeContainerInstances](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeContainerInstances.html) API operation in the *Amazon Elastic Container Service API Reference*\)\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.  

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
```   Task State Change Events   The following scenarios trigger task state change events:  

You call the `StartTask`, `RunTask`, or `StopTask` API operations \(either directly, or with the AWS Management Console or SDKs\)  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\. 

The Amazon ECS service scheduler starts or stops a task  
Starting or stopping tasks creates new task resources or modifies the state of existing task resources\. 

The Amazon ECS container agent calls the `SubmitTaskStateChange` API operation  
The Amazon ECS container agent monitors the state of tasks on your container instances, and it reports any state changes \(for example, from `PENDING` to `RUNNING`, or from `RUNNING` to `STOPPED`\.   

You force deregistration of the underlying container instance with the `DeregisterContainerInstance` API operation and the `force` flag \(either directly, or with the AWS Management Console or SDKs\)  
Deregistering a container instance changes the status of the container instance and the connection status of the Amazon ECS container agent\. If tasks are running on the container instance, the `force` flag must be set to allow deregistration\. This stops all tasks on the instance\. 

The underlying container instance is stopped or terminated   
When you stop or terminate a container instance, the tasks that are running on it are transitioned to the `STOPPED` status\. 

A container in the task changes state  
The Amazon ECS container agent monitors the state of containers within tasks\. For example, if a container that is running within a task stops, this container state change triggers an event\.  

**Example Task State Change Event**  
Task state change events are delivered in the following format \(the `detail` section below resembles the [Task](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Task.html) object that is returned from a [DescribeTasks](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html) API operation in the *Amazon Elastic Container Service API Reference*\)\. For more information about CloudWatch Events parameters, see [Events and Event Patterns](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.  

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
```    Handling Events  Amazon ECS sends events on an "at least once" basis\. This means you may receive more than a single copy of a given event\. Additionally, events may not be delivered to your event listeners in the order in which the events occurred\. To enable proper ordering of events, the `detail` section of each event contains a `version` property\. Events with a higher version property number should be treated as occurring later than events with lower version numbers\. Events with matching version numbers can be treated as duplicates\.  Example: Handling Events in an AWS Lambda Function  The following example shows a Lambda function written in Python 2\.7 that captures both task and container instance state change events, and saves them to one of two Amazon DynamoDB tables:   *ECSCtrInstanceState*: Stores the latest state for a container instance\. The table ID is the `containerInstanceArn` value of the container instance\.   *ECSTaskState*: Stores the latest state for a task\. The table ID is the `taskArn` value of the task\.   

```
import json
import boto3

def lambda_handler(event, context):
    id_name = ""
    new_record = {}

    # For debugging so you can see raw event format.
    print('Here is the event:')
    print(json.dumps(event))

    if event["source"] != "aws.ecs":
       raise ValueError("Function only supports input from events with a source type of: aws.ecs")

    # Switch on task/container events.
    table_name = ""
    if event["detail-type"] == "ECS Task State Change":
        table_name = "ECSTaskState"
        id_name = "taskArn"
        event_id = event["detail"]["taskArn"]
    elif event["detail-type"] == "ECS Container Instance State Change":
        table_name = "ECSCtrInstanceState"
        id_name =  "containerInstanceArn"
        event_id = event["detail"]["containerInstanceArn"]
    else:
        raise ValueError("detail-type for event is not a supported type. Exiting without saving event.")

    new_record["cw_version"] = event["version"]
    new_record.update(event["detail"])

    # "status" is a reserved word in DDB, but it appears in containerPort
    # state change messages.
    if "status" in event:
        new_record["current_status"] = event["status"]
        new_record.pop("status")


    # Look first to see if you have received a newer version of an event ID.
    # If the version is OLDER than what you have on file, do not process it.
    # Otherwise, update the associated record with this latest information.
    print("Looking for recent event with same ID...")
    dynamodb = boto3.resource("dynamodb", region_name="us-east-1")
    table = dynamodb.Table(table_name)
    saved_event = table.get_item(
        Key={
            id_name : event_id
        }
    )
    if "Item" in saved_event:
        # Compare events and reconcile.
        print("EXISTING EVENT DETECTED: Id " + event_id + " - reconciling")
        if saved_event["Item"]["version"] < event["detail"]["version"]:
            print("Received event is more recent version than stored event - updating")
            table.put_item(
                Item=new_record
            )
        else:
            print("Received event is more recent version than stored event - ignoring")
    else:
        print("Saving new event - ID " + event_id)

        table.put_item(
            Item=new_record
        )
```    Tutorial: Listening for Amazon ECS CloudWatch Events  In this tutorial, you set up a simple AWS Lambda function that listens for Amazon ECS task events and writes them out to a CloudWatch Logs log stream\.  Prerequisite: Set Up a Test Cluster  If you do not have a running cluster to capture events from, follow the steps in [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md) to create one\. At the end of this tutorial, you run a task on this cluster to test that you have configured your Lambda function correctly\.    Step 1: Create the Lambda Function   In this procedure, you will create a simple Lambda function to serve as a target for Amazon ECS event stream messages\.   Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.  Choose **Create a function**\.    On the **Author from scratch** screen, do the following:   choose a **Name** for the function\.   For **Runtime**, choose **Python 2\.7**\.   For **Role**, choose **Create a custom role**\. A new window pops up enabling you to create a new role for your Lambda function\.   On the **AWS Lambda requires access to your resources** screen, accept the defaults and choose **Allow**\.     Choose **Create function**\.   In the **Function code** section, edit the sample code to match the following example: 

   ```
   import json
   
   def lambda_handler(event, context):
       if event["source"] != "aws.ecs":
          raise ValueError("Function only supports input from events with a source type of: aws.ecs")
          
       print('Here is the event:')
       print(json.dumps(event))
   ``` This is a simple Python 2\.7 function that prints the event sent by Amazon ECS\. If everything is configured correctly, at the end of this tutorial, you see the event details appear in the CloudWatch Logs log stream associated with this Lambda function\.   In the **Function code** section, edit the value of **Handler** to be **eventstream\-handler\.lambda\_handler**\.   Choose **Save**\.     Step 2: Register Event Rule   Next, you create a CloudWatch Events event rule that captures task events coming from your Amazon ECS clusters\. This rule captures all events coming from all clusters within the account where it is defined\. The task messages themselves contain information about the event source, including the cluster on which it resides, that you can use to filter and sort events programmatically\.   When you use the AWS Management Console to create an event rule, the console automatically adds the IAM permissions necessary to grant CloudWatch Events permission to call your Lambda function\. If you are creating an event rule using the AWS CLI, you need to grant this permission explicitly\. For more information, see [Events and Event Patterns](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.  To route events to your Lambda function Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.  On the navigation pane, choose **Events**, **Create rule**\.   For **Event Source**, choose **ECS** as the event source\. By default, the rule applies to all Amazon ECS events for all of your Amazon ECS groups\. Alternatively, you can select specific events or a specific Amazon ECS group\.   For **Targets**, choose **Add target**, for **Target type**, choose **Lambda function**, and then select your Lambda function\.   Choose **Configure details**\.   For **Rule definition**, type a name and description for your rule and choose **Create rule**\.     Step 3: Test Your Rule   Finally, you create a CloudWatch Events event rule that captures task events coming from your Amazon ECS clusters\. This rule captures all events coming from all clusters within the account where it is defined\. The task messages themselves contain information about the event source, including the cluster on which it resides, that you can use to filter and sort events programmatically\.  To test your rule Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  Choose **Clusters**, **default**\.   On the **Cluster : default** screen, choose **Tasks**, **Run new Task**\.   For **Task Definition**, select the latest version of **console\-sample\-app\-static** and choose **Run Task**\.  Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.  On the navigation pane, choose **Logs** and select the log group for your Lambda function \(for example, **/aws/lambda/***my\-function*\)\.   Select a log stream to view the event data\.       Tutorial: Sending Amazon Simple Notification Service Alerts for Task Stopped Events  In this tutorial, you configure a CloudWatch Events event rule that only captures task events where the task has stopped running because one of its essential containers has terminated\. The event sends only task events with a specific `stoppedReason` property to the designated Amazon SNS topic\.  Prerequisite: Set Up a Test Cluster   If you do not have a running cluster to capture events from, follow the steps in [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md) to create one\. At the end of this tutorial, you run a task on this cluster to test that you have configured your Amazon SNS topic and CloudWatch Events event rule correctly\.    Step 1: Create and Subscribe to an Amazon SNS Topic   For this tutorial, you configure an Amazon SNS topic to serve as an event target for your new event rule\.  To create a Amazon SNS topic Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.  Choose **Topics**, **Create new topic**\.   On the **Create new topic** window, for **Topic name**, enter **TaskStoppedAlert** and choose **Create topic**\.    On the **Topics** window, select the topic that you just created\. On the **Topic details: TaskStoppedAlert** screen, choose **Create subscription**\.     On the **Create Subscription** window, for **Protocol**, choose **Email**\. For **Endpoint**, enter an email address to which you currently have access and choose **Create subscription**\.     Check your email account, and wait to receive a subscription confirmation email message\. When you receive it, choose **Confirm subscription**\.      Step 2: Register Event Rule   Next, you register an event rule that captures only task\-stopped events for tasks with stopped containers\.  To create an event rule Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.  On the navigation pane, choose **Events**, **Create rule**\.   Choose **Show advanced options**, **edit**\.   For **Build a pattern that selects events for processing by your targets**, replace the existing text with the following text:  

   ```
   {
   "source": [
     "aws.ecs"
   ],
   "detail-type": [
     "ECS Task State Change"
   ],
   "detail": {
     "lastStatus": [
       "STOPPED"
     ],
     "stoppedReason" : [
       "Essential container in task exited"
     ]
   }
   }
   ``` This code defines a CloudWatch Events event rule that matches any event where the `lastStatus` and `stoppedReason` fields match the indicated values\. For more information about event patterns, see [Events and Event Patterns](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch User Guide*\.    For **Targets**, choose **Add target**\. For **Target type**, choose **SNS topic**, and then choose **TaskStoppedAlert**\.   Choose **Configure details**\.   For **Rule definition**, type a name and description for your rule and then choose **Create rule**\.     Step 3: Test Your Rule   To test your rule, you attempt to run a task that exits shortly after it starts\. If your event rule is configured correctly, you receive an email message within a few minutes with the event text\.  To test a rule Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  Choose **Task Definitions**, **Create new Task Definition**\.   For **Task Definition Name**, type **WordPressFailure** and choose **Add Container**\.   For **Container name**, type **Wordpress**, for **Image**, type **wordpress**, and for **Maximum memory \(MB\)**, type **128**\.    Choose **Add**, **Create**\.    On the **Task Definition** screen, choose **Actions**, **Run Task**\.   For **Cluster**, choose **default** and then **Run Task**\.   On the **Tasks** tab for your cluster, periodically choose the refresh icon until you no longer see your task running\. For **Desired task status**, choose **Stopped** to verify that your task has stopped\.   Check your email to confirm that you have received an email alert for the stopped notification\.     ](cloudwatch_event_stream.md)\. | 21 November 2016 | 
| Amazon ECS container logging to CloudWatch Logs | 2014\-11\-13 | Added support for the awslogs driver to send container log streams to CloudWatch Logs\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\. | 12 September 2016 | 
| Amazon ECS services with Elastic Load Balancing support for dynamic ports | 2014\-11\-13 | Added support for a load balancer to support multiple instance:port combinations per listener, which increases flexibility for containers\. Now you can let Docker dynamically define the container's host port and the ECS scheduler registers the instance:port with the load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\. | 11 August 2016 | 
| IAM roles for Amazon ECS tasks | 2014\-11\-13 | Added support for associating IAM roles with a task\. This provides finer\-grained permissions to containers as opposed to a single role for an entire container instance\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\. | 13 July 2016 | 
| Amazon ECS CLI support for Docker Compose v2 format | 2014\-11\-13 | The Amazon ECS CLI added support for Docker Compose v2 format\. For more information, see [ecs\-cli compose](cmd-ecs-cli-compose.md)\. | 8 July 2016 | 
| Docker 1\.11 support | 2014\-11\-13 | Added support for Docker 1\.11\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\. | 31 May 2016 | 
| Task automatic scaling | 2014\-11\-13 | Amazon ECS added support for automatically scaling your tasks run by a service\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\. | 18 May 2016 | 
| Task definition filtering on task family | 2014\-11\-13 | Added support for filtering a list of task definition based on the task definition family\. For more information, see [ListTaskDefinitions](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ListTaskDefinitions.html)\. | 17 May 2016 | 
| Docker container and Amazon ECS agent logging | 2014\-11\-13 | Amazon ECS added ability to send ECS agent and Docker container logs from container instances to CloudWatch Logs to simplify troubleshooting issues\. | 5 May 2016 | 
| Amazon ECS CLI v0\.3 released | 2014\-11\-13 | New version of the Amazon ECS CLI released, which added support for service creation with a load balancer\. | 11 April 2016 | 
| ECS\-optimized AMI now supports Amazon Linux 2016\.03\. | 2014\-11\-13 | The ECS\-optimized AMI added support for Amazon Linux 2016\.03\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\. | 5 April 2016 | 
| Docker 1\.9 support | 2014\-11\-13 | Added support for Docker 1\.9\. For more information, see [Amazon ECS\-Optimized AMI](ecs-optimized_AMI.md)\. | 22 December 2015 | 
| CloudWatch metrics for cluster CPU and memory reservation | 2014\-11\-13 | Amazon ECS added custom CloudWatch metrics for CPU and memory reservation\. | 22 December 2015 | 
| Amazon ECR  | 2014\-11\-13 | Added the new Amazon ECR service to the console, which added support for storing images that are controlled by resource\-level permissions associated with Docker Hub or IAM users\. Available in all AWS Regions, images are automatically replicated and cached globally so that starting hundreds of containers is as fast as a single container\. | 21 December 2015 | 
| New Amazon ECS first\-run experience | 2014\-11\-13 | The Amazon ECS console first\-run experience added zero\-click role creation\. | 23 November 2015 | 
| Task placement across Availability Zones | 2014\-11\-13 | The Amazon ECS service scheduler added support for task placement across Availability Zones\.  | 8 October 2015 | 
| Amazon ECS CLI with support for Docker Compose | 2014\-11\-13 | The Amazon ECS CLI added support for Docker Compose\. | 8 October 2015 | 
| CloudWatch metrics for Amazon ECS clusters and services | 2014\-11\-13 | Amazon ECS added custom CloudWatch metrics for CPU and memory utilization for each container instance, service, and task definition family in a cluster\. These new metrics can be used to scale container instances in a cluster using Auto Scaling groups or to create custom CloudWatch alarms\. | 17 August 2015 | 
| UDP port support | 2014\-11\-13 | Added support for UDP ports in task definitions\. | 7 July 2015 | 
| Environment variable overrides | 2014\-11\-13 | Added support for deregisterTaskDefinition and environment variable overrides for runTask\. | 18 June 2015 | 
| Automated Amazon ECS agent updates | 2014\-11\-13 | Added ability to see the ECS agent version that is running on a container instance\. Also able to update the ECS agent from the AWS Management Console, AWS CLI, and SDK\. | 11 June 2015 | 
| Amazon ECS service scheduler and Elastic Load Balancing integration | 2014\-11\-13 |  Added ability to define a service and associate that service with an Elastic Load Balancing load balancer\.  | 9 April 2015 | 
|  Amazon ECS GA  |  2014\-11\-13  |  Amazon ECS general availability in the IAD, PDX, NRT, and DUB regions\.  |  9 April 2015  | 