# Container instance draining<a name="container-instance-draining"></a>

**Note**  
You don't need to take any action related to container instance draining when using an Amazon ECS capacity provider\. The capacity provider coordinates container instance draining with the Auto Scaling group\.

There might be times when you need to remove a container instance from your cluster; for example, to perform system updates, update the Docker daemon, or to scale down the cluster capacity\. Amazon ECS provides the ability to transition a container instance to a `DRAINING` status\. This is referred to as *container instance draining*\. When a container instance is set to `DRAINING`, Amazon ECS prevents new tasks from being scheduled for placement on the container instance\. 

## Draining behavior for services<a name="draining-service-behavior"></a>

Any tasks that are part of a service that are in a `PENDING` state are stopped immediately\. If there is available container instance capacity in the cluster, the service scheduler will start replacement tasks\. If there isn't enough container instance capacity, a service event message will be sent indicating the issue\.

Tasks that are part of a service on the container instance that are in a `RUNNING` state are transitioned to a `STOPPED` state\. The service scheduler attempts to replace the tasks according to the service's deployment configuration parameters, `minimumHealthyPercent` and `maximumPercent`\. For more information, see [Service definition parameters](service_definition_parameters.md)\.
+ If `minimumHealthyPercent` is below 100%, the scheduler can ignore `desiredCount` temporarily during task replacement\. For example, `desiredCount` is four tasks, a minimum of 50% allows the scheduler to stop two existing tasks before starting two new tasks\. If the minimum is 100%, the service scheduler can't remove existing tasks until the replacement tasks are considered healthy\. If tasks for services that do not use a load balancer are in the `RUNNING` state, they are considered healthy\. Tasks for services that use a load balancer are considered healthy if they are in the `RUNNING` state and the container instance they are hosted on is reported as healthy by the load balancer\.
+ The `maximumPercent` parameter represents an upper limit on the number of running tasks during task replacement, which enables you to define the replacement batch size\. For example, if `desiredCount` of four tasks, a maximum of 200% starts four new tasks before stopping the four tasks to be drained \(provided that the cluster resources required to do this are available\)\. If the maximum is 100%, then replacement tasks can't start until the draining tasks have stopped\.
**Important**  
If both `minimumHealthyPercent` and `maximumPercent` are 100%, then the service can't remove existing tasks, and also cannot start replacement tasks\. This prevents successful container instance draining and prevents making new deployments\.

## Draining behavior for standalone tasks<a name="draining-standalone-behavior"></a>

Any standalone tasks in the `PENDING` or `RUNNING` state are unaffected; you must wait for them to stop on their own or stop them manually\. The container instance will remain in `DRAINING` status\.

A container instance has completed draining when all tasks running on the instance transition to a `STOPPED` state\. The container instance remains in a `DRAINING` state until it is activated again or deleted\. You can verify the state of the tasks on the container instance by using the [ListTasks](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ListTasks.html) operation with the `containerInstance` parameter to get a list of tasks on the instance followed by a [DescribeTasks](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeTasks.html) operation with the Amazon Resource Name \(ARN\) or ID of each task to verify the task state\.

When you are ready for the container instance to start hosting tasks again, you change the state of the container instance from `DRAINING` to `ACTIVE`\. The Amazon ECS service scheduler will then consider the container instance for task placement again\.

## Draining container instances<a name="drain-instances"></a>

------
#### [ New AWS Management Console ]

**New AWS Management Console**

The following steps can be used to set a container instance to draining using the new AWS Management Console\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose a cluster that hosts your instances\.

1. On the **Cluster : *name*** page, choose the **Infrastructure** tab\. Then, under **Container instances** select the check box for each container instance you want to drain\.

1. Choose **Drain**\.

------
#### [ Classic AWS Management Console ]

**Classic AWS Management Console**

The following steps can be used to set a container instance to draining using the classic AWS Management Console\.

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters** and select the cluster\.

1. Choose the **ECS Instances** tab and select the check box for each container instance you want to drain\.

1. Choose **Actions**, **Drain instances**\.

1. After the instances are processed, choose **Done**\.

1. When you want to activate the container instances again, repeat these same steps but choose **Activate instances** from the **Actions** menu\.

------
#### [ AWS CLI ]

You can use the [UpdateContainerInstancesState](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateContainerInstancesState.html) API action or the [update\-container\-instances\-state](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-container-instances-state.html) command to change the status of a container instance to `DRAINING`\.

------