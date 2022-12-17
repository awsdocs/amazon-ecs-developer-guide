# Tagging your Amazon ECS resources<a name="ecs-using-tags"></a>

To help you manage your Amazon ECS resources, you can optionally assign your own metadata to each resource using *tags*\. This topic provides an overview of tags in Amazon ECS and how you can create them\.

To use this feature, you must opt in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) format\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\.

**Important**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags are not intended to be used for private or sensitive data\.

## Tag basics<a name="tag-basics"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a *key* and an optional *value*\. You define both\.

You can use tags to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type\. You can quickly identify a specific resource based on the tags that you assigned to it\. For example, you can define a set of tags for your account's Amazon ECS container instances\. This helps you track each instance's owner and stack level\.

We recommend that you devise a set of tag keys that meets your needs for each resource type\. You can use a consistent set of tag keys for easier management of your resources\. You can search and filter the resources based on the tags you add\.

Tags don't have any semantic meaning to Amazon ECS and are interpreted strictly as a string of characters\. Also, tags aren't automatically assigned to your resources\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the earlier value\. If you delete a resource, any tags for the resource are also deleted\.

You can work with tags using the AWS Management Console, the AWS CLI, and the Amazon ECS API\.

If you use AWS Identity and Access Management \(IAM\), you can control which users in your AWS account have permission to manage tags\.

## Tagging your resources<a name="tag-resources"></a>

You can tag new or existing Amazon ECS tasks, services, task definitions, and clusters\.

**Important**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags are not intended to be used for private or sensitive data\.

If you're using the Amazon ECS console, you can apply tags to new or existing resources\. Do this by using the **Tags** tab on the relevant resource page at any time\. When you use the Amazon ECS\-managed tags option \(the **Propagate tags from** option\), tags are copied from the task definition or service to a task\. This can be done when you're running a task or creating a service\.

If you're using the Amazon ECS API, the AWS CLI, or an AWS SDK, you can apply tags to new resources using the `tags` parameter on the relevant API action\. Or, alternatively, you can use the `TagResource` API action to apply tags to existing resources\. For more information, see [TagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TagResource.html)\. The **propagateTags** parameter can be used to copy tags from the task definition or service to the task\. This can be done when you run a task or create a service\. For more information, see [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) and [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html)\.

Additionally, you can use some resource\-creating actions to specify tags for a resource when the resource is created\. If tags can't be applied while resources are being created, we roll back the process of creating resources\. This ensures that resources are either created with tags or not created at all, and that no resources are left untagged at any time\. By tagging resources while they're being created, you can eliminate the need to run custom tagging scripts after resource creation\.

When you use the ECS managed tags option, Amazon ECS automatically propagates tags to your tasks from either your task definition or your service\.

The following table describes the Amazon ECS resources that can be tagged, and the resources that can be tagged when created\.


**Tagging support for Amazon ECS resources**  

