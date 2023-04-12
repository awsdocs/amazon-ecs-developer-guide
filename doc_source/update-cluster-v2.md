# Updating a cluster using the console<a name="update-cluster-v2"></a>

You can modify the following cluster properties:
+ Set a default capacity provider

  Each cluster can have one or more capacity providers and an optional capacity provider strategy\. The capacity provider strategy determines how the tasks are spread across the cluster's capacity providers\. When you run a standalone task or create a service, you either use the cluster's default capacity provider strategy or a capacity provider strategy that overrides the default one\.

  Capacity providers are an alternative to launch types\. For more information, see [Capacity provider concepts](cluster-capacity-providers.md#capacity-providers-concepts)\.
+ Turn on Container Insights\.

  CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. Container Insights also provides diagnostic information, such as container restart failures, that you use to isolate issues and resolve them quickly\. For more information, see [Amazon ECS CloudWatch Container Insights](cloudwatch-container-insights.md)\.
+ Add tags to help you identify your clusters\.

**To update the cluster \(Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose **Update cluster**\.

1. To set the default capacity provider, under **Default capacity provider strategy**, choose **Add more**\.

   1. For **Capacity prover**, choose the capacity provider\.

   1. \(Optional\) For **Base**, enter the minimum number of tasks that run on the capacity provider\. 

      You can only set a **Base** value for one capacity provider\.

   1. \(Optional\) For **Weight**, enter the relative percentage of the total number of launched tasks that use the specified capacity provider\.

   1. \(Optional\) Repeat the steps for any additional capacity providers\.

1. To turn on or off Container Insights, expand **Monitoring**, and then turn on **Use Container Insights**\.

1. To help identify your cluster, expand **Tags**, and then configure your tags\.

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Update**\.