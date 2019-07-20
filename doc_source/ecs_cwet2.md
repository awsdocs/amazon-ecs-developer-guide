# Tutorial: Sending Amazon Simple Notification Service Alerts for Task Stopped Events<a name="ecs_cwet2"></a>

In this tutorial, you configure a CloudWatch Events event rule that only captures task events where the task has stopped running because one of its essential containers has terminated\. The event sends only task events with a specific `stoppedReason` property to the designated Amazon SNS topic\.

## Prerequisite: Set Up a Test Cluster<a name="cwet2_step_1"></a>

 If you do not have a running cluster to capture events from, follow the steps in [Getting Started with Amazon ECS](ECS_GetStarted.md) to create one\. At the end of this tutorial, you run a task on this cluster to test that you have configured your Amazon SNS topic and CloudWatch Events event rule correctly\. 

## Step 1: Create and Subscribe to an Amazon SNS Topic<a name="cwet2_step_2"></a>

 For this tutorial, you configure an Amazon SNS topic to serve as an event target for your new event rule\. 

**To create an Amazon SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. Choose **Topics**, **Create new topic**\.

1. On the **Create new topic** window, for **Topic name**, enter **TaskStoppedAlert** and choose **Create topic**\.

1.  On the **Topics** window, select the topic that you just created\. On the **Topic details: TaskStoppedAlert** screen, choose **Create subscription**\. 

1.  On the **Create Subscription** window, for **Protocol**, choose **Email**\. For **Endpoint**, enter an email address to which you currently have access and choose **Create subscription**\. 

1.  Check your email account, and wait to receive a subscription confirmation email message\. When you receive it, choose **Confirm subscription**\. 

## Step 2: Register Event Rule<a name="cwet2_step_3"></a>

 Next, you register an event rule that captures only task\-stopped events for tasks with stopped containers\. 

**To create an event rule**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation pane, choose **Events**, **Create rule**\.

1. Choose **Show advanced options**, **edit**\.

1. For **Build a pattern that selects events for processing by your targets**, replace the existing text with the following text: 

   ```
   {
   "source": [
     "aws.ecs"
   ],
   "detail-type": [
     "ECS Task State Change"
   ],
   "detail": {
     "lastStatus": [
       "STOPPED"
     ],
     "stoppedReason" : [
       "Essential container in task exited"
     ]
   }
   }
   ```

   This code defines a CloudWatch Events event rule that matches any event where the `lastStatus` and `stoppedReason` fields match the indicated values\. For more information about event patterns, see [Events and Event Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch User Guide*\. 

1. For **Targets**, choose **Add target**\. For **Target type**, choose **SNS topic**, and then choose **TaskStoppedAlert**\.

1. Choose **Configure details**\.

1. For **Rule definition**, type a name and description for your rule and then choose **Create rule**\.

## Step 3: Test Your Rule<a name="cwet2_step_4"></a>

To test your rule, you attempt to run a task that exits shortly after it starts\. If your event rule is configured correctly, you receive an email message within a few minutes with the event text\. 

**To test a rule**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose **Task Definitions**, **Create new Task Definition**\.

1. For **Task Definition Name**, type **WordPressFailure** and choose **Add Container**\.

1. For **Container name**, type **Wordpress**, for **Image**, type **wordpress**, and for **Maximum memory \(MB\)**, type **128**\.

1.  Choose **Add**, **Create**\. 

1. On the **Task Definition** screen, choose **Actions**, **Run Task**\.

1. For **Cluster**, choose **default**\. Choose **Run Task**\.

1. On the **Tasks** tab for your cluster, periodically choose the refresh icon until you no longer see your task running\. To verify that your task has stopped, for **Desired task status**, choose **Stopped**\.

1. Check your email to confirm that you have received an email alert for the stopped notification\.