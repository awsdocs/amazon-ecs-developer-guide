# How Amazon Elastic Container Service works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Amazon ECS, you should understand what IAM features are available to use with Amazon ECS\. To get a high\-level view of how Amazon ECS and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Amazon ECS identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [Amazon ECS resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Supported resource\-level permissions for Amazon ECS API actions](#ecs-supported-iam-actions-resources)
+ [Authorization based on Amazon ECS tags](#security_iam_service-with-iam-tags)
+ [Amazon ECS IAM roles](#security_iam_service-with-iam-roles)

## Amazon ECS identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. Amazon ECS supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in Amazon ECS use the following prefix before the action: `ecs:`\. For example, to grant someone permission to create an Amazon ECS cluster with the Amazon ECS `CreateCluster` API operation, you include the `ecs:CreateCluster` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. Amazon ECS defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ecs:action1",
      "ecs:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ecs:Describe*"
```



To see a list of Amazon ECS actions, see [Actions, Resources, and Condition Keys for Amazon Elastic Container Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerservice.html) in the *IAM User Guide*

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



The Amazon ECS cluster resource has the following ARN:

```
arn:${Partition}:ecs:${Region}:${Account}:cluster/${clusterName}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `my-cluster` cluster in your statement, use the following ARN:

```
"Resource": "arn:aws:ecs:us-east-1:123456789012:cluster/my-cluster"
```

To specify all clusters that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:ecs:us-east-1:123456789012:cluster/*"
```

Some Amazon ECS actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Some Amazon ECS API actions can be performed on multiple resources\. For example, multiple clusters can be referenced when calling the `DescribeClusters` API action\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

The following table describes the ARNs for each resource type used by the Amazon ECS API actions\.

**Important**  
The following table uses the new longer ARN format for Amazon ECS tasks, services, and container instances\. If you have not opted in to the long ARN format, the ARNs will not include the cluster name\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\.


| Resource Type | ARN | 
| --- | --- | 
|  All Amazon ECS resources  |  arn:aws:ecs:\*  | 
|  All Amazon ECS resources owned by the specified account in the specified region  |  arn:aws:ecs:*region*:*account*:\*  | 
|  Cluster  |  arn:aws:ecs:*region*:*account*:cluster/*cluster\-name*  | 
|  Container instance  |  arn:aws:ecs:*region*:*account*:container\-instance/*cluster\-name*/*container\-instance\-id*  | 
|  Task definition  |  arn:aws:ecs:*region*:*account*:task\-definition/*task\-definition\-family\-name*:*task\-definition\-revision\-number*  | 
|  Service  |  arn:aws:ecs:*region*:*account*:service/*cluster\-name*/*service\-name*  | 
|  Task  |  arn:aws:ecs:*region*:*account*:task/*cluster\-name*/*task\-id*  | 
|  Container  |  arn:aws:ecs:*region*:*account*:container/*cluster\-name*/*task\-id*/*container\-id*  | 

To learn with which actions you can specify the ARN of each resource, see [Supported resource\-level permissions for Amazon ECS API actions](#ecs-supported-iam-actions-resources)\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

Amazon ECS defines its own set of condition keys and also supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



Amazon ECS implements the following service\-specific condition keys\. 


| Condition Key | Description | Evaluation Types | 
| --- | --- | --- | 
|  aws:RequestTag/$\{TagKey\}  | The context key is formatted `"aws:RequestTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag key–value pair is present in an AWS request\. For example, you could check to see that the request includes the tag key `"Dept"` and that it has the value `"Accounting"`\.  |  String  | 
|  aws:ResourceTag/$\{TagKey\}  |  The context key is formatted `"aws:ResourceTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag attached to the identity resource \(user or role\) matches the specified key name and value\.  | String | 
|  aws:TagKeys  |  This context key is formatted `"aws:TagKeys":"tag-key"` where *tag\-key* is a list of tag keys without values \(for example, `["Dept","Cost-Center"]`\)\. Checks the tag keys that are present in an AWS request\.  | String | 
|  ecs:ResourceTag/$\{TagKey\}  |  The context key is formatted `"ecs:ResourceTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag attached to the identity resource \(user or role\) matches the specified key name and value\.  | String | 
|  ecs:cluster  |  The context key is formatted `"ecs:cluster":"cluster-arn"` where *cluster\-arn* is the ARN for the Amazon ECS cluster\.  |  ARN, Null  | 
|  ecs:container\-instances  |  The context key is formatted `"ecs:container-instances":"container-instance-arns"` where *container\-instance\-arns* is one or more container instance ARNs\.  |  ARN, Null  | 
|  ecs:task\-definition  |  The context key is formatted `"ecs:task-definition":"task-definition-arn"` where *task\-definition\-arn* is the ARN for the Amazon ECS task definition\.  | ARN, Null | 
|  ecs:service  |  The context key is formatted `"ecs:service":"service-arn"` where *service\-arn* is the ARN for the Amazon ECS service\.  | ARN, Null | 

To learn with which actions and resources you can use a condition key, see [Supported resource\-level permissions for Amazon ECS API actions](#ecs-supported-iam-actions-resources)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Amazon ECS identity\-based policies, see [Amazon Elastic Container Service identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## Amazon ECS resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Amazon ECS does not support resource\-based policies\.

## Supported resource\-level permissions for Amazon ECS API actions<a name="ecs-supported-iam-actions-resources"></a>

The term *resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

For more information about the resources that are created or modified by the Amazon ECS actions, and the ARNs and Amazon ECS condition keys that you can use in an IAM policy statement, see [Actions, Resources, and Condition Keys for Amazon Elastic Container Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerservice.html) in the *IAM User Guide*\.

## Authorization based on Amazon ECS tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to Amazon ECS resources or pass tags in a request to Amazon ECS\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the  `aws:RequestTag/key-name` or `aws:TagKeys` condition keys\. For more information, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\.

For more information about tagging Amazon ECS resources, see [Resources and tags](ecs-resource-tagging.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Describing Amazon ECS Services Based on Tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-cluster-tags)\.

## Amazon ECS IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with Amazon ECS<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\.

Amazon ECS supports using temporary credentials\.

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

Amazon ECS supports service\-linked roles\. For details about creating or managing Amazon ECS service\-linked roles, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

Amazon ECS supports service roles\.