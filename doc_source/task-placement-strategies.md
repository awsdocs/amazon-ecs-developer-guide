# Amazon ECS Task Placement Strategies<a name="task-placement-strategies"></a>

A *task placement strategy* is an algorithm for selecting instances for task placement or tasks for termination\. Task placement strategies can be specified when either running a task or creating a new service\. For more information, see [Amazon ECS Task Placement](task-placement.md)\.

## Strategy Types<a name="strategy-types"></a>

Amazon ECS supports the following task placement strategies:

`binpack`  
Place tasks based on the least available amount of CPU or memory\. This minimizes the number of instances in use\.

`random`  
Place tasks randomly\.

`spread`  
Place tasks evenly based on the specified value\. Accepted values are attribute key\-value pairs, `instanceId`, or `host`\. Service tasks are spread based on the tasks from that service\.

## Example Strategies<a name="strategy-examples"></a>

You can specify task placement strategies with the following actions: [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html) and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)\.

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
        "type": "spread”
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
        "type": "spread”
    },
    {
        "field": "memory",
        "type": "binpack"
    }
]
```