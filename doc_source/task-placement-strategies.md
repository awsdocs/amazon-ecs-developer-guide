# Amazon ECS task placement strategies<a name="task-placement-strategies"></a>

A *task placement strategy* is an algorithm for selecting instances for task placement or tasks for termination\. Task placement strategies can be specified when either running a task or creating a new service\. The task placement strategies can be updated for existing services as well\. For more information, see [Amazon ECS task placement](task-placement.md)\.

## Strategy types<a name="strategy-types"></a>

Amazon ECS supports the following task placement strategies:

`binpack`  
Tasks are placed on container instances so as to leave the least amount of unused CPU or memory\. This strategy minimizes the number of container instances in use\.  
When this strategy is used and a scale\-in action is taken, Amazon ECS will terminate tasks based on the amount of resources that will be left on the container instance after the task is terminated\. The container instance that will have the most available resources left after task termination will have that task terminated\.

`random`  
Tasks are placed randomly\.

`spread`  
Tasks are placed evenly based on the specified value\. Accepted values are `instanceId` \(or `host`, which has the same effect\), or any platform or custom attribute that is applied to a container instance, such as `attribute:ecs.availability-zone`\. Service tasks are spread based on the tasks from that service\. Standalone tasks are spread based on the tasks from the same task group\.  
When this strategy is used and a scale\-in action is taken, Amazon ECS will select tasks to terminate that maintains a balance across Availability Zones\. Within an Availability Zone, tasks will be selected at random\.

## Example strategies<a name="strategy-examples"></a>

You can specify task placement strategies with the following actions: [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html), [UpdateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html), and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)\.

The following strategy distributes tasks evenly across Availability Zones\.

```
"placementStrategy": [
    {
        "field": "attribute:ecs.availability-zone",
        "type": "spread"
    }
]
```

The following strategy distributes tasks evenly across all instances\.

```
"placementStrategy": [
    {
        "field": "instanceId",
        "type": "spread"
    }
]
```

The following strategy bin packs tasks based on memory\.

```
"placementStrategy": [
    {
        "field": "memory",
        "type": "binpack"
    }
]
```

The following strategy places tasks randomly\.

```
"placementStrategy": [
    {
        "type": "random"
    }
]
```

The following strategy distributes tasks evenly across Availability Zones and then distributes tasks evenly across the instances within each Availability Zone\.

```
"placementStrategy": [
    {
        "field": "attribute:ecs.availability-zone",
        "type": "spread"
    },
    {
        "field": "instanceId",
        "type": "spread"
    }
]
```

The following strategy distributes tasks evenly across Availability Zones and then bin packs tasks based on memory within each Availability Zone\.

```
"placementStrategy": [
    {
        "field": "attribute:ecs.availability-zone",
        "type": "spread"
    },
    {
        "field": "memory",
        "type": "binpack"
    }
]
```