# Services<a name="ecs_services"></a>

Amazon ECS allows you to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster\. This is called a service\. If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it and maintain the desired count of tasks in the service depending on the scheduling strategy used\.

In addition to maintaining the desired count of tasks in your service, you can optionally run your service behind a load balancer\. The load balancer distributes traffic across the tasks that are associated with the service\.

**Topics**
+ [Service Scheduler Concepts](#service_scheduler)
+ [Additional Service Concepts](#service_concepts)
+ [Service Definition Parameters](service_definition_parameters.md)
+ [Amazon ECS Deployment Types](deployment-types.md)
+ [Service Load Balancing](service-load-balancing.md)
+ [Service Auto Scaling](service-auto-scaling.md)
+ [Service Discovery](service-discovery.md)
+ [Creating a Service](create-service.md)
+ [Updating a Service](update-service.md)
+ [Deleting a Service](delete-service.md)
+ [Service Throttle Logic](service-throttle-logic.md)

## Service Scheduler Concepts<a name="service_scheduler"></a>

If a task in a service stops, the task is killed and a new task is launched\. This process continues until your service reaches the number of desired running tasks based on the scheduling strategy that you specified\.

The service scheduler includes logic that throttles how often tasks are restarted if they repeatedly fail to start\. If a task is stopped without having entered a `RUNNING` state, determined by the task having a `startedAt` time stamp, the service scheduler starts to incrementally slow down the launch attempts and emits a service event message\. This behavior prevents unnecessary resources from being used for failed tasks, giving you a chance to resolve the issue\. After the service is updated, the service scheduler resumes normal behavior\. For more information, see [Service Throttle Logic](service-throttle-logic.md) and [Service Event Messages](service-event-messages.md)\.

There are two service scheduler strategies available:
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.

### Daemon<a name="service_scheduler_daemon"></a>

The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints specified in your cluster\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\.

The daemon service scheduler does not place any tasks on instances that have the `DRAINING` status\. If a container instance transitions to `DRAINING`, the daemon tasks on it are stopped\. It also monitors when new container instances are added to your cluster and adds the daemon tasks to them\.

If `deploymentConfiguration` is specified, the maximum percent parameter must be `100`\. The default value for a daemon service for `maximumPercent` is 100%\. The default value for a daemon service for `minimumHealthyPercent` is 0% for the AWS CLI, the AWS SDKs, and the APIs, and 50% for the AWS Management Console\.

Tasks using the Fargate launch type or the `CODE_DEPLOY` or `EXTERNAL` deployment controller types don't support the daemon scheduling strategy\.

**Note**  
The daemon service scheduler does not support the use of Classic Load Balancers\.

### Replica<a name="service_scheduler_replica"></a>

The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\.

When the service scheduler, using the `REPLICA` strategy, launches new tasks or stops running tasks that use the Fargate launch type, it attempts to maintain balance across the Availability Zones in your service\.

When the service scheduler, using the `REPLICA` strategy, launches new tasks using the EC2 launch type, the scheduler uses the following logic:
+ Determine which of the container instances in your cluster can support your service's task definition \(for example, they have the required CPU, memory, ports, and container instance attributes\)\.
+ Determine which container instances satisfy any placement constraints that are defined for the service\.
+ If there is a placement strategy defined, use that strategy to select an instance from the remaining candidates\.
+ If there is no placement strategy defined, balance tasks across the Availability Zones in your cluster with the following logic:
  + Sort the valid container instances, giving priority to instances that have the fewest number of running tasks for this service in their respective Availability Zone\. For example, if zone A has one running service task and zones B and C each have zero, valid container instances in either zone B or C are considered optimal for placement\.
  + Place the new service task on a valid container instance in an optimal Availability Zone \(based on the previous steps\), favoring container instances with the fewest number of running tasks for this service\.

When the service scheduler, using the `REPLICA` strategy, stops running tasks, it attempts to maintain balance across the Availability Zones in your cluster\. For tasks using the EC2 launch type, the scheduler uses the following logic: 
+ If a placement strategy is defined, use that strategy to select which tasks to terminate\. For example, if a service has an Availability Zone spread strategy defined, then a task is selected that leaves the remaining tasks with the best spread\.
+ If no placement strategy is defined, maintain balance across the Availability Zones in your cluster with the following logic:
  + Sort the valid container instances, giving priority to instances that have the largest number of running tasks for this service in their respective Availability Zone\. For example, if zone A has one running service task and zones B and C each have two, container instances in either zone B or C are considered optimal for termination\.
  + Stop the task on a container instance in an optimal Availability Zone \(based on the previous steps\), favoring container instances with the largest number of running tasks for this service\.

## Additional Service Concepts<a name="service_concepts"></a>
+ You can optionally run your service behind a load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\.
+ You can optionally specify a deployment configuration for your service\. During a deployment \(which is triggered by updating the task definition or desired count of a service\), the service scheduler uses the minimum healthy percent and maximum percent parameters to determine the deployment strategy\. For more information, see [Service Definition Parameters](service_definition_parameters.md)\.
+ You can optionally configure your service to use Amazon ECS service discovery\. Service discovery uses Amazon Route 53 auto naming APIs to manage DNS entries for your service's tasks, making them discoverable within your VPC\. For more information, see [Service Discovery](service-discovery.md)\.
+ When you delete a service, if there are still running tasks that require cleanup, the service status moves from `ACTIVE` to `DRAINING`, and the service is no longer visible in the console or in the ListServices API operation\. After all tasks have transitioned to either `STOPPING` or `STOPPED` status, the service status moves from `DRAINING` to `INACTIVE`\. Services in the `DRAINING` or `INACTIVE` status can still be viewed with the DescribeServices API operation\. However, in the future, `INACTIVE` services may be cleaned up and purged from Amazon ECS record keeping, and DescribeServices calls on those services return a `ServiceNotFoundException` error\.