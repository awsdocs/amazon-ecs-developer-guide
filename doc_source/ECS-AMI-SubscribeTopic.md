# Subscribing to Amazon ECS–Optimized AMI Update Notifications<a name="ECS-AMI-SubscribeTopic"></a>

The Amazon ECS\-optimized AMI receives regular updates for agent changes, Docker version updates, and Linux kernel security updates\. You can subscribe to the AMI update Amazon SNS topic to receive notifications when a new Amazon ECS–optimized AMI is available\. Notifications are available in all formats that Amazon SNS supports\. 

**Note**  
Your user account must have `sns::subscribe` IAM permissions to subscribe to an SNS topic\.

You can subscribe an Amazon SQS queue to this notification topic, but you must use a topic ARN that is in the same region\. For more information, see [Tutorial: Subscribing an Amazon SQS Queue to an Amazon SNS Topic]() in the *Amazon Simple Queue Service Developer Guide*\.

You can also use an AWS Lambda function to trigger events when notifications are received\. For more information, see [Invoking Lambda functions using Amazon SNS notifications](http://docs.aws.amazon.com/sns/latest/dg/sns-lambda.html) in the *Amazon Simple Notification Service Developer Guide*\.

The Amazon SNS topic ARNs for each region are shown below\.


| AWS Region | Amazon SNS Topic ARN | 
| --- | --- | 
| us\-east\-1 | arn:aws:sns:us\-east\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| us\-east\-2 | arn:aws:sns:us\-east\-2:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| us\-west\-1 | arn:aws:sns:us\-west\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| us\-west\-2 | arn:aws:sns:us\-west\-2:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ca\-central\-1 | arn:aws:sns:ca\-central\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| eu\-west\-1 | arn:aws:sns:eu\-west\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| eu\-west\-2 | arn:aws:sns:eu\-west\-2:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| eu\-west\-3 | arn:aws:sns:eu\-west\-3:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| eu\-central\-1 | arn:aws:sns:eu\-central\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ap\-northeast\-1 | arn:aws:sns:ap\-northeast\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ap\-northeast\-2 | arn:aws:sns:ap\-northeast\-2:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ap\-southeast\-1 | arn:aws:sns:ap\-southeast\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ap\-southeast\-2 | arn:aws:sns:ap\-southeast\-2:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| ap\-south\-1 | arn:aws:sns:ap\-south\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 
| sa\-east\-1 | arn:aws:sns:sa\-east\-1:177427601217:ecs\-optimized\-amazon\-ami\-update | 

**To subscribe to AMI update notification emails in the AWS Management Console**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. In the region list, choose the same region as the topic ARN to which to subscribe\. This example uses the `us-west-2` region\.

1. Choose **Subscriptions** in the left navigation, then **Create subscription**\.

1. In the **Create Subscription** dialog box, for **Topic ARN**, paste the Amazon ECS\-optimized AMI update topic ARN: `arn:aws:sns:us-west-2:177427601217:ecs-optimized-amazon-ami-update`\. 

1. For **Protocol**, choose **Email**\. For **Endpoint**, type an email address you can use to receive the notification\.

1. Choose **Create subscription**\.

1. In your email application, open the message from AWS Notifications and open the link to confirm your subscription\.

   Your web browser displays a confirmation response from Amazon SNS\.

**To subscribe to AMI update notification emails with the AWS CLI**

1. Run the following command with the AWS CLI:

   ```
   aws sns --region us-west-2 subscribe --topic-arn arn:aws:sns:us-west-2:177427601217:ecs-optimized-amazon-ami-update --protocol email --notification-endpoint your_email@your_domain.com
   ```

1. In your email application, open the message from AWS Notifications and open the link to confirm your subscription\.

   Your web browser displays a confirmation response from Amazon SNS\.

## Amazon SNS Message Format<a name="ECS-AMI-Notification-format"></a>

An example AMI update notification message is shown below:

```
{
  "ECSAgent": {
    "ReleaseVersion": "1.14.1"
  },
  "ECSAmis": [
    {
      "ReleaseVersion": "2016.09.g",
      "AgentVersion": "1.14.1",
      "ReleaseNotes": "This AMI includes the latest ECS agent 2016.09.g",
      "OsType": "linux",
      "OperatingSystemName": "Amazon Linux",
      "Regions": {
        "ap-northeast-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-f63f6f91"
        },
        "ap-southeast-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-b4ae1dd7"
        },
        "ap-southeast-2": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-fbe9eb98"
        },
        "ca-central-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-ee58e58a"
        },
        "eu-central-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-085e8a67"
        },
        "eu-west-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-95f8d2f3"
        },
        "eu-west-2": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-bf9481db"
        },
        "us-east-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-275ffe31"
        },
        "us-east-2": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-62745007"
        },
        "us-west-1": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-689bc208"
        },
        "us-west-2": {
          "Name": "amzn-ami-2016.09.g-amazon-ecs-optimized",
          "ImageId": "ami-62d35c02"
        }
      }
    }
  ]
}
```