# AWS Fargate throttling limits<a name="throttling"></a>

AWS Fargate throttles Amazon ECS RunTask API requests for each AWS account on a per\-Region basis\. We do this to help the performance of the service, and to ensure fair usage for all Fargate customers\. Throttling ensures that calls to the launch tasks do not exceed the maximum allowed API request limits\. API calls are subject to the request limits whether they originate from the Amazon ECS console, a command line tool, or a third\-party application\.

When the RunTask API throttling limit is reached, you will get the following exception:

```
An error occurred (ThrottlingException) when calling the RunTask operation (reached max retries: 4): Rate exceeded.
```

To request an API rate limit quota increase, complete the following steps\.

**To request a rate limit increase**

1. Open the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

1. Choose **Create case**, **Service limit increase**\.

1. For **Limit type**, select **Fargate**\.

1. For **Region**, select the region you want to submit the rate limit increase for\.

1. For **Limit**, choose **Concurrent Task Limit**\.

1. For **Use case description**, describe your use case concerning the RunTask API throttle limit so support can assist you further\.