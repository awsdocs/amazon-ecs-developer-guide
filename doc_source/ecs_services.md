# Amazon ECS services<a name="ecs_services"></a>

You can use an Amazon ECS service to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster\. If one of your tasks fails or stops, the Amazon ECS service scheduler launches another instance of your task definition to replace it\. This helps maintain your desired number of tasks in the service\.

You can also optionally run your service behind a load balancer\. The load balancer distributes traffic across the tasks that are associated with the service\.

**Topics**
+ [Service scheduler concepts](#service_scheduler)
+ [Additional service concepts](#service_concepts)
+ [Service definition parameters](service_definition_parameters.md)
+ [Service management in the Amazon ECS console](manage-service.md)
+ [Amazon ECS Deployment types](deployment-types.md)
+ [Service load balancing](service-load-balancing.md)
+ [Service auto scaling](service-auto-scaling.md)
+ [Service Discovery](service-discovery.md)
+ [Service throttle logic](service-throttle-logic.md)

## Service scheduler concepts<a name="service_scheduler"></a>

We recommend that you use the service scheduler for long running stateless services and applications\. The service scheduler ensures that the scheduling strategy that you specify is followed and reschedules tasks when a task fails\. For example, if the underlying infrastructure fails, the service scheduler reschedules a task\. You can use task placement strategies and constraints to customize how the scheduler places and terminates tasks\. If a task in a service stops, the scheduler launches a new task to replace it\. This process continues until your service reaches your desired number of tasks based on the scheduling strategy that the service uses\. The scheduling strategy of the service is also referred to as the *service type*\.

The service scheduler includes logic that throttles how often tasks are restarted if tasks repeatedly fail to start\. If a task is stopped without having entered a `RUNNING` state, the service scheduler starts to slow down the launch attempts and sends out a service event message\. This behavior prevents unnecessary resources from being used for failed tasks before you can resolve the issue\. After the service is updated, the service scheduler resumes normal scheduling behavior\. For more information, see [Service throttle logic](service-throttle-logic.md) and [Service event messages](service-event-messages.md)\.

There are two service scheduler strategies available:
+ `REPLICA`—The replica scheduling strategy places and maintains the desired number of tasks across your cluster\. By default, the service scheduler spreads tasks across Availability Zones\. You can use task placement strategies and constraints to customize task placement decisions\. For more information, see [Replica](#service_scheduler_replica)\.
+ `DAEMON`—The daemon scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints that you specify in your cluster\. The service scheduler evaluates the task placement constraints for running tasks and will stop tasks that do not meet the placement constraints\. When using this strategy, there is no need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\. For more information, see [Daemon](#service_scheduler_daemon)\.
**Note**  
Fargate tasks do not support the `DAEMON` scheduling strategy\.

### Daemon<a name="service_scheduler_daemon"></a>

The *daemon* scheduling strategy deploys exactly one task on each active container instance that meets all of the task placement constraints specified in your cluster\. The service scheduler also evaluates the task placement constraints for running tasks, and stops tasks that don't meet the placement constraints\. When using this strategy, you don't need to specify a desired number of tasks, a task placement strategy, or use Service Auto Scaling policies\.

Amazon ECS reserves container instance compute resources including CPU, memory, and network interfaces for the daemon tasks\. When you launch a daemon service on a cluster with other replica services, Amazon ECS prioritizes the daemon task\. This means that the daemon task is the first task to launch on the instances and the last task to stop\. This strategy ensures that resources aren't used by pending replica tasks and are available for the daemon tasks\.

The daemon service scheduler doesn't place any tasks on instances that have a `DRAINING` status\. If a container instance transitions to a `DRAINING` status, the daemon tasks on it are stopped\. The service scheduler also monitors when new container instances are added to your cluster and adds the daemon tasks to them\.

If a deployment configuration is specified, the maximum percent parameter must be `100`\. The default value for a daemon service for `maximumPercent` is 100%\. The default value for a daemon service for `minimumHealthyPercent` is 0%\.

You must restart the service when you change the placement constraints for the daemon service\. Amazon ECS dynamically updates the resources that are reserved on qualifying instances for the daemon task\. For existing instances, the scheduler tries to place the task on the instance\. 

A new deployment starts when there is a change to the task size or container resource reservation in the task definition\.\. Amazon ECS picks up the updated CPU and memory reservations for the daemon, and then blocks that capacity for the daemon task\.

If there are insufficient resources for either of the above cases, the following happens:
+ The task placement fails\.
+ A CloudWatch event is generated\.
+ Amazon ECS continues to try and schedule the task on the instance by waiting for resources to become available\. 
+ Amazon ECS frees up any reserved instances that no longer meet the placement constraint criteria and stops the corresponding daemon tasks\.

The daemon scheduling strategy can be used in the following cases:
+ Running application containers
+ Running support containers for logging, monitoring and tracing tasks

Tasks using the Fargate launch type or the `CODE_DEPLOY` or `EXTERNAL` deployment controller types don't support the daemon scheduling strategy\.

**Note**  
The daemon service scheduler doesn't support the use of Classic Load Balancers\.

When the service scheduler stops running tasks, it attempts to maintain balance across the Availability Zones in your cluster\. The scheduler uses the following logic: 
+ If a placement strategy is defined, use that strategy to select which tasks to terminate\. For example, if a service has an Availability Zone spread strategy defined, a task is selected that leaves the remaining tasks with the best spread\.
+ If no placement strategy is defined, use the following logic to maintain balance across the Availability Zones in your cluster:
  + Sort the valid container instances\. Give priority to instances that have the largest number of running tasks for this service in their respective Availability Zone\. For example, if zone A has one running service task and zones B and C each have two running service task, container instances in either zone B or C are considered optimal for termination\.
  + Stop the task on a container instance in an optimal Availability Zone based on the previous steps\. Favoring container instances with the largest number of running tasks for this service\.

### Replica<a name="service_scheduler_replica"></a>

The *replica* scheduling strategy places and maintains the desired number of tasks in your cluster\.

For a service that runs tasks on Fargate, when the service scheduler launches new tasks or stops running tasks, the service scheduler attempts to maintain a balance across Availability Zones\. You don't need to specify task placement strategies or restraints\.

When you create a service that runs tasks on EC2 instances, you can optionally specify task placement strategies and constraints to customize task placement decisions\. If no task placement strategies or constraints are specified, then by default the service scheduler spreads the tasks across Availability Zones\. The service scheduler uses the following logic:
+ Determines which of the container instances in your cluster can support your service's task definition \(for example, required CPU, memory, ports, and container instance attributes\)\.
+ Determines which container instances satisfy any placement constraints that are defined for the service\.
+ When there's a defined placement strategy, use that strategy to select an instance from the remaining candidates\.
+ When there's no defined placement strategy, use the following logic to balance tasks across the Availability Zones in your cluster:
  + Sorts the valid container instances\. Gives priority to instances that have the fewest number of running tasks for this service in their respective Availability Zone\. For example, if zone A has one running service task and zones B and C each have zero, valid container instances in either zone B or C are considered optimal for placement\.
  + Places the new service task on a valid container instance in an optimal Availability Zone based on the previous steps\. Favors container instances with the fewest number of running tasks for this service\.

## Additional service concepts<a name="service_concepts"></a>
+ You can optionally run your service behind a load balancer\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ You can optionally specify a deployment configuration for your service\. A deployment is triggered by updating the task definition or desired count of a service\. During a deployment, the service scheduler uses the *minimum healthy percent* and *maximum percent* parameters to determine the deployment strategy\. For more information, see [Service definition parameters](service_definition_parameters.md)\.
+ You can optionally configure your service to use Amazon ECS service discovery\. Service discovery uses the AWS Cloud Map autonaming APIs to manage DNS entries for your service's tasks\. This makes them discoverable from within your VPC\. For more information, see [Service Discovery](service-discovery.md)\.
+ When you delete a service, if there are still running tasks that require cleanup, the service moves from an `ACTIVE` to a `DRAINING` status, and the service is no longer visible in the console or in the `ListServices` API operation\. After all tasks transition to either a `STOPPING` or `STOPPED` status, the service moves from a `DRAINING` to `INACTIVE` status\. You can view services in the `DRAINING` or `INACTIVE` status by using the `DescribeServices` API operation\. However, in the future, `INACTIVE` services might be cleaned up and purged from Amazon ECS record keeping, and `DescribeServices` calls on those services return a `ServiceNotFoundException` error\.