# AWS Fargate capacity providers<a name="fargate-capacity-providers"></a>

With Amazon ECS on AWS Fargate capacity providers, you can use both Fargate and Fargate Spot capacity with your Amazon ECS tasks\. For more information about capacity providers, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\.

With Fargate Spot, you can run interruption tolerant Amazon ECS tasks at a rate that's discounted compared to the Fargate price\. Fargate Spot runs tasks on spare compute capacity\. When AWS needs the capacity back, your tasks are interrupted with a two\-minute warning\. This is described in greater detail in this section\.

## Fargate capacity provider considerations<a name="fargate-capacity-providers-considerations"></a>

Consider the following when using Fargate capacity providers:
+ Windows containers on Fargate don't support the Fargate Spot capacity provider\.
+ Linux tasks with the ARM64 architecture don't support the Fargate Spot capacity provider\. Fargate Spot only supports Linux tasks with the X86\_64 architecture\.
+ You don't need to create Fargate and Fargate Spot capacity providers\. They're available to all accounts\. To use them, all you need to do is associate them with a cluster\.
+ To associate Fargate and Fargate Spot capacity providers to a cluster, you must use the Amazon ECS API or AWS CLI\. You cannot associate them using the console\. For more information, see [Adding Fargate capacity providers to an existing cluster](#fargate-capacity-providers-existing-cluster)\.
+ The Fargate and Fargate Spot capacity providers are reserved and can't be deleted\. However, you can disassociate them from a cluster using the `PutClusterCapacityProviders` API operation\.
+ When a new cluster is created using the Amazon ECS classic console with the **Networking only** cluster template, the `FARGATE` and `FARGATE_SPOT` capacity providers are associated with the new cluster automatically\.
+ If you use Fargate Spot, your task must use platform version 1\.3\.0 or later \(for Linux\)\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.
+ When tasks that use the Fargate and Fargate Spot capacity providers are stopped, the task state change event is sent to Amazon EventBridge\. The stopped reason describes the cause\. For more information, see [Task state change events](ecs_cwe_events.md#ecs_task_events)\.
+ A cluster can contain a mix of Fargate and Auto Scaling group capacity providers\. However, a capacity provider strategy can only contain either Fargate or Auto Scaling group capacity providers, but not both\. For more information, see [Auto Scaling Group Capacity Providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-auto-scaling.html#asg-capacity-providers) in the *Amazon Elastic Container Service Developer Guide*\.

## Handling Fargate Spot termination notices<a name="fargate-capacity-providers-termination"></a>

When tasks using Fargate Spot capacity are stopped due to a Spot interruption, a two\-minute warning is sent before a task is stopped\. The warning is sent as a task state change event to Amazon EventBridge and as a SIGTERM signal to the running task\. If you use Fargate Spot as part of a service, then in this scenario the service scheduler receives the interruption signal and attempts to launch additional tasks on Fargate Spot if there's capacity available\. A service with only one task is interrupted until capacity is available\.

To ensure that your containers exit gracefully before the task stops, the following can be configured:
+ A `stopTimeout` value of `120` seconds or less can be specified in the container definition that the task is using\. The default `stopTimeout` value is 30 seconds\. You can specify a longer `stopTimeout` value to give yourself more time between the moment that the task state change event is received and the point in time when the container is forcefully stopped\. For more information, see [Container timeouts](task_definition_parameters.md#container_definition_timeout)\.
+ The SIGTERM signal must be received from within the container to perform any cleanup actions\. Failure to process this signal results in the task receiving a SIGKILL signal after the configured `stopTimeout` and may result in data loss or corruption\.

The following is a snippet of a task state change event\. This snippet displays the stopped reason and stop code for a Fargate Spot interruption\.

```
{
  "version": "0",
  "id": "9bcdac79-b31f-4d3d-9410-fbd727c29fab",
  "detail-type": "ECS Task State Change",
  "source": "aws.ecs",
  "account": "111122223333",
  "resources": [
    "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6f1cebef"
  ],
  "detail": {
    "clusterArn": "arn:aws:ecs:us-east-1:111122223333:cluster/default",
    "createdAt": "2016-12-06T16:41:05.702Z",
    "desiredStatus": "STOPPED",
    "lastStatus": "RUNNING",
    "stoppedReason": "Your Spot Task was interrupted.",
    "stopCode": "TerminationNotice",
    "taskArn": "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6fEXAMPLE",
    ...
  }
}
```

The following is an event pattern that's used to create an EventBridge rule for Amazon ECS task state change events\. You can optionally specify a cluster in the `detail` field\. Doing so means that you will receive task state change events for that cluster\. For more information, see [Creating an EventBridge Rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

```
{
    "source": [
        "aws.ecs"
    ],
    "detail-type": [
        "ECS Task State Change"
    ],
    "detail": {
        "clusterArn": [
            "arn:aws:ecs:us-west-2:111122223333:cluster/default"
        ]
    }
}
```

## Creating a new cluster that uses Fargate capacity providers<a name="fargate-capacity-providers-create-cluster"></a>

When a new Amazon ECS cluster is created, you can specify one or more capacity providers to associate with the cluster\. The capacity providers are used to define a capacity provider strategy that determines the infrastructure that your tasks run on\.

If you set up using the AWS Management Console and use the **Networking only** cluster template, the `FARGATE` and `FARGATE_SPOT` capacity providers are associated with the cluster automatically\. For more information, see [Creating a cluster using the classic console](create_cluster.md)\.

### To create an Amazon ECS cluster using Fargate capacity providers \(AWS CLI\)<a name="fargate-capacity-providers-create-cluster-cli"></a>

Use the following command to create a new cluster and associate both the Fargate and Fargate Spot capacity providers with it\.
+ [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\)

  ```
  aws ecs create-cluster \
       --cluster-name FargateCluster \
       --capacity-providers FARGATE FARGATE_SPOT \
       --region us-west-2
  ```

## Adding Fargate capacity providers to an existing cluster<a name="fargate-capacity-providers-existing-cluster"></a>

You can update the pool of available capacity providers for an existing Amazon ECS cluster by using the `PutClusterCapacityProviders` API operation\.

Adding either the Fargate or Fargate Spot capacity providers to an existing cluster isn't supported in the AWS Management Console\. You must either create a new Fargate cluster in the console or add the Fargate or Fargate Spot capacity providers to the existing cluster using the Amazon ECS API or AWS CLI\.

### To add the Fargate capacity providers to an existing cluster \(AWS CLI\)<a name="fargate-capacity-providers-create-cluster-cli"></a>

Use the following command to add the Fargate and Fargate Spot capacity providers to an existing cluster\. If the specified cluster has existing capacity providers associated with it, you must specify all existing capacity providers in addition to any new ones you want to add\. Any existing capacity providers that are associated with a cluster that are omitted from a `PutClusterCapacityProviders` API call will be disassociated from the cluster\. You can only disassociate an existing capacity provider from a cluster if it's not being used by any existing tasks\. These same rules apply to the cluster's default capacity provider strategy\. If the cluster has an existing default capacity provider strategy defined, it must be included in the `PutClusterCapacityProviders` API call\. Otherwise, it is overwritten\.
+ [put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html) \(AWS CLI\)

  ```
  aws ecs put-cluster-capacity-providers \
       --cluster FargateCluster \
       --capacity-providers FARGATE FARGATE_SPOT existing_capacity_provider1 existing_capacity_provider2 \
       --default-capacity-provider-strategy existing_default_capacity_provider_strategy \
       --region us-west-2
  ```

## Running tasks using a Fargate capacity provider<a name="fargate-capacity-providers-run-task"></a>

You can run a task or create a service using either the Fargate or Fargate Spot capacity providers by specifying a capacity provider strategy\. If no capacity provider strategy is provided, the cluster's default capacity provider strategy is used\.

Running a task that uses the Fargate or Fargate Spot capacity providers is supported in the AWS Management Console\. You must add the Fargate or Fargate Spot capacity providers to cluster's default capacity provider strategy if using the AWS Management Console\. When using the Amazon ECS API or AWS CLI you can specify either a capacity provider strategy or use the cluster's default capacity provider strategy\.

### To run a task using a Fargate capacity provider \(AWS CLI\)<a name="fargate-capacity-providers-run-task-cli"></a>

Use the following command to run a task using the Fargate and Fargate Spot capacity providers\.
+ [run\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/run-task.html) \(AWS CLI\)

  ```
  aws ecs run-task \
       --capacity-provider-strategy capacityProvider=FARGATE,weight=1 capacityProvider=FARGATE_SPOT,weight=1 \
       --cluster FargateCluster \
       --task-definition task-def-family:revision \
       --network-configuration "awsvpcConfiguration={subnets=[string,string],securityGroups=[string,string],assignPublicIp=string}" \
       --count integer \
       --region us-west-2
  ```

**Note**  
When running standalone tasks using Fargate Spot it's important to note that the task might be interrupted before it can complete and exit\. Therefore, it's important that you code your application to gracefully exit within two minutes when it receives a SIGTERM signal and can be resumed\. For more information, see [Handling Fargate Spot termination notices](#fargate-capacity-providers-termination)\.