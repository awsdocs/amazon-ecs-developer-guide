# AWS Fargate throttling quotas<a name="throttling"></a>

AWS Fargate limits Amazon ECS tasks and Amazon EKS pods launch rates to quotas \(formerly referred to as limits\) using a [token bucket algorithm](https://en.wikipedia.org/wiki/Token_bucket) for each AWS account on a per\-Region basis\. With this algorithm, your account has a bucket that holds a specific number of tokens\. The number of tokens in the bucket represents your rate quota at any given second\. Each customer account has a tasks and pods token bucket that depletes based on the number of tasks and pods launched by the customer account\. This token bucket has a bucket maximum that allows you to periodically make a higher number of requests, and a refill rate that allows you to sustain a steady rate of requests for as long as needed\.

For example, the tasks and pods token bucket size for a Fargate customer account is 100 tokens, and the refill rate is 20 tokens per second\. Therefore, you can immediately launch up to 100 Amazon ECS tasks and Amazon EKS pods per customer account, with a sustained launch rate of 20 Amazon ECS tasks and Amazon EKS pods per second\. 


| Actions | Bucket maximum capacity \(or Burst rate\) | Bucket refill rate \(or Sustained rate\) | 
| --- | --- | --- | 
| Fargate Resource rate quota for On Demand Amazon ECS tasks and Amazon EKS pods[1](#fargate-throttling-note-1) | 100 | 20 | 
| Fargate Resource rate quota for Spot Amazon ECS tasks | 100 | 20 | 

<a name="fargate-throttling-note-1"></a>1Accounts launching only Amazon EKS pods have a burst rate of 20 with a sustained pod launch rate of 20 pod launches per second when using the platform versions called out in the [Amazon EKS platform versions](https://docs.aws.amazon.com/eks/latest/userguide/platform-versions.html)\.

## Throttling the `RunTask` API<a name="fargate-throttling-runtask"></a>

In addition, Fargate limits the request rate when launching tasks using the Amazon ECS `RunTask` API using a separate quota\. Fargate limits Amazon ECS `RunTask` API requests for each AWS account on a per\-Region basis\. Each request that you make removes one token from the bucket\. We do this to help the performance of the service, and to ensure fair usage for all Fargate customers\. API calls are subject to the request quotas whether they originate from the Amazon Elastic Container Service console, a command line tool, or a third\-party application\. The rate quota for calls to the Amazon ECS `RunTask` API is 20 calls per second \(burst and sustained\)\. Each call to this API can, however, launch up to 10 tasks\. This means you can launch 100 tasks in one second by making 10 calls to this API, requesting 10 tasks to be launched in each call\. Similarly, you could also make 20 calls to this API, requesting 5 tasks to be launched in each call\. For more information on API throttling for Amazon ECS `RunTask` API , see [API request throttling](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/request-throttling.html) in the Amazon ECS API Reference\.

In practice, task and pod launch rates are also dependent on other considerations such as container images to be downloaded and unpacked, health checks and other integrations enabled, such as registering tasks or pods into a load balancer\. Customers will see variations in task and pod launch rates compared with the quotas represented above based on the features that customers enable\.

## Adjusting rate quotas<a name="fargate-throttling-increase"></a>

You can request an increase for Fargate rate throttling quotas for your AWS account\. To request a quota adjustment, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.