# Tutorial: Sending Amazon Simple Notification Service alerts for task stopped events<a name="ecs_cwet2"></a>

In this tutorial, you configure an Amazon EventBridge event rule that only captures task events where the task has stopped running because one of its essential containers has terminated\. The event sends only task events with a specific `stoppedReason` property to the designated Amazon SNS topic\.

## Prerequisite: Set up a test cluster<a name="cwet2_step_1"></a>

 If you do not have a running cluster to capture events from, follow the steps in [Creating a cluster using the classic console](create_cluster.md) to create one\. At the end of this tutorial, you run a task on this cluster to test that you have configured your Amazon SNS topic and EventBridge rule correctly\. 

## Prerequisite: Configure permissions for Amazon SNS<a name="cwet2_step_1a"></a>

To allow EventBridge to publish to an Amazon SNS topic, use the aws sns get\-topic\-attributes and the aws sns set\-topic\-attributes commands\. 

For information about how to add the permission, see [Amazon SNS permissions](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-use-resource-based.html#eb-sns-permissions) in the *Amazon Simple Notification Service Developer Guide *

Add the following permissions:

```
{
  "Sid": "PublishEventsToMyTopic",
  "Effect": "Allow",
  "Principal": {
     "Service": "events.amazonaws.com"
  },
  "Action": "sns: Publish",
  "Resource": "arn:aws:sns:region:account-id:TaskStoppedAlert",
}
```

## Step 1: Create and subscribe to an Amazon SNS topic<a name="cwet2_step_2"></a>

 For this tutorial, you configure an Amazon SNS topic to serve as an event target for your new event rule\. 

For information about how to create and subscribe to an Amazon SNS topic , see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html#step-create-queue) in the *Amazon Simple Notification Service Developer Guide * and use the following table to determine what options to select\.


| Option | Value | 
| --- | --- | 
|  Type  | Standard | 
| Name |  TaskStoppedAlert  | 
| Protocol | Email | 
| Endpoint |  An email address to which you currently have access  | 

## Step 2: Register an event rule<a name="cwet2_step_3"></a>

 Next, you register an event rule that captures only task\-stopped events for tasks with stopped containers\. 

For information about how to create and subscribe to an Amazon SNS topic , see [Create a rule in Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html) in the *Amazon EventBridge User Guide* and use the following table to determine what options to select\.


| Option | Value | 
| --- | --- | 
|  Rule type  |  Rule with an event pattern  | 
| Event source | AWS events or EventBridge partner events | 
| Event pattern |  Custom pattern \(JSON editor\)  | 
| Event pattern |  <pre>{<br />   "source":[<br />      "aws.ecs"<br />   ],<br />   "detail-type":[<br />      "ECS Task State Change"<br />   ],<br />   "detail":{<br />      "lastStatus":[<br />         "STOPPED"<br />      ],<br />      "stoppedReason":[<br />         "Essential container in task exited"<br />      ]<br />   }<br />}</pre> | 
| Target type |  AWS service  | 
| Target | SNS topic | 
| Topic |  TaskStoppedAlert \(The topic you created in Step 1\)  | 

## Step 3: Test your rule<a name="cwet2_step_4"></a>

Verify that the rule is working by running a task that exits shortly after it starts\. If your event rule is configured correctly, you receive an email message within a few minutes with the event text\. If you have an existing task definition that can satisfy the rule requirements, run a task using it\. If you do not, the following steps will walk you through registering a Fargate task definition and running it that will\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**\.

1. Choose **Create new task definition**, **Create new task definition with JSON**\.

1. In the JSON editor box, edit your JSON file,

   Copy the following into the editor\.

   ```
   {
      "containerDefinitions":[
         {
            "command":[
               "sh",
               "-c",
               "sleep 5"
            ],
            "essential":true,
            "image":"amazonlinux:2",
            "name":"test-sleep"
         }
      ],
      "cpu":"256",
      "executionRoleArn":"arn:aws:iam::012345678910:role/ecsTaskExecutionRole",
      "family":"fargate-task-definition",
      "memory":"512",
      "networkMode":"awsvpc",
      "requiresCompatibilities":[
         "FARGATE"
      ]
   }
   ```

1. Choose **Create**\.

**To run a task from the console**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, select the cluster you created in the prerequisites\.

1. From the **Tasks** tab, choose **Run new task**\.

1. For **Application type**, choose **Task**\.

1. For **Task definition**, choose **fargate\-task\-definition**\.

1. For **Desired tasks**, enter the number of tasks to launch\.

1. Choose **Create**\.