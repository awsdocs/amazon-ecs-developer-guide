# Amazon ECS Task Placement Constraints<a name="task-placement-constraints"></a>

A *task placement constraint* is a rule that is considered during task placement\. For more information, see [Amazon ECS Task Placement](task-placement.md)\.

## Constraint Types<a name="constraint-types"></a>

Amazon ECS supports the following types of task placement constraints:

`distinctInstance`  
Place each task on a different container instance\.

`memberOf`  
Place tasks on container instances that satisfy an expression\.

For more information about expression syntax, see [Cluster Query Language](cluster-query-language.md)\.

**Note**
The placement constraint type `distinctInstance` is not supported when using task placement constraints on a TaskDefinition. 

## Attributes<a name="attributes"></a>

You can add custom metadata to your container instances, known as *attributes*\. Each attribute has a name and an optional string value\. You can use the built\-in attributes provided by Amazon ECS or define custom attributes\.Built\-in Attributes

Amazon ECS automatically applies the following attributes to your container instances\.

`ecs.ami-id`  
The ID of the AMI used to launch the instance\. An example value for this attribute is "ami\-eca289fb"\.

`ecs.availability-zone`  
The Availability Zone for the instance\. An example value for this attribute is "us\-east\-1a"\.

`ecs.instance-type`  
The instance type for the instance\. An example value for this attribute is "g2\.2xlarge"\.

`ecs.os-type`  
The operating system for the instance\. The possible values for this attribute are "linux" and "windows"\.

**Custom Attributes**  
You can apply custom attributes to your container instances\. For example, you can define an attribute with the name "stack" and a value of "prod"\.

## Adding an Attribute<a name="add-attribute"></a>

You can add custom attributes at instance registration time using the container agent or manually, using the AWS Management Console\. For more information about using the container agent, see [Amazon ECS Container Agent Configuration Parameters](ecs-agent-config.md#ecs-instance-attributes)\.

**To add custom attributes using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Clusters** and select a cluster\.

1. On the **ECS Instances** tab, select the check box for the container instance\.

1. Choose **Actions**, **View/Edit Attributes**\.

1. For each attribute, do the following:

   1. Choose **Add attribute**\.

   1. Type a name and a value for the attribute\.

   1. Choose the checkmark icon to save the attribute\.

1. When you are finished adding attributes, choose **Close**\.

**Adding custom attributes using the AWS CLI**  
The following examples demonstrate how to add custom attributes using the [put\-attributes](http://docs.aws.amazon.com/cli/latest/reference/ecs/put-attributes.html) command\.

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

## Filtering by Attribute<a name="filter-attribute"></a>

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
The following examples demonstrate how to filter container instances by attribute using the [list\-constainer\-instances](http://docs.aws.amazon.com/cli/latest/reference/ecs/list-container-instances.html) command\. For more information about the filter syntax, see [Cluster Query Language](cluster-query-language.md)\.

**Example: Built\-in Attribute**  
The following example uses built\-in attributes to list the g2\.2xlarge instances\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type == g2.2xlarge"
```

**Example: Custom Attribute**  
The following example lists the instances with the custom attribute "stack=prod"\.

```
aws ecs list-container-instances --filter "attribute:stack == prod"
```

**Example: Exclude an Attribute Value**  
The following example lists the instances with the custom attribute "stack" unless the attribute value is "prod"\.

```
aws ecs list-container-instances --filter "attribute:stack != prod"
```

**Example: Multiple Attribute Values**  
The following example uses built\-in attributes to list the instances of type t2\.small or t2\.medium\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type in [t2.small, t2.medium]"
```

**Example: Multiple Attributes**  
The following example uses built\-in attributes to list the T2 instances in Availability Zone us\-east\-1a\.

```
aws ecs list-container-instances --filter "attribute:ecs.instance-type =~ t2.* and attribute:ecs.availability-zone == us-east-1a"
```

## Task Groups<a name="task-groups"></a>

You can identify a set of related tasks as a *task group*\. All tasks with the same task group name are considered as a set when performing spread placement\. For example, suppose that you are running different applications in one cluster, such as databases and web servers\. To ensure that your databases are balanced across Availability Zones, add them to a task group named "databases" and then use this task group as a constraint for task placement\.

When you launch a task using the `RunTask` or `StartTask` action, you can specify the name of the task group for the task\. If you don't specify a task group for the task, the default name is the family name of the task definition \(for example, family:my\-task\-definition\)\.

For tasks launched by the service scheduler, the task group name is the name of the service \(for example, service:my\-service\-name\)\.

**Limits**
+ A task group name must be 255 characters or less\.
+ Each task can be in exactly one group\.
+ After launching a task, you cannot modify its task group\.

## Example Constraints<a name="constraint-examples"></a>

You can specify task placement constraints with the following actions: [CreateService](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_CreateService.html), [RegisterTaskDefinition](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html), and [RunTask](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html)\.

The following constraint places tasks on T2 instances\.

```
"placementConstraints": [
    {
        "expression": "attribute:ecs.instance-type =~ t2.*",
        "type": "memberOf"
    }
]
```

The following constraint places tasks on instances in the databases task group\.

```
"placementConstraints": [
    {
        "expression": "task:group == databases",
        "type": "memberOf"
    }
]
```

The following constraint places each task in the group on a different instance\. This type of constraint is not supported when performing [RegisterTaskDefinition](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html)\.

```
"placementConstraints": [
    {
        "type": "distinctInstance"
    }
]
```
