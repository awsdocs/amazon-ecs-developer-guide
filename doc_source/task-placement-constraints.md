# Amazon ECS task placement constraints<a name="task-placement-constraints"></a>

A *task placement constraint* is a rule that is considered during task placement\. Task placement constraints can be specified when either running a task or creating a new service\. The task placement constraints can be updated for existing services as well\. For more information, see [Amazon ECS task placement](task-placement.md)\.

## Constraint types<a name="constraint-types"></a>

Amazon ECS supports the following types of task placement constraints:

`distinctInstance`  
Place each task on a different container instance\. This task placement constraint can be specified when either running a task or creating a new service\.

`memberOf`  
Place tasks on container instances that satisfy an expression\. For more information about the expression syntax for constraints, see [Cluster query language](cluster-query-language.md)\.  
The `memberOf` task placement constraint can be specified with the following actions:  
+ Running a task
+ Creating a new service
+ Creating a new task definition
+ Creating a new revision of an existing task definition

## Attributes<a name="attributes"></a>

You can add custom metadata to your container instances, known as *attributes*\. Each attribute has a name and an optional string value\. You can use the built\-in attributes provided by Amazon ECS or define custom attributes\.

### Built\-in attributes<a name="ecs-automatic-attributes"></a>

Amazon ECS automatically applies the following attributes to your container instances\.

`ecs.ami-id`  
The ID of the AMI used to launch the instance\. An example value for this attribute is `ami-1234abcd`\.

`ecs.availability-zone`  
The Availability Zone for the instance\. An example value for this attribute is `us-east-1a`\.

`ecs.instance-type`  
The instance type for the instance\. An example value for this attribute is `g2.2xlarge`\.

`ecs.os-type`  
The operating system for the instance\. The possible values for this attribute are `linux` and `windows`\.

`ecs.cpu-architecture`  
The CPU architecture for the instance\. The possible values for this attribute are `x86_64` and `arm64`\.

`ecs.vpc-id`  
The VPC the instance was launched into\. An example value for this attribute is `vpc-1234abcd`\.

`ecs.subnet-id`  
The subnet the instance is using\. An example value for this attribute is `subnet-1234abcd`\.

### Optional attributes<a name="ecs-optional-attributes"></a>

Amazon ECS may add the following attributes to your container instances\.

`ecs.awsvpc-trunk-id`  
If this attribute exists, the instance has a trunk network interface\. For more information, see [Elastic network interface trunking](container-instance-eni.md)\.

`ecs.outpost-arn`  
If this attribute exists, it contains the Amazon Resource Name \(ARN\) of the Outpost\. For more information, see [Amazon Elastic Container Service on AWS Outposts](ecs-on-outposts.md)\.

### Custom attributes<a name="ecs-custom-attributes"></a>

You can apply custom attributes to your container instances\. For example, you can define an attribute with the name "stack" and a value of "prod"\.

When specifying custom attributes, the following should be considered\.
+ The `name` must contain between 1 and 128 characters and name may contain letters \(uppercase and lowercase\), numbers, hyphens, underscores, forward slashes, back slashes, or periods\.
+ The `value` must contain between 1 and 128 characters and may contain letters \(uppercase and lowercase\), numbers, hyphens, underscores, periods, at signs \(@\), forward slashes, back slashes, colons, or spaces\. The value cannot contain any leading or trailing whitespace\.

## Adding an attribute<a name="add-attribute"></a>

