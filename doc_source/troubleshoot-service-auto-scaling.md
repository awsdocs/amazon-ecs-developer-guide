# Troubleshooting service auto scaling<a name="troubleshoot-service-auto-scaling"></a>

## Service auto scaling and deployments<a name="auto-scaling-deployment"></a>

As a best practice to prevent scale in processes from behaving like scale out processes, suspend dynamic scaling during deployments\. This will prevent alarms from triggering scale in and scale out processes that depend on the running task count\. Take the following steps to avoid this issue\.

1. Call the `describe-scalable-targets` command, specifying the resource ID of the ECS service associated with the scalable target in Application Auto Scaling \(Example: service/default/sample\-webapp\)\. Record the output\. You will need it when you call the next command\.

1. Call the `register-scalable-target` command, specifying the resource ID, namespace, and scalable dimension\. Specify `true` for both `DynamicScalingInSuspended` and `DynamicScalingOutSuspended`\.

1. After deployment is complete, you can call the `register-scalable-target` command to resume scaling\.

For more information, see [Suspending and Resuming Scaling for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-suspend-resume-scaling.html)\.