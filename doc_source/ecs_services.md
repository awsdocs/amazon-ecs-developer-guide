# Services<a name="ecs_services"></a>

Amazon ECS allows you to run and maintain a specified number \(the "desired count"\) of instances of a task definition simultaneously in an Amazon ECS cluster\. This is called a service\. If any of your tasks should fail or stop for any reason, the Amazon ECS service scheduler launches another instance of your task definition to replace it and maintain the desired count of tasks in the service\. 

In addition to maintaining the desired count of tasks in your service, you can optionally run your service behind a load balancer\. The load balancer distributes traffic across the tasks that are associated with the service\.


+ [Service Concepts](#service_concepts)
+ [Service Definition Parameters](service_definition_parameters.md)
+ [Service Load Balancing](service-load-balancing.md)
+ [Service Auto Scaling](service-auto-scaling.md)
+ [Creating a Service](create-service.md)
+ [Updating a Service](update-service.md)
+ [Deleting a Service](delete-service.md)
+ [Service Throttle Logic](service-throttle-logic.md)

## Service Concepts<a name="service_concepts"></a>

+ If a task in a service stops, the task is killed and a new task is launched\. This process continues until your service reaches the number of desired running tasks\.

+ The service scheduler includes logic that throttles how often tasks are restarted if they repeatedly fail to start\. If a task is stopped without having entered a `RUNNING` state, determined by the task having a `startedAt` time stamp, the service scheduler starts to incrementally slow down the launch attempts and will emit a service event message\. This behavior prevents unnecessary resources from being used for failed tasks, giving you a chance to resolve the issue\. After the service is updated, the service scheduler resumes normal behavior\. For more information, see [Service Throttle Logic](service-throttle-logic.md) and [Service Event Messages](service-event-messages.md)\.

+ You can optionally run your service behind a load balancer\. For more information, see [Service Load Balancing](service-load-balancing.md)\.

+ You can optionally specify a deployment configuration for your service\. During a deployment \(which is triggered by updating the task definition or desired count of a service\), the service scheduler uses the minimum healthy percent and maximum percent parameters to determine the deployment strategy\. For more information, see [Service Definition Parameters](service_definition_parameters.md)\.

+ When the service scheduler launches new tasks or stops running tasks that use the Fargate launch type, it attempts to maintain balance across the Availability Zones in your service\.

+ When the service scheduler launches new tasks using the EC2 launch type, the scheduler uses the following logic:

  + Determine which of the container instances in your cluster can support your service's task definition \(for example, they have the required CPU, memory, ports, and container instance attributes\)\.

  + Determine which container instances satisfy any placement constraints that are defined for the service\.

  + If there is a placement strategy defined, use that strategy to select an instance from the remaining candidates\.

  + If there is no placement strategy defined, balance tasks across the Availability Zones in your cluster with the following logic:

    + Sort the valid container instances by the fewest number of running tasks for this service in the same Availability Zone as the instance\. For example, if zone A has one running service task and zones B and C each have zero, valid container instances in either zone B or C are considered optimal for placement\.

    + Place the new service task on a valid container instance in an optimal Availability Zone \(based on the previous steps\), favoring container instances with the fewest number of running tasks for this service\.

+ When the service scheduler stops running tasks, it attempts to maintain balance across the Availability Zones in your cluster\. For tasks using the EC2 launch type, the scheduler uses the following logic: 

  + If a placement strategy is defined, use that strategy to select which tasks to terminate\. For example, if a service has an Availability Zone spread strategy defined, then a task is selected which leaves the remaining tasks with the best spread\.

  + If no placement strategy is defined, maintain balance across the Availability Zones in your cluster with the following logic:

    + Sort the container instances by the largest number of running tasks for this service in the same Availability Zone as the instance\. For example, if zone A has one running service task and zones B and C each have two, container instances in either zone B or C are considered optimal for termination\.

    + Stop the task on a container instance in an optimal Availability Zone \(based on the previous steps\), favoring container instances with the largest number of running tasks for this service\.