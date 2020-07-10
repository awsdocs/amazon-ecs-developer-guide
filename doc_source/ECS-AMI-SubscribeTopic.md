# Subscribing to Amazon ECS\-optimized AMI update notifications<a name="ECS-AMI-SubscribeTopic"></a>

**Important**  
For the Linux variants of the Amazon ECS\-optimized AMI, the Amazon SNS alert is only sent when there is a new Amazon ECS\-optimized Amazon Linux AMI deployed\. Generally when a new Amazon ECS\-optimized Amazon Linux AMI is deployed, a new AMI for each of the other Linux variants of the Amazon ECS\-optimized AMI are deployed as well although there are not separate Amazon SNS alerts for them\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.

The Amazon ECS\-optimized AMIs receive regular updates for Amazon ECS container agent changes, Docker version updates, and Windows or Linux kernel security updates\. You can subscribe to the AMI update Amazon SNS topic for the Windows and Linux Amazon ECS\-optimized AMIs to receive notifications when a new Amazon ECS\-optimized AMI is available\. Notifications are available in all formats that Amazon SNS supports\.

**Note**  
Your user account must have `sns::subscribe` IAM permissions to subscribe to an SNS topic\.

You can subscribe an Amazon SQS queue to this notification topic, but you must use a topic ARN that is in the same Region as the Amazon SNS topic\. For more information, see [Tutorial: Subscribing an Amazon SQS Queue to an Amazon SNS Topic](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-subscribe-queue-sns-topic.html) in the *Amazon Simple Queue Service Developer Guide*\.

