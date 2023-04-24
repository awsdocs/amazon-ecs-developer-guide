# Tagging your Amazon ECS resources<a name="ecs-using-tags"></a>

To help you manage your Amazon ECS resources, you can optionally assign your own metadata to each resource using *tags*\. Each *tag* consists of a *key* and an optional *value*\.

You can use tags to categorize your Amazon ECS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type\. You can quickly identify a specific resource based on the tags that you assigned to it\. For example, you can define a set of tags for your account's Amazon ECS container instances\. This helps you track each instance's owner and stack level\.

You can use tags for your Cost and Usage reports\. You can use these reports to analyze the cost and usage of your Amazon ECS resources\. For more information, see [Amazon ECS usage reports](usage-reports.md)\.

**Warning**  
Tag keys and their values are returned by many different API calls\. Denying access to `DescribeTags` doesn’t automatically deny access to tags returned by other APIs\. As a best practice, we recommend that you do not include sensitive data in your tags\.

We recommend that you devise a set of tag keys that meets your needs for each resource type\. You can use a consistent set of tag keys for easier management of your resources\. You can search and filter the resources based on the tags you add\.

Tags don't have any semantic meaning to Amazon ECS and are interpreted strictly as a string of characters\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the earlier value\. If you delete a resource, any tags for the resource are also deleted\.

If you use AWS Identity and Access Management \(IAM\), you can control which users in your AWS account have permission to manage tags\.

## How resources are tagged<a name="tag-resources"></a>

