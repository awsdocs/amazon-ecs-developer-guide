# Amazon ECS troubleshooting<a name="troubleshooting"></a>

You may need to troubleshoot issues with your load balancers, tasks, services, or container instances\. This chapter helps you find diagnostic information from the Amazon ECS container agent, the Docker daemon on the container instance, and the service event log in the Amazon ECS console\.

**Topics**
+ [Checking stopped tasks for errors](stopped-task-errors.md)
+ [CannotPullContainer task errors](task_cannot_pull_image.md)
+ [Service event messages](service-event-messages.md)
+ [Invalid CPU or memory value specified](task-cpu-memory-error.md)
+ [`CannotCreateContainerError: API error (500): devmapper`](CannotCreateContainerError.md)
+ [Troubleshooting service load balancers](troubleshoot-service-load-balancers.md)
+ [Troubleshooting service auto scaling](troubleshoot-service-auto-scaling.md)
+ [Enabling Docker debug output](docker-debug-mode.md)
+ [Amazon ECS Log File Locations](logs.md)
+ [Amazon ECS logs collector](ecs-logs-collector.md)
+ [Agent introspection diagnostics](introspection-diag.md)
+ [Docker diagnostics](docker-diags.md)
+ [AWS Fargate throttling limits](throttling.md)
+ [API failure reasons](api_failures_messages.md)
+ [Troubleshooting IAM Roles for Tasks](troubleshoot-task-iam-roles.md)