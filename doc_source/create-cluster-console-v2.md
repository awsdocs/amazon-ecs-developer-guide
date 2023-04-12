# Creating a cluster for the Fargate launch type using the console<a name="create-cluster-console-v2"></a>

You can create an Amazon ECS cluster using the new Amazon ECS experience\. Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) and assign the appropriate IAM permission\. For more information, see [Cluster examples](security_iam_id-based-policy-examples.md#IAM_cluster_policies)\. The new Amazon ECS experience provides a simple way to create the resources that are needed by an Amazon ECS cluster by creating a AWS CloudFormation stack\. 

To make the cluster creation process as easy as possible, the console has default selections for many choices which we describe below\. There are also help panels available for most of the sections in the console which provide further context\. 

By default, we create an Amazon ECS cluster for the Fargate launch type with the following properties:
+ Uses Fargate and Fargate Spot capacity providers\.
+ Launches tasks and services in all the default subnets in the default VPC for your selected Region\.
+ Does not use Container Insights\.
+ Has three tags configured for AWS CloudFormation\.
+ Creates a default namespace in AWS Cloud Map that is the same name as the cluster\. A namespace allows services that you create in the cluster to connect to the other services in the namespace without additional configuration\. 

  For more information, see [Interconnecting services](interconnecting-services.md)\.

You can modify the following default options:
+ Change the subnets where tasks and services launch into by default\.
+ Change the default namespace associated with the cluster\. 
+ Turn on Container Insights\.

  CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. Container Insights also provides diagnostic information, such as container restart failures, that you use to isolate issues and resolve them quickly\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Add tags to help you identify your clusters\.

**To create a new cluster \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter a unique name\.

   The name can contain up to 255 letters \(uppercase and lowercase\), numbers, and hyphens\.

1. \(Optional\) To change the name of the default namespace\. for **Namespace**, enter a unique name\.

1. \(Optional\) To change the VPC and subnets where your tasks and services launch, under **Networking**, perform any of the following operations:
   + To remove a subnet, under **Subnets**, choose **X** for each subnet that you want to remove\.
   + To change to a VPC other than the **default** VPC, under **VPC**, choose an existing **VPC**, and then under **Subnets**, select each subnet\.

1. \(Optional\) To turn on Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

1. \(Optional\) To help identify your cluster, expand **Tags**, and then configure your tags\.

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.