There are multiple ways that Amazon ECS tasks, services, task definitions, and clusters are tagged:
+ A user manually tags a resource by using the AWS Management Console, Amazon ECS API, the AWS CLI, or an AWS SDK\.
+ A user creates a service or runs a standalone task and selects the Amazon ECS\-managed tags option\.

  Amazon ECS automatically tags all newly launched tasks\. For more information, see [Amazon ECS\-managed tags](#managed-tags)\.
+ A user creates a resource using the console\. The console automatically tags the resources\.

  These tags are returned in the AWS CLI, and AWS SDK responses and are displayed in the console\. You cannot modify or delete these tags\.

  For information about the added tags, see the **Tags automatically added by the console** column in the **Tagging support for Amazon ECS resources** table\.

If you specify tags when you create a resource and the tags can't be applied, Amazon ECS rolls back the creation process\. This ensures that resources are either created with tags or not created at all, and that no resources are left untagged at any time\. By tagging resources while they're being created, you can eliminate the need to run custom tagging scripts after resource creation\.

The following table describes the Amazon ECS resources that support tagging\.


**Tagging support for Amazon ECS resources**  

|  Resource  |  Supports tags  |  Supports tag propagation  | Tags automatically added by the console | 
| --- | --- | --- | --- | 
|  Amazon ECS tasks  |  Yes  |  Yes, from the task definition\.  | Key: aws:ecs:clusterName *Value*: `cluster-name` | 
|  Amazon ECS services  |  Yes  |  Yes, from either the task definition or the service to the tasks in the service\.  | Key: ecs:service:stackId *Value* `arn:aws:cloudformation:arn` | 
|  Amazon ECS task sets  |  Yes  |  No  | N/A | 
|  Amazon ECS task definitions  |  Yes  |  No  | Key: ecs:taskDefinition:createdFrom *Value*: `ecs-console-v2` | 
|  Amazon ECS clusters  |  Yes  |  No  | Key: aws:cloudformation:logical\-id *Value*: `ECSCluster` Key: aws:cloudformation:stack\-id*Value*: `arn:aws:cloudformation:arn`*Key*: `aws:cloudformation:stack-name`*Value*: `ECS-Console-V2-Cluster-EXAMPLE` | 
|  Amazon ECS container instances  |  Yes  |  Yes, from the Amazon EC2 instance\. For more information, see [Adding tags to an Amazon EC2 container instance](#instance-details-tags)\.   | N/A | 
|  Amazon ECS External instances  |  Yes  |  No  | N/A | 
| Amazon ECS capacity provider |  Yes\. You cannot tag the predefined `FARGATE` and `FARGATE_SPOT` capacity providers\. | No | N/A | 

## Tagging resources on creation<a name="tags-on-creation"></a>

The following resources support tagging on creation using the Amazon ECS API, AWS CLI, AWS SDK:
+ Amazon ECS tasks
+ Amazon ECS services
+ Amazon ECS task definition
+ Amazon ECS task sets
+ Amazon ECS clusters
+ Amazon ECS container instances
+ Amazon ECS capacity providers

Amazon ECS has the option to use tagging authorization for resource creation\. In order to use the feature, perform the following steps:
+ You must opt into the feature\. For more information, see [Tagging authorization](ecs-account-settings.md#tag-resources-setting)\.
+ After you opt in, users must have permissions for actions that creates the resource, such as `ecsCreateCluster`\. If tags are specified in the resource\-creating action, AWS performs additional authorization to verify if users or roles have permissions to create tags\. Therefore, you must grant explicit permissions to use the `ecs:TagResource` action\. For more information, see [Grant permission to tag resources on creation](supported-iam-actions-tagging.md)\.

## Tag restrictions<a name="tag-restrictions"></a>

The following restrictions apply to tags:
+ A maximum of 50 tags can be associated with a resource\.
+ Tag keys can't be repeated for one resource\. Each tag key must be unique, and can only have one value\.
+ Keys can be up to 128 characters long in UTF\-8\.
+ Values can be up to 256 characters long in UTF\-8\.
+ If multiple AWS services and resources use your tagging schema, limit the types of characters you use\. Some services might have restrictions on allowed characters\. Generally, allowed characters are letters, numbers, spaces, and the following characters: `+` `-` `=` `.` `_` `:` `/` `@`\.
+ Tag keys and values are case sensitive\.
+ You can't use `aws:`, `AWS:`, or any upper or lowercase combination of such as a prefix for either keys or values\. These are reserved only for AWS use\. You can't edit or delete tag keys or values with this prefix\. Tags with this prefix don't count against your tags\-per\-resource limit\.

## Amazon ECS\-managed tags<a name="managed-tags"></a>

When you use Amazon ECS\-managed tags, Amazon ECS automatically tags all newly launched tasks with cluster information and either the user added task definition tags or the service tags\. The following describes the added tags:
+ Standalone tasks – a tag with a *Key* as `aws:ecs:clusterName` and a *Value* set to the cluster name\. All task definition tags that were added by users\. 
+ Tasks that are part of a service – a tag with a *Key* as `aws:ecs:clusterName` and a *Value* set to the cluster name\. A tag with a *Key* as `aws:ecs:serviceName` and a *Value* set to the service name\. Tags from one of the following resources:
  + Task definitions – All task definition tags that were added by users\.
  + Services – All service tags that were added by users\.

The following options are required for this feature:
+ You must opt in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) formats\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\.
+ When you use the APIs to create a service or run a task, you must set `enableECSManagedTags` to `true` for `run-task` and `create-service`\. For more information, see [create\-service](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html) and [run\-task](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) in the *AWS Command Line Interface API Reference*\.

## Tagging your resources for billing<a name="tag-resources-for-billing"></a>

You can use Amazon ECS\-managed tags or user\-added tags for your Cost and Usage Report\. For more information, see [Amazon ECS usage reports](usage-reports.md)\.

To see the cost of your combined resources, you can organize your billing information based on resources that have the same tag key values\. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services\. For more information about setting up a cost allocation report with tags, see [The Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html) in the *AWS Billing User Guide*\.

Additionally, you can turn on *Split Cost Allocation Data* to get task\-level CPU and memory usage data in your Cost and Usage Reports\. For more information, see [Task\-level Cost and Usage Reports](usage-reports.md#task-cur)\.

**Note**  
If you've turned on reporting, it can take up to 24 hours before the data for the current month is available for viewing\.

## Working with tags using the console<a name="tag-resources-console"></a>

Using the Amazon ECS console, you can manage the tags that are associated with new or existing tasks, services, task definitions, clusters, or container instances\.

When you select a resource\-specific page in the Amazon ECS console, it displays a list of those resources\. For example, if you select **Clusters** from the navigation pane, the console displays a list of Amazon ECS clusters\. When you select a resource from one of these lists \(for example, a specific cluster\) that supports tags, you can view and manage its tags on the **Tags** tab\.

**Warning**  
As a best practice, we recommend that you do not include sensitive data in your tags\.

**Topics**
+ [Adding tags on an individual resource during launch](#adding-tags-creation)
+ [Managing individual resource tags using the console](#adding-or-deleting-tags)
+ [Adding tags to an Amazon EC2 container instance](#instance-details-tags)
+ [Adding tags to an external container instance](#instance-details-tags-external)

### Adding tags on an individual resource during launch<a name="adding-tags-creation"></a>

You can use the following resources to specify tags when you create the resource\.


|  Task  |  Console  | 
| --- | --- | 
|  Run one or more tasks\.  |  [Running a standalone task using the Amazon ECS console](ecs_run_task-v2.md)  | 
|  Create a service\.  |  [Creating a service using the console](create-service-console-v2.md)  | 
|  Create a task set\.  |  [External deployment](deployment-type-external.md)  | 
|  Register a task definition\.  | [Creating a task definition using the console](create-task-definition.md) | 
|  Create a cluster\.  |  [Creating a cluster for the Fargate launch type using the console](create-cluster-console-v2.md)   | 
|  Run one or more container instances\.  |  [Launching an Amazon ECS Linux container instance](launch_container_instance.md)  | 

### Managing individual resource tags using the console<a name="adding-or-deleting-tags"></a>

Amazon ECS allows you to add or delete tags that are associated with your clusters, services, tasks, and task definitions directly from the resource's page\. For information about tagging your container instances, see [Adding tags to an Amazon EC2 container instance](#instance-details-tags)\.

**Warning**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags are not intended to be used for private or sensitive data\.

**To modify a tag for an individual resource**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the AWS Region to use\.

1. In the navigation pane, select a resource type \(for example, **Clusters**\)\.

1. Select the resource from the resource list, choose the **Tags** tab, and then choose **Manage tags**\.

1. Configure your tags\.

   \[Add a tag\] Choose **Add tag**, and then do the following:
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

  The following is an example policy that's used to add these permissions\.

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

**Warning**  
Don't add personally identifiable information \(PII\) or other confidential or sensitive information in tags\. Tags are accessible to many AWS services, including billing\. Tags aren't intended to be used for private or sensitive data\.


**Tagging support for Amazon ECS resources**  

|  Task  |  AWS CLI  |  API action  | 
| --- | --- | --- | 
|  Add or overwrite one or more tags\.  |  [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/tag-resource.html)  |  [TagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TagResource.html)  | 
|  Delete one or more tags\.  |  [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/untag-resource.html)  |  [UntagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UntagResource.html)  | 

You can use some resource\-creating actions to specify tags when you create the resource\. The following actions support tagging on creation\.

You must have the `ecsTagResource` permission\. For more information, see [Grant permission to tag resources on creation](supported-iam-actions-tagging.md)\.


|  Task  |  AWS CLI  |  AWS Tools for Windows PowerShell  |  API Action  | 
| --- | --- | --- | --- | 
|  Run one or more tasks\.  |  [run\-task](https://docs.aws.amazon.com/cli/latest/reference/run-task.html)  |  [Start\-ECSTask](https://docs.aws.amazon.com/powershell/latest/reference/items/Start-ECSTask.html)  |  [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)  | 
|  Create a service\.  |  [create\-service](https://docs.aws.amazon.com/cli/latest/reference/create-service.html)  |  [New\-ECSService](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSService.html)  |  [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html)  | 
|  Create a task set\.  |  [create\-task\-set](https://docs.aws.amazon.com/cli/latest/reference/create-task-set.html)  |  [New\-ECSTaskSet](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSTaskSet.html)  |  [CreateTaskSet](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateTaskSet.html)  | 
|  Register a task definition\.  |  [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/register-task-definition.html)  |  [Register\-ECSTaskDefinition](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-ECSTaskDefinition.html)  |  [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html)  | 
|  Create a cluster\.  |  [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/create-cluster.html)  |  [New\-ECSCluster](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSCluster.html)  |  [CreateCluster](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateCluster.html)  | 
|  Run one or more container instances\.  |  [run\-instances](https://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/run-instances.html)  |  [New\-EC2Instance](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2Instance.html)  |  [RunInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html)  | 