You can add custom attributes at instance registration time using the container agent or manually, using the AWS Management Console\. For more information about using the container agent, see [Amazon ECS Container Agent Configuration Parameters](ecs-agent-config.md#ecs-instance-attributes)\.

**To add custom attributes using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters** and select a cluster\.

1. On the **ECS Instances** tab, select the check box for the container instance\.

1. Choose **Actions**, **View/Edit Attributes**\.

1. For each attribute, do the following:

   1. Choose **Add attribute**\.

   1. Type a name and a value for the attribute and choose the checkmark icon\.

1. When you are finished adding attributes, choose **Close**\.

**Adding custom attributes using the AWS CLI**  
The following examples demonstrate how to add custom attributes using the [put\-attributes](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-attributes.html) command\.

**Example: Single Attribute**  
The following example adds the custom attribute "stack=prod" to the specified container instance in the default cluster\.

```
aws ecs put-attributes --attributes name=stack,value=prod,targetId=arn
```

**Example: Multiple Attributes**  
The following example adds the custom attributes "stack=prod" and "project=a" to the specified container instance in the default cluster\.

```
aws ecs put-attributes --attributes name=stack,value=prod,targetId=arn name=project,value=a,targetId=arn
```

## Filtering by attribute<a name="filter-attribute"></a>

You can apply a filter for your container instances, allowing you to see custom attributes\.

**Filter container instances by attribute using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Choose a cluster that has container instances\.

1. Choose **ECS Instances**\.

1. Set column visibility preferences by choosing the gear icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/cog.png)\) and selecting the attributes to display\. This setting persists across all container clusters associated with your account\.

1. Using the **Filter by attributes** text field, type or select the attributes you would like to filter by\. The format must be *AttributeName:AttributeValue*\.

   For **Filter by attributes**, type or select the attributes by which to filter\. After you select the attribute name, you are prompted for the attribute value\.

1. Add additional attributes to the filter as needed\. Remove an attribute by choosing the **X** next to it\.

**Filter container instances by attribute using the AWS CLI**  
The following examples demonstrate how to filter container instances by attribute using the [list\-constainer\-instances](https://docs.aws.amazon.com/cli/latest/reference/ecs/list-container-instances.html) command\. For more information about the filter syntax, see [Cluster query language](cluster-query-language.md)\.

**Example: Built\-in attribute**  
The following example uses built\-in attributes to list the g2\.2xlarge instances\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type == g2.2xlarge"
```

**Example: Custom attribute**  
The following example lists the instances with the custom attribute "stack=prod"\.

```
aws ecs list-container-instances --filter "attribute:stack == prod"
```

**Example: Exclude an attribute value**  
The following example lists the instances with the custom attribute "stack" unless the attribute value is "prod"\.

```
aws ecs list-container-instances --filter "attribute:stack != prod"
```

**Example: Multiple attribute values**  
The following example uses built\-in attributes to list the instances of type `t2.small` or `t2.medium`\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type in [t2.small, t2.medium]"
```

**Example: Multiple attributes**  
The following example uses built\-in attributes to list the T2 instances in the us\-east\-1a Availability Zone\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type =~ t2.* and attribute:ecs.availability-zone == us-east-1a"
```

## Task groups<a name="task-groups"></a>

You can identify a set of related tasks as a *task group*\. All tasks with the same task group name are considered as a set when performing spread placement\. For example, suppose that you are running different applications in one cluster, such as databases and web servers\. To ensure that your databases are balanced across Availability Zones, add them to a task group named "databases" and then use this task group as a constraint for task placement\.

When you launch a task using the `RunTask` or `StartTask` action, you can specify the name of the task group for the task\. If you don't specify a task group for the task, the default name is the family name of the task definition \(for example, `family:my-task-definition`\)\.

For tasks launched by the service scheduler, the task group name is the name of the service \(for example, `service:my-service-name`\)\.

**Limits**
+ A task group name must be 255 characters or less\.
+ Each task can be in exactly one group\.
+ After launching a task, you cannot modify its task group\.

## Example constraints<a name="constraint-examples"></a>

The following are task placement constraint examples\.

This example uses the `memberOf` constraint to place tasks on T2 instances\. It can be specified with the following actions: [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html), [UpdateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html), [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html), and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)\.

```
"placementConstraints": [
    {
        "expression": "attribute:ecs.instance-type =~ t2.*",
        "type": "memberOf"
    }
]
```

The example uses the `memberOf` constraint to place tasks on instances in the `databases` task group\. It can be specified with the following actions: [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html), [UpdateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html), [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html), and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)\.

```
"placementConstraints": [
    {
        "expression": "task:group == databases",
        "type": "memberOf"
    }
]
```

The `distinctInstance` constraint places each task in the group on a different instance\. It can be specified with the following actions: [CreateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html), [UpdateService](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_UpdateService.html), and [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)

```
"placementConstraints": [
    {
        "type": "distinctInstance"
    }
]
```