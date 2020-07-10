# Container instance draining<a name="container-instance-draining"></a>

There are times when you might need to remove a container instance from a cluster; for example, to perform system updates, update the Docker daemon, or scale down the cluster size\. Container instance draining enables you to remove a container instance from a cluster without impacting tasks in your cluster\.

When you set a container instance to `DRAINING`, Amazon ECS prevents new tasks from being scheduled for placement on the container instance\. Service tasks on the draining container instance that are in the `PENDING` state are stopped immediately\. If there are container instances in the cluster that are available, replacement service tasks are started on them\. 

Service tasks on the container instance that are in the `RUNNING` state are stopped and replaced according to the service's deployment configuration parameters, `minimumHealthyPercent` and `maximumPercent`\. 
+ If `minimumHealthyPercent` is below 100%, the scheduler can ignore `desiredCount` temporarily during task replacement\. For example, `desiredCount` is four tasks, a minimum of 50% allows the scheduler to stop two existing tasks before starting two new tasks\. If the minimum is 100%, the service scheduler can't remove existing tasks until the replacement tasks are considered healthy\. If tasks for services that do not use a load balancer are in the `RUNNING` state, they are considered healthy\. Tasks for services that use a load balancer are considered healthy if they are in the `RUNNING` state and the container instance they are hosted on is reported as healthy by the load balancer\.
+ The `maximumPercent` parameter represents an upper limit on the number of running tasks during task replacement, which enables you to define the replacement batch size\. For example, if `desiredCount` of four tasks, a maximum of 200% starts four new tasks before stopping the four tasks to be drained \(provided that the cluster resources required to do this are available\)\. If the maximum is 100%, then replacement tasks can't start until the draining tasks have stopped\.

For more information, see [Service definition parameters](service_definition_parameters.md)\.

Any `PENDING` or `RUNNING` tasks that do not belong to a service are unaffected; you must wait for them to finish or stop them manually\.

A container instance has completed draining when there are no more `RUNNING` tasks \(although the state remains as `DRAINING`\)\. You can verify this using the [ListTasks](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ListTasks.html) operation with the `containerInstance` parameter\.

When you change the status of a container instance from `DRAINING` to `ACTIVE`, the Amazon ECS scheduler can schedule tasks on the instance again\.

## Draining instances<a name="drain-instances"></a>

You can use the [UpdateContainerInstancesState](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateContainerInstancesState.html) API action or the [update\-container\-instances\-state](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-container-instances-state.html) command to change the status of a container instance to `DRAINING`\.

The following procedure demonstrates how to set your instance to `DRAINING` using the AWS Management Console\.

**To set your instance to `DRAINING` using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters** and select the cluster\.

1. Choose **ECS Instances** and select the check box for the container instances\.

1. Choose **Actions**, **Drain instances**\.

1. After the instances are processed, choose **Done**\.