You can also use an AWS Lambda function to trigger events when notifications are received\. For more information, see [Invoking Lambda functions using Amazon SNS notifications](https://docs.aws.amazon.com/sns/latest/dg/sns-lambda-as-subscriber.html) in the *Amazon Simple Notification Service Developer Guide*\.

## Linux Amazon ECS\-optimized AMIs<a name="sns-topic-linux"></a>

The Amazon SNS topic ARNs for the Linux variants of the Amazon ECS\-optimized AMI for each region are shown below\.


| AWS Region | Amazon SNS Topic ARN | 
| --- | --- | 
| `us-east-1` | `arn:aws:sns:us-east-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `us-east-2` | `arn:aws:sns:us-east-2:177427601217:ecs-optimized-amazon-ami-update` | 
| `us-west-1` | `arn:aws:sns:us-west-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `us-west-2` | `arn:aws:sns:us-west-2:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-east-1` | `arn:aws:sns:ap-east-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-northeast-1` | `arn:aws:sns:ap-northeast-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-northeast-2` | `arn:aws:sns:ap-northeast-2:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-southeast-1` | `arn:aws:sns:ap-southeast-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-southeast-2` | `arn:aws:sns:ap-southeast-2:177427601217:ecs-optimized-amazon-ami-update` | 
| `ap-south-1` | `arn:aws:sns:ap-south-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `ca-central-1` | `arn:aws:sns:ca-central-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `eu-west-1` | `arn:aws:sns:eu-west-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `eu-west-2` | `arn:aws:sns:eu-west-2:177427601217:ecs-optimized-amazon-ami-update` | 
| `eu-west-3` | `arn:aws:sns:eu-west-3:177427601217:ecs-optimized-amazon-ami-update` | 
| `eu-central-1` | `arn:aws:sns:eu-central-1:177427601217:ecs-optimized-amazon-ami-update` | 
| `sa-east-1` | `arn:aws:sns:sa-east-1:177427601217:ecs-optimized-amazon-ami-update` | 

**To subscribe to AMI update notification email in the AWS Management Console**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the region list, choose the same Region as the topic ARN to which to subscribe\. This example uses the `us-west-2` Region\.

1. In the left navigation pane, choose **Subscriptions**, **Create subscription**\.

1. In the **Create Subscription** dialog box, for **Topic ARN**, paste the Amazon ECS\-optimized Amazon Linux AMI update topic ARN: `arn:aws:sns:us-west-2:177427601217:ecs-optimized-amazon-ami-update`\. 

1. For **Protocol**, choose **Email**\. For **Endpoint**, type an email address that you can use to receive the notification\.

1. Choose **Create subscription**\.

1. In your email application, open the message from AWS Notifications and open the link to confirm your subscription\.

   Your web browser displays a confirmation response from Amazon SNS\.

**To subscribe to AMI update notification email with the AWS CLI**

1. Run the following command with the AWS CLI:

   ```
   aws sns --region us-west-2 subscribe --topic-arn arn:aws:sns:us-west-2:177427601217:ecs-optimized-amazon-ami-update --protocol email --notification-endpoint your_email@your_domain.com
   ```

1. In your email application, open the message from AWS Notifications and open the link to confirm your subscription\.

   Your web browser displays a confirmation response from Amazon SNS\.

### Amazon SNS Message Format<a name="ECS-AMI-Notification-format"></a>

An example AMI update notification message is shown below:

```
{
  "Type" : "Notification",
  "MessageId" : "e2534930-337d-5561-8636-1a2be5ba802e",
  "TopicArn" : "arn:aws:sns:us-west-2:917786371007:ecs-optimized-amazon-ami-update",
  "Message" : "{\"ECSAgent\":{\"ReleaseVersion\":\"1.17.2\"},\"ECSAmis\":[{\"ReleaseVersion\":\"2017.09.j\",\"AgentVersion\":\"1.17.2\",\"ReleaseNotes\":\"This AMI includes the latest ECS agent 1.17.2\",\"OsType\":\"linux\",\"OperatingSystemName\":\"Amazon Linux\",\"Regions\":{\"ap-northeast-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-bb5f13dd\"},\"ap-northeast-2\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-3b19b455\"},\"ap-south-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-9e91cff1\"},\"ap-southeast-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-f88ade84\"},\"ap-southeast-2\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-a677b6c4\"},\"ca-central-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-db48cfbf\"},\"cn-north-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-ca508ca7\"},\"eu-central-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-3b7d1354\"},\"eu-west-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-64c4871d\"},\"eu-west-2\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-25f51242\"},\"eu-west-3\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-0356e07e\"},\"sa-east-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-da2c66b6\"},\"us-east-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-cad827b7\"},\"us-east-2\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-ef64528a\"},\"us-gov-west-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-cc3cb7ad\"},\"us-west-1\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-29b8b249\"},\"us-west-2\":{\"Name\":\"amzn-ami-2017.09.j-amazon-ecs-optimized\",\"ImageId\":\"ami-baa236c2\"}}}]}",
  "Timestamp" : "2018-03-09T00:25:43.483Z",
  "SignatureVersion" : "1",
  "Signature" : "XWox8GDGLRiCgDOXlo/fG9Lu/88P8S0FL6M6oQYOmUFzkucuhoblsdea3BjqdCHcWR7qdhMPQnLpN7y9iBrWVUqdAGJrukAI8athvAS+4AQD/V/QjrhsEnlj+GaiW+ozAu006X6GopOzFGnCtPMROjCMrMonjz7Hpv/8KRuMZR3pyQYm5d4wWB7xBPYhUMuLoZ1V8YFs55FMtgQV/YLhSYuEu0BP1GMtLQauxDkscOtPP/vjhGQLFx1Q9LTadcQiRHtNIBxWL87PSI+BVvkin6AL7PhksvdQ7FAgHfXsit+6p8GyOvKCqaeBG7HZhR1AbpyVka7JSNRO/6ssyrlj1g==",
  "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-433026a4050d206028891664da859041.pem",
  "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:917786371007:ecs-optimized-amazon-ami-update:8ad8798e-3bbf-4490-89b1-76136fca61dc"
}
```

The parsed `Message` value \(with escaped quotes removed\) is shown below:

```
{
  "ECSAgent": {
    "ReleaseVersion": "1.17.2"
  },
  "ECSAmis": [
    {
      "ReleaseVersion": "2017.09.j",
      "AgentVersion": "1.17.2",
      "ReleaseNotes": "This AMI includes the latest ECS agent 1.17.2",
      "OsType": "linux",
      "OperatingSystemName": "Amazon Linux",
      "Regions": {
        "ap-northeast-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-bb5f13dd"
        },
        "ap-northeast-2": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-3b19b455"
        },
        "ap-south-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-9e91cff1"
        },
        "ap-southeast-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-f88ade84"
        },
        "ap-southeast-2": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-a677b6c4"
        },
        "ca-central-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-db48cfbf"
        },
        "cn-north-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-ca508ca7"
        },
        "eu-central-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-3b7d1354"
        },
        "eu-west-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-64c4871d"
        },
        "eu-west-2": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-25f51242"
        },
        "eu-west-3": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-0356e07e"
        },
        "sa-east-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-da2c66b6"
        },
        "us-east-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-cad827b7"
        },
        "us-east-2": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-ef64528a"
        },
        "us-gov-west-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-cc3cb7ad"
        },
        "us-west-1": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-29b8b249"
        },
        "us-west-2": {
          "Name": "amzn-ami-2017.09.j-amazon-ecs-optimized",
          "ImageId": "ami-baa236c2"
        }
      }
    }
  ]
}
```

## Windows Amazon ECS\-optimized AMIs<a name="sns-topic-windows"></a>

AWS provides two Amazon SNS topic ARNs for variants of the Windows Amazon ECS\-optimized AMI\. One topic sends update notifications when new Windows AMIs are released\. The other topic sends notifications when previously released Windows AMIs are made private\. For more information on subscribing to Windows AMI notifications, see [Subscribing to Windows AMI notifications](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/windows-ami-version-history.html#subscribe-notifications) in the *Amazon EC2 User Guide for Windows Instances*\.