# Tagging Your Amazon ECS Resources<a name="ecs-using-tags"></a>

To help you manage your Amazon ECS tasks, services, task definitions, clusters, and container instances, you can optionally assign your own metadata to each resource in the form of *tags*\. This topic describes tags and shows you how to create them\.

**Important**  
To use this feature, it requires that you opt\-in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) formats\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-resource-ids.md)\.

**Note**  
Resource tagging is not available in the GovCloud \(US\-East\) region\.

**Topics**
+ [Tag Basics](#tag-basics)
+ [Tagging Your Resources](#tag-resources)
+ [Tag Restrictions](#tag-restrictions)
+ [Tagging Your Resources for Billing](#tag-resources-for-billing)
+ [Working with Tags Using the Console](#tag-resources-console)
+ [Working with Tags Using the CLI or API](#tag-resources-api-sdk)

## Tag Basics<a name="tag-basics"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a *key* and an optional *value*, both of which you define\.

Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type—you can quickly identify a specific resource based on the tags you've assigned to it\. For example, you could define a set of tags for your account's Amazon ECS container instances that helps you track each container instance's owner and stack level\.

We recommend that you devise a set of tag keys that meets your needs for each resource type\. Using a consistent set of tag keys makes it easier for you to manage your resources\. You can search and filter the resources based on the tags you add\.

Tags don't have any semantic meaning to Amazon ECS and are interpreted strictly as a string of characters\. Also, tags are not automatically assigned to your resources\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to an empty string, but you can't set the value of a tag to null\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the old value\. If you delete a resource, any tags for the resource are also deleted\.

You can work with tags using the AWS Management Console, the AWS CLI, and the Amazon ECS API\.

If you're using AWS Identity and Access Management \(IAM\), you can control which users in your AWS account have permission to create, edit, or delete tags\.

## Tagging Your Resources<a name="tag-resources"></a>

You can tag new or existing Amazon ECS tasks, services, task definitions, and clusters\.

If you're using the Amazon ECS console, you can apply tags to new resources when they are created or existing resources by using the **Tags** tab on the relevant resource page at any time\.

If you're using the Amazon ECS API, the AWS CLI, or an AWS SDK, you can apply tags to new resources using the `tags` parameter on the relevant API action or use the `TagResource` API action to apply tags to existing resources\. For more information, see [TagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TagResource.html)\.

Additionally, some resource\-creating actions enable you to specify tags for a resource when the resource is created\. If tags cannot be applied during resource creation, we roll back the resource creation process\. This ensures that resources are either created with tags or not created at all, and that no resources are left untagged at any time\. By tagging resources at the time of creation, you can eliminate the need to run custom tagging scripts after resource creation\.

The following table describes the Amazon ECS resources that can be tagged, and the resources that can be tagged on creation\.


**Tagging Support for Amazon ECS Resources**  

| Resource | Supports tags | Supports tagging on creation \(Amazon ECS API, AWS CLI, AWS SDK\) | 
| --- | --- | --- | 
|  Amazon ECS tasks  |  Yes  |  Yes  | 
|  Amazon ECS services  |  Yes  |  Yes  | 
|  Amazon ECS task definitions  |  Yes  |  Yes  | 
|  Amazon ECS clusters  |  Yes  |  Yes  | 
|  Amazon ECS container instances  |  Yes  |  Yes  | 

## Tag Restrictions<a name="tag-restrictions"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per resource – 50
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ Maximum key length – 128 Unicode characters in UTF\-8
+ Maximum value length – 256 Unicode characters in UTF\-8
+ If your tagging schema is used across multiple services and resources, remember that other services may have restrictions on allowed characters\. Generally allowed characters are: letters, numbers, and spaces representable in UTF\-8, and the following characters: \+ \- = \. \_ : / @\.
+ Tag keys and values are case\-sensitive\.
+ Don't use the `aws:` prefix for either keys or values; it's reserved for AWS use\. You can't edit or delete tag keys or values with this prefix\. Tags with this prefix do not count against your tags per resource limit\.

## Tagging Your Resources for Billing<a name="tag-resources-for-billing"></a>

When enabling Amazon ECS managed tags, Amazon ECS will automatically tag all newly launched tasks with the cluster name\. For tasks that belong to a service, they will be tagged with the service name as well\. These managed tags are helpful when reviewing cost allocation after enabling them in your Cost & Usage Report\. For more information, see [Amazon ECS Usage Reports](usage-reports.md)\.

To see the cost of your combined resources, you can organize your billing information based on resources that have the same tag key values\. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services\. For more information about setting up a cost allocation report with tags, see [The Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html) in the *AWS Billing and Cost Management User Guide*\.

**Important**  
To use this feature, it requires that you opt\-in to the new Amazon Resource Name \(ARN\) and resource identifier \(ID\) formats\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-resource-ids.md)\.

**Note**  
If you've just enabled reporting, data for the current month is available for viewing after 24 hours\.

## Working with Tags Using the Console<a name="tag-resources-console"></a>

Using the Amazon ECS console, you can manage the tags associated with new or existing tasks, services, task definitions, clusters, or container instances\.

When you select a resource\-specific page in the Amazon ECS console, it displays a list of those resources\. For example, if you select **Clusters** from the navigation pane, the console displays a list of Amazon ECS clusters\. When you select a resource from one of these lists \(for example, a specific cluster\), if the resource supports tags, you can view and manage its tags on the **Tags** tab\.

**Topics**
+ [Adding Tags on an Individual Resource During Launch](#adding-tags-creation)
+ [Adding and Deleting Tags on an Individual Resource](#adding-or-deleting-tags)
+ [Adding Tags to a Container Instance](#instance-details-tags)

### Adding Tags on an Individual Resource During Launch<a name="adding-tags-creation"></a>

The following resources allow you to specify tags when you create the resource\.


| Task | Console | 
| --- | --- | 
|  Run one or more tasks\.   |  [Running Tasks](ecs_run_task.md)  | 
|  Create a service\.  |  [Creating a Service](create-service.md)  | 
|  Register a task definition\.  |  [Creating a Task Definition](create-task-definition.md)  | 
|  Create a cluster\.  |  [Creating a Cluster](create_cluster.md)  | 
|  Run one or more container instances\.  |  [Launching an Amazon ECS Container Instance](launch_container_instance.md)  | 

### Adding and Deleting Tags on an Individual Resource<a name="adding-or-deleting-tags"></a>

Amazon ECS allows you to add or delete tags associated with your clusters, services, tasks, and task definitions directly from the resource's page\. For information about tagging your container instances, see [Adding Tags to a Container Instance](#instance-details-tags)\.

**To add a tag to an individual resource**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the region to use\.

1. In the navigation pane, select a resource type \(for example, **Clusters**\)\.

1. Select the resource from the resource list and choose **Tags**, **Edit**\.

1. In the **Edit Tags** dialog box, specify the key and value for each tag, and then choose **Save**\.

**To delete a tag from an individual resource**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, select the region to use\.

1. In the navigation pane, choose a resource type \(for example, **Clusters**\)\.

1. Select the resource from the resource list and choose **Tags**, **Edit**\.

1. On the **Edit Tags** page, select the **Delete** icon for each tag you want to delete, and choose **Save**\.

### Adding Tags to a Container Instance<a name="instance-details-tags"></a>

You can associate tags with your container instances using one of the following methods:
+ Method 1 – When creating your container instance, specify tags by passing user data to the instance using the container agent configuration parameter `ECS_CONTAINER_INSTANCE_TAGS`\. This creates tags that are associated with Amazon ECS only, they cannot be listed using the Amazon EC2 API\. For more information, see [Bootstrapping Container Instances with Amazon EC2 User Data](bootstrap_container_instance.md)\.

  The following is an example of a user data script that would associate tags with your container instance:

  ```
  #!/bin/bash
  cat <<'EOF' >> /etc/ecs/ecs.config
  ECS_CLUSTER=MyCluster
  ECS_CONTAINER_INSTANCE_TAGS={"tag_key": "tag_value"}
  EOF
  ```
+ Method 2 – When creating your container instance, specify tags using `TagSpecification.N` and then pass user data to the instance using the container agent configuration parameter `ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM` which will propagate them from Amazon EC2 to Amazon ECS

  The following is an example of a user data script that would propagate the tags associated with a container instance, as well as register the container instance with a cluster named `MyCluster`:

  ```
  #!/bin/bash
  cat <<'EOF' >> /etc/ecs/ecs.config
  ECS_CLUSTER=MyCluster
  ECS_CONTAINER_INSTANCE_PROPAGATE_TAGS_FROM=ec2_instance
  EOF
  ```

  To provide access to allow container instance tags to propagate from Amazon EC2 to Amazon ECS, manually add the following permissions as an inline policy to the Amazon ECS container instance IAM role\. For more information, see [Adding and Removing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\.
  + `ec2:DescribeTags`

  An example inline policy adding the permissions is shown below

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

## Working with Tags Using the CLI or API<a name="tag-resources-api-sdk"></a>

Use the following to add, update, list, and delete the tags for your resources\. The corresponding documentation provides examples\.


**Tagging Support for Amazon ECS Resources**  

| Task | AWS CLI | API Action | 
| --- | --- | --- | 
|  Add or overwrite one or more tags\.  |   |  [TagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_TagResource.html)  | 
|  Delete one or more tags\.  |   |  [UntagResource](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UntagResource.html)  | 

The following examples show how to tag or untag resources using the AWS CLI\.

**Example 1: Tag an existing cluster**  
The following command tags an existing cluster\.

```
aws ecs tag-resource --resource-arn resource_ARN --tags key=stack,values=dev
```

**Example 2: Untag an existing cluster**  
The following command deletes a tag from an existing cluster\.

```
aws ecs untag-resource --resource-arn resource_ARN --tag-keys tag_key
```

**Example 3: List tags for a resource**  
The following command lists the tags associated with an existing resource\.

```
aws ecs list-tags-for-resource --resource-arn resource_ARN
```

Some resource\-creating actions enable you to specify tags when you create the resource\. The following actions support tagging on creation\.


| Task | AWS CLI | AWS Tools for Windows PowerShell | API Action | 
| --- | --- | --- | --- | 
|  Run one or more tasks\.   |  [run\-task](https://docs.aws.amazon.com/cli/latest/reference/run-task.html)  |  [Start\-ECSTask](https://docs.aws.amazon.com/powershell/latest/reference/items/Start-ECSTask.html)  |  [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)  | 
|  Create a service\.  |  [create\-service](https://docs.aws.amazon.com/cli/latest/reference/create-service.html)  |  [New\-ECSService](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSService.html)  |  [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html)  | 
|  Register a task definition\.  |  [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/register-task-definition.html)  |  [Register\-ECSTaskDefinition](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-ECSTaskDefinition.html)  |  [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html)  | 
|  Create a cluster\.  |  [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/create-cluster.html)  |  [New\-ECSCluster](https://docs.aws.amazon.com/powershell/latest/reference/items/New-ECSCluster.html)  |  [CreateCluster](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateCluster.html)  | 
|  Run one or more container instances\.  |  [run\-instances](https://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/run-instances.html)  |  [New\-EC2Instance](https://docs.aws.amazon.com/powershell/latest/reference/items/New-EC2Instance.html)  |  [RunInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html)  | 

The following examples demonstrate how to apply tags when you create resources\.

**Example 1: Create a cluster and apply a tag**  
The following command creates a cluster named `devcluster` and adds a tag with key `team` and value `devs`\.

```
aws ecs create-cluster --cluster-name devcluster --tags key=team,values=devs
```

**Example 2: Create a service and apply a tag**  
The following command creates a service named `application` and adds a tag with key `stack` and value `dev`\.

```
aws ecs create-service --service-name application --task-definition task-def-app --tags key=stack,values=dev
```

The `--propagateTags` parameter can be used to copy the tags from either a task definition or a service to the tasks in a service\. The following command creates a service with tags and propagates them to the tasks in that service\.

```
aws ecs create-service --service-name application --task-definition task-def-app --tags key=stack,values=dev --propagateTags Service
```