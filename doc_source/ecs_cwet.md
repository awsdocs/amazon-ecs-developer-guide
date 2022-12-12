# Tutorial: Listening for Amazon ECS CloudWatch Events<a name="ecs_cwet"></a>

In this tutorial, you set up a simple AWS Lambda function that listens for Amazon ECS task events and writes them out to a CloudWatch Logs log stream\.

## Prerequisite: Set up a test cluster<a name="cwet_step_1"></a>

If you do not have a running cluster to capture events from, follow the steps in [Creating a cluster for the Fargate launch type using the new console](create-cluster-console-v2.md) to create one\. At the end of this tutorial, you run a task on this cluster to test that you have configured your Lambda function correctly\. 

## Step 1: Create the Lambda function<a name="cwet_step_2"></a>

In this procedure, you create a simple Lambda function to serve as a target for Amazon ECS event stream messages\. 

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\. 

1. On the **Author from scratch** screen, do the following:

   1. For **Name**, enter a value\. 

   1. For **Runtime**, choose your version of Python, for example, **Python 3\.9**\.

   1. For **Role**, choose **Create a new role with basic Lambda permissions**\.

1. Choose **Create function**\.

1. In the **Function code** section, edit the sample code to match the following example:

   ```
   import json
   
   def lambda_handler(event, context):
       if event["source"] != "aws.ecs":
          raise ValueError("Function only supports input from events with a source type of: aws.ecs")
          
       print('Here is the event:')
       print(json.dumps(event))
   ```

   This is a simple Python 3\.9 function that prints the event sent by Amazon ECS\. If everything is configured correctly, at the end of this tutorial, you see that the event details appear in the CloudWatch Logs log stream associated with this Lambda function\.

1. Choose **Save**\.

## Step 2: Register an event rule<a name="cwet_step_3"></a>

 Next, you create a CloudWatch Events event rule that captures task events coming from your Amazon ECS clusters\. This rule captures all events coming from all clusters within the account where it is defined\. The task messages themselves contain information about the event source, including the cluster on which it resides, that you can use to filter and sort events programmatically\. 

**Note**  
When you use the AWS Management Console to create an event rule, the console automatically adds the IAM permissions necessary to grant CloudWatch Events permission to call your Lambda function\. If you are creating an event rule using the AWS CLI, you need to grant this permission explicitly\. For more information, see [Events and Event Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) in the *Amazon CloudWatch Events User Guide*\.

**To route events to your Lambda function**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation pane, choose **Events**, **Rules**, **Create rule**\.

1. For **Event Source**, choose **ECS** as the event source\. By default, the rule applies to all Amazon ECS events for all of your Amazon ECS groups\. Alternatively, you can select specific events or a specific Amazon ECS group\.

1. For **Targets**, choose **Add target**, for **Target type**, choose **Lambda function**, and then select your Lambda function\.

1. Choose **Configure details**\.

1. For **Rule definition**, type a name and description for your rule and choose **Create rule**\.

## Step 3: Create a task definition<a name="cwet_step_task-def"></a>

Create a task definition\.

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task Definitions**\.

1. Choose **Create new Task Definition**, **Create new revision with JSON**\.

1. Copy and paste the following example task definition into the box and then choose **Save**\.

   ```
   {
       "containerDefinitions": [
           {
               "entryPoint": [
                   "sh",
                   "-c"
               ],
               "portMappings": [
                   {
                       "hostPort": 80,
                       "protocol": "tcp",
                       "containerPort": 80
                   }
               ],
               "command": [
                   "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
               ],
               "cpu": 10,
               "memory": 300,
               "image": "httpd:2.4",
               "name": "simple-app"
           }
       ],
       "family": "console-sample-app-static"
   }
   ```

1. Choose **Create**\.

## Step 4: Test your rule<a name="cwet_step_4"></a>

 Finally, you create a CloudWatch Events event rule that captures task events coming from your Amazon ECS clusters\. This rule captures all events coming from all clusters within the account where it is defined\. The task messages themselves contain information about the event source, including the cluster on which it resides, that you can use to filter and sort events programmatically\. 

**To test your rule**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. Choose **Task definitions**\.

1. Choose **console\-sample\-app\-static**, and then choose **Deploy**, **Run new task**\.

1. For **Cluster**, choose default, and then choose **Deploy**\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation pane, choose **Logs** and select the log group for your Lambda function \(for example, **/aws/lambda/***my\-function*\)\.

1. Select a log stream to view the event data\. 