| Resource | Supports tags | Supports tag propagation | Supports tagging on creation \(Amazon ECS API, AWS CLI, AWS SDK\) | 
| --- | --- | --- | --- | 
|  Amazon ECS tasks  |  Yes  | Yes, from the task definition\. |  Yes  | 
|  Amazon ECS services  |  Yes  | Yes, from either the task definition or the service to the tasks in the service\. |  Yes  | 
|  Amazon ECS task sets  |  Yes  |  No  |  Yes  | 
|  Amazon ECS task definitions  |  Yes  | No |  Yes  | 
|  Amazon ECS clusters  |  Yes  | No |  Yes  | 
|  Amazon ECS container instances  |  Yes  |  Yes, from the Amazon EC2 instance\. For more information, see [Adding tags to an Amazon EC2 container instance](#instance-details-tags)\.   |  Yes  | 
|  Amazon ECS External instances  |  Yes  |  No  |  No, you can add tags after the External instance is registered to a cluster using the AWS Management Console or by using the Amazon ECS API, AWS CLI, or AWS SDK\. For more information, see [Managing individual resource tags using the console](#adding-or-deleting-tags)\.  | 
| Amazon ECS capacity provider |  Yes\. Predefined FARGATE and FARGATE\_SPOT capacity providers cannot be tagged\. | No | Yes | 

## Tag restrictions<a name="tag-restrictions"></a>

The following basic restrictions apply to tags:
+ The maximum number of tags for each resource – 50
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ The maximum key length – 128 Unicode characters in UTF\-8
+ The maximum value length – 256 Unicode characters in UTF\-8
+ If your tagging schema is used across multiple services and resources, remember that other services might have restrictions on allowed characters\. Generally allowed characters are: letters, numbers, and spaces representable in UTF\-8, and the following characters: \+ \- = \. \_ : / @\.
+ Tag keys and values are case sensitive\.
+ Don't use `aws:`, `AWS:`, or any upper or lowercase combination of such as a prefix for either keys or values\. These are reserved only for AWS use\. You can't edit or delete tag keys or values with this prefix\. Tags with this prefix don't count toward your tags quota\.

## Tagging your resources for billing<a name="tag-resources-for-billing"></a>

When you use Amazon ECS\-managed tags, Amazon ECS automatically tags all newly launched tasks with the cluster name\. For tasks that belong to a service, they are also tagged with the service name\. These managed tags are helpful when reviewing cost allocation after enabling them in your Cost and Usage Report\. For more information, see [Amazon ECS usage reports](usage-reports.md)\.

The following options are required for this feature:
+ You must opt in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) formats\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\.
+ When you use the APIs to create a service or run a task, you must set `enableECSManagedTags` to `true` for `run-task` and `create-service`\. For more information, see [create\-service](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html) and [run\-task](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) in the *AWS Command Line Interface API Reference*\.

To see the cost of your combined resources, you can organize your billing information based on resources that have the same tag key values\. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services\. For more information about setting up a cost allocation report with tags, see [The Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html) in the *AWS Billing User Guide*\.

**Note**  
If you've turned on reporting, data for the current month is available for viewing after 24 hours\.



## Working with tags using the console<a name="tag-resources-console"></a>

Using the Amazon ECS console, you can manage the tags that are associated with new or existing tasks, services, task definitions, clusters, or container instances\.

When you select a resource\-specific page in the Amazon ECS console, it displays a list of those resources\. For example, if you select **Clusters** from the navigation pane, the console displays a list of Amazon ECS clusters\. When you select a resource from one of these lists \(for example, a specific cluster\) that supports tags, you can view and manage its tags on the **Tags** tab\.

**Important**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags are not intended to be used for private or sensitive data\.

**Topics**
+ [Adding tags on an individual resource during launch](#adding-tags-creation)
+ [Managing individual resource tags using the console](#adding-or-deleting-tags)
+ [Adding tags to an Amazon EC2 container instance](#instance-details-tags)
+ [Adding tags to an external container instance](#instance-details-tags-external)

### Adding tags on an individual resource during launch<a name="adding-tags-creation"></a>

You can use the following resources to specify tags when you create the resource\.


| Task | Console | 
| --- | --- | 
|  Run one or more tasks\.  |  [Running a standalone task using the new Amazon ECS console](ecs_run_task-v2.md)  | 
|  Create a service\.  |  [Creating a service using the new console](create-service-console-v2.md)  | 
|  Create a task set\.  |  [External deployment](deployment-type-external.md)  | 
|  Register a task definition\.  |  [Creating a task definition using the new console](create-task-definition.md)  | 
|  Create a cluster\.  |  [Creating a cluster for the Fargate launch type using the new console](create-cluster-console-v2.md)   | 
|  Run one or more container instances\.  |  [Launching an Amazon ECS Linux container instance](launch_container_instance.md)  | 

### Managing individual resource tags using the console<a name="adding-or-deleting-tags"></a>

Amazon ECS allows you to add or delete tags that are associated with your clusters, services, tasks, and task definitions directly from the resource's page\. For information about tagging your container instances, see [Adding tags to an Amazon EC2 container instance](#instance-details-tags)\.

**To modify a tag for an individual resource**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the AWS Region to use\.

1. In the navigation pane, select a resource type \(for example, **Clusters**\)\.

1. Select the resource from the resource list, choose the **Tags** tab, and then choose **Manage tags**\.

1. Configure your tags\.

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Save**\.

### Adding tags to an Amazon EC2 container instance<a name="instance-details-tags"></a>

You can associate tags with your container instances using one of the following methods:
+ Method 1 – When creating the container instance using the Amazon EC2 API, CLI, or console, specify tags by passing user data to the instance using the container agent configuration parameter `ECS_CONTAINER_INSTANCE_TAGS`\. This creates tags that are associated with the container instance in Amazon ECS only, they cannot be listed using the Amazon EC2 API\. For more information, see [Bootstrapping container instances with Amazon EC2 user data](bootstrap_container_instance.md)\.
**Important**  
If you launch your container instances using an Amazon EC2 Auto Scaling group, then you should use the ECS\_CONTAINER\_INSTANCE\_TAGS agent configuration parameter to add tags\. This is due to the way in which tags are added to Amazon EC2 instances that are launched using Auto Scaling groups\.

  The following is an example of a user data script that associates tags with your container instance:

  ```
  #!/bin/bash
  cat <<'EOF' >> /etc/ecs/ecs.config
  ECS_CLUSTER=MyCluster
  ECS_CONTAINER_INSTANCE_TAGS={"tag_key": "tag_value"}
  EOF
  ```
+ Method 2 – When you create your container instance using the Amazon EC2 API, CLI, or console, first specify tags using the `TagSpecification.N` parameter\. Then, pass user data to the instance by using the container agent configuration parameter `ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM`\. Doing so propagates them from Amazon EC2 to Amazon ECS\.

  The following is an example of a user data script that propagates the tags that are associated with an Amazon EC2 instance, and registers the instance with a cluster that's named `MyCluster`\.

  ```
  #!/bin/bash
  cat <<'EOF' >> /etc/ecs/ecs.config
  ECS_CLUSTER=MyCluster
  ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM=ec2_instance
  EOF
  ```

  To provide access to allow container instance tags to propagate from Amazon EC2 to Amazon ECS, manually add the following permissions as an inline policy to the Amazon ECS container instance IAM role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
  + `ec2:DescribeTags`

  The following is an example inline policy that's used to add these permissions\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
              	"ec2:DescribeTags"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

### Adding tags to an external container instance<a name="instance-details-tags-external"></a>

You can associate tags with your external container instances by using one of the following methods\.
+ Method 1 – Before running the installation script to register your external instance with your cluster, create or edit the Amazon ECS container agent configuration file at `/etc/ecs/ecs.config` and add the `ECS_CONTAINER_INSTANCE_TAGS` container agent configuration parameter\. This creates tags that are associated with the external instance\.

  The following is example syntax\.

  ```
  ECS_CONTAINER_INSTANCE_TAGS={"tag_key": "tag_value"}
  ```
+ Method 2 – After your external instance is registered to your cluster, you can use the AWS Management Console to add tags\. For more information, see [Managing individual resource tags using the console](#adding-or-deleting-tags)\.

## Working with tags using the CLI or API<a name="tag-resources-api-sdk"></a>

Use the following to add, update, list, and delete the tags for your resources\. The corresponding documentation provides examples\.

**Important**  
Don't add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags aren't intended to be used for private or sensitive data\.


**Tagging support for Amazon ECS resources**  

| Task | AWS CLI | API action | 
| --- | --- | --- | 
|  Add or overwrite one or more tags\.  |  [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/tag-resource.html)  |  [TagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TagResource.html)  | 
|  Delete one or more tags\.  |  [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/untag-resource.html)  |  [UntagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UntagResource.html)  | 

The following examples show how to tag or untag resources using the AWS CLI\.

**Example 1: Tag an existing cluster**  
The following command tags an existing cluster\.

```
aws ecs tag-resource --resource-arn resource_ARN --tags key=stack,value=dev
```

**Example 2: Add multiple tags for a cluster**  
The following command adds multiple tags for a cluster\.

```
aws ecs tag-resource \
--resource-arn resource_ARN \
--tags key=key1,value=value1 key=key2,value=value2 key=key3,value=value3
```

**Example 3: Untag an existing cluster**  
The following command deletes a tag from an existing cluster\.

```
aws ecs untag-resource --resource-arn resource_ARN --tag-keys tag_key
```

**Example 4: List tags for a resource**  
The following command lists the tags associated with an existing resource\.

```
aws ecs list-tags-for-resource --resource-arn resource_ARN
```

You can use some resource\-creating actions to specify tags when you create the resource\. The following actions support tagging on creation\.


| Task | AWS CLI | AWS Tools for Windows PowerShell | API Action | 
| --- | --- | --- | --- | 
|  Run one or more tasks\.  |  [run\-task](https://docs.aws.amazon.com/cli/latest/reference/run-task.html)  |  [Start\-ECSTask](https://docs.aws.amazon.com/powershell/latest/reference/items/Start-ECSTask.html)  |  [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)  | 
|  Create a service\.  |  [create\-service](https://docs.aws.amazon.com/cli/latest/reference/create-service.html)  |  [New\-ECSService](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSService.html)  |  [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html)  | 
|  Create a task set\.  |  [create\-task\-set](https://docs.aws.amazon.com/cli/latest/reference/create-task-set.html)  |  [New\-ECSTaskSet](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSTaskSet.html)  |  [CreateTaskSet](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateTaskSet.html)  | 
|  Register a task definition\.  |  [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/register-task-definition.html)  |  [Register\-ECSTaskDefinition](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-ECSTaskDefinition.html)  |  [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html)  | 
|  Create a cluster\.  |  [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/create-cluster.html)  |  [New\-ECSCluster](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSCluster.html)  |  [CreateCluster](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateCluster.html)  | 
|  Run one or more container instances\.  |  [run\-instances](https://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/run-instances.html)  |  [New\-EC2Instance](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2Instance.html)  |  [RunInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html)  | 

The following examples demonstrate how to apply tags when you create resources\.

**Example 1: Create a cluster and apply a tag**  
The following command creates a cluster named `devcluster` and adds a tag with key `team` and value `devs`\.

```
aws ecs create-cluster --cluster-name devcluster --tags key=team,value=devs
```

**Example 2: Create a service and apply a tag**  
The following command creates a service that's named `application` and adds a tag with key `stack` and value `dev`\.

```
aws ecs create-service --service-name application --task-definition task-def-app --tags key=stack,value=dev
```

**Example 3: Create a service with tags and propagate the tags**  
You can use the `--propagateTags` parameter to copy the tags from either a task definition or a service to the tasks in a service\. The following command creates a service with tags and propagates them to the tasks in that service\.

```
aws ecs create-service --service-name application --task-definition task-def-app --tags key=stack,value=dev --propagateTags Service
```