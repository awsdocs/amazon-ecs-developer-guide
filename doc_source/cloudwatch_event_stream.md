# Amazon ECS events and EventBridge<a name="cloudwatch_event_stream"></a>

Amazon EventBridge enables you to automate your AWS services and respond automatically to system events such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near real time\. You can write simple rules to indicate which events are of interest to you and what automated actions to take when an event matches a rule\. The actions that can be automatically triggered include the following:
+ Adding events to log groups in CloudWatch Logs
+ Invoking an AWS Lambda function
+ Invoking Amazon EC2 Run Command
+ Relaying the event to Amazon Kinesis Data Streams
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an Amazon Simple Queue Service \(Amazon SQS\) queue

For more information, see [Getting Started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

You can use Amazon ECS events for EventBridge to receive near real\-time notifications regarding the current state of your Amazon ECS clusters\. If your tasks are using the Fargate launch type, you can see the state of your tasks\. If your tasks are using the EC2 launch type, you can see the state of both the container instances and the current state of all tasks running on those container instances\. For services, you can see events related to the health of your service\.

Using EventBridge, you can build custom schedulers on top of Amazon ECS that are responsible for orchestrating tasks across clusters and monitoring the state of clusters in near real time\. You can eliminate scheduling and monitoring code that continuously polls the Amazon ECS service for status changes and instead handle Amazon ECS state changes asynchronously using any EventBridge target\. Targets might include AWS Lambda, Amazon Simple Queue Service, Amazon Simple Notification Service, or Amazon Kinesis Data Streams\.

An Amazon ECS event stream ensures that every event is delivered at least one time\. If duplicate events are sent, the event provides enough information to identify duplicates\. For more information, see [Handling events](ecs_cwet_handling.md)\.

Events are relatively ordered, so that you can easily tell when an event occurred in relation to other events\.

**Topics**
+ [Amazon ECS events](ecs_cwe_events.md)
+ [Handling events](ecs_cwet_handling.md)