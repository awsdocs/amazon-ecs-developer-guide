# AWS Fargate task maintenance<a name="task-maintenance"></a>

AWS is responsible for patching and maintaining the underlying infrastructure for AWS Fargate\. When AWS determines that a security or infrastructure update is needed for an Amazon ECS task hosted on Fargate, the tasks need to be stopped and new tasks launched to replace them\.

Amazon ECS tasks can be categorized as either service tasks or standalone tasks\.

**service task**  
Service tasks are tasks deployed as part of an Amazon ECS service and are overseen by the service scheduler\.

**standalone task**  
Standalone tasks are tasks started by the `RunTask` action of the ECS API, either directly or by an external scheduler such as scheduled tasks \(which are started by Amazon EventBridge\), AWS Batch, or AWS Step Functions\.

For service tasks, AWS stops the task when there is an issue with the underlying host or a security issue found with the platform version revision\. When AWS stops tasks, AWS uses the [minimum healthy percent](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-type-ecs.html) launches a new task in an attempt to maintain the desired count for the service\. By default, the minimum healthy percent of a service is 100 percent, so a new task is started first before a task is stopped\. Service tasks are routinely replaced in the same way when you scale the service or deploy configuration changes or deploy task definition revisions\.

For standalone tasks, AWS sends a task retirement notice to your [AWS Health Dashboard](http://aws.amazon.com/premiumsupport/phd/) when there is an issue with the underlying host or a security issue with the platform version revision that the task is using\. The notice also is sent to the email address associated with the account\. The task retirement notice provides details about the issue, the task retirement date, and what the next steps are\. AWS will stop the task on or after the task retirement date\. For standalone tasks, Amazon ECS doesn't launch a replacement task when a task is stopped\. Therefore, we recommend that customers monitor the state of standalone tasks and if required, implement logic to replace the stopped tasks\. For more information, see [Understanding the task retirement notice](#task-retirement-notice)\.

When a task is stopped in any of the scenarios mentioned here, you can describe the stopped task to retrieve the `stoppedReason` value\. The `stoppedReason` containing a `ECS is performing maintenance on the underlying infrastructure hosting the task` message indicates that the task was stopped due to a task maintenance issue\.

To prepare for the task retirement process, we recommend that you test your application behavior by simulating this scenario\. You can do this by stopping an individual task in your service to test for resiliency\.

## Understanding the task retirement notice<a name="task-retirement-notice"></a>

When a task retirement notice is sent, you're notified by email of the pending retirement\. An email is sent before the event with the task ID and retirement date\. This email is sent to the address that's associated with your account\. This is the same email address that you use to log in to the AWS Management Console\. You can update the contact information for your account on the [Account Settings](https://console.aws.amazon.com/billing/home?#/account) page\.

If you use an email account that you don't check regularly, you can use the [AWS Health Dashboard](http://aws.amazon.com/premiumsupport/phd/) to determine if any of your tasks are scheduled for retirement\. AWS Health notifications can be sent through Amazon EventBridge to archival storage such as Amazon Simple Storage Service, take automated actions such as run an AWS Lambda function, or other notification systems such as Amazon Simple Notification Service\. For more information, see [Monitoring AWS Health events with Amazon EventBridge](https://docs.aws.amazon.com/health/latest/ug/cloudwatch-events-health.html)\. For sample configuration to send notifications to Amazon Chime, Slack, or Microsoft Teams, see the [AWS Health Aware](https://github.com/aws-samples/aws-health-aware) repository on GitHub\.

When a task reaches its scheduled retirement date, it's stopped or terminated by AWS\. This is if it hasn't already been stopped\. For service tasks, the service scheduler launches a new task to replace the retired task, and then stops the task that will be retired\. The service scheduler maintains the service's desired count\. For standalone tasks, they're stopped and you're responsible for launching a replacement\.