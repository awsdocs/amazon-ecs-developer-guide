# How Amazon Elastic Container Service works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Amazon ECS, learn what IAM features are available to use with Amazon ECS\.






**IAM features you can use with Amazon Elastic Container Service**  

| IAM feature | Amazon ECS support | 
| --- | --- | 
|  [Identity\-based policies](#security_iam_service-with-iam-id-based-policies)  |    Yes  | 
|  [Resource\-based policies](#security_iam_service-with-iam-resource-based-policies)  |    No   | 
|  [Policy actions](#security_iam_service-with-iam-id-based-policies-actions)  |    Yes  | 
|  [Policy resources](#security_iam_service-with-iam-id-based-policies-resources)  |    Partial  | 
|  [Policy condition keys](#security_iam_service-with-iam-id-based-policies-conditionkeys)  |    Yes  | 
|  [ACLs](#security_iam_service-with-iam-acls)  |    No   | 
|  [ABAC \(tags in policies\)](#security_iam_service-with-iam-tags)  |    Yes  | 
|  [Temporary credentials](#security_iam_service-with-iam-roles-tempcreds)  |    Yes  | 
|  [Principal permissions](#security_iam_service-with-iam-principal-permissions)  |    Yes  | 
|  [Service roles](#security_iam_service-with-iam-roles-service)  |    Yes  | 
|  [Service\-linked roles](#security_iam_service-with-iam-roles-service-linked)  |    Yes  | 

To get a high\-level view of how Amazon ECS and other AWS services work with most IAM features, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

## Identity\-based policies for Amazon ECS<a name="security_iam_service-with-iam-id-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports identity\-based policies  |    Yes  | 

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, group of users, or role\. These policies control what actions users and roles can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. You can't specify the principal in an identity\-based policy because it applies to the user or role to which it is attached\. To learn about all of the elements that you can use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Identity\-based policy examples for Amazon ECS<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Amazon ECS identity\-based policies, see [Identity\-based policy examples for Amazon Elastic Container Service](security_iam_id-based-policy-examples.md)\.

## Resource\-based policies within Amazon ECS<a name="security_iam_service-with-iam-resource-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports resource\-based policies  |    No   | 

Resource\-based policies are JSON policy documents that you attach to a resource\. Examples of resource\-based policies are IAM *role trust policies* and Amazon S3 *bucket policies*\. In services that support resource\-based policies, service administrators can use them to control access to a specific resource\. For the resource where the policy is attached, the policy defines what actions a specified principal can perform on that resource and under what conditions\. You must [specify a principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in a resource\-based policy\. Principals can include accounts, users, roles, federated users, or AWS services\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the principal in a resource\-based policy\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, an IAM administrator in the trusted account must also grant the principal entity \(user or role\) permission to access the resource\. They grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM roles differ from resource\-based policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

## Policy actions for Amazon ECS<a name="security_iam_service-with-iam-id-based-policies-actions"></a>


|  |  | 
| --- |--- |
|  Supports policy actions  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.



To see a list of Amazon ECS actions, see [Actions defined by Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html#amazonelasticcontainerservice-actions-as-permissions) in the *Service Authorization Reference*\.

Policy actions in Amazon ECS use the following prefix before the action:

```
ecs
```

To specify multiple actions in a single statement, separate them with commas\.

```
"Action": [
      "ecs:action1",
      "ecs:action2"
         ]
```





You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ecs:Describe*"
```

To view examples of Amazon ECS identity\-based policies, see [Identity\-based policy examples for Amazon Elastic Container Service](security_iam_id-based-policy-examples.md)\.

## Policy resources for Amazon ECS<a name="security_iam_service-with-iam-id-based-policies-resources"></a>


|  |  | 
| --- |--- |
|  Supports policy resources  |    Partial  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```

To see a list of Amazon ECS resource types and their ARNs, see [Resources defined by Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html#amazonelasticcontainerservice-resources-for-iam-policies) in the *Service Authorization Reference*\. To learn with which actions you can specify the ARN of each resource, see [Actions defined by Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html#amazonelasticcontainerservice-actions-as-permissions)\.





Some Amazon ECS API actions support multiple resources\. For example, multiple clusters can be referenced when calling the `DescribeClusters` API action\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "EXAMPLE-RESOURCE-1",
      "EXAMPLE-RESOURCE-2"
```

For example, the Amazon ECS cluster resource has the following ARN:

```
arn:${Partition}:ecs:${Region}:${Account}:cluster/${clusterName}
```

To specify `my-cluster-1` and `my-cluster-2` cluster in your statement, use the following ARNs:

```
"Resource": [
         "arn:aws:ecs:us-east-1:123456789012:cluster/my-cluster-1",
         "arn:aws:ecs:us-east-1:123456789012:cluster/my-cluster-2"
```

To specify all clusters that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:ecs:us-east-1:123456789012:cluster/*"
```

For task definitions, you can specify the latest revision, or a specific revision\.

To specify the latest task definition, use:

```
"Resource:arn:${Partition}:ecs:${Region}:${Account}:task-definition/${TaskDefinitionFamilyName}"
```

To specify a specific task definition revision, use $\{TaskDefinitionRevisionNumber\}:

```
"Resource:arn:${Partition}:ecs:${Region}:${Account}:task-definition/${TaskDefinitionFamilyName}:${TaskDefinitionRevisionNumber}"
```

To view examples of Amazon ECS identity\-based policies, see [Identity\-based policy examples for Amazon Elastic Container Service](security_iam_id-based-policy-examples.md)\.

## Policy condition keys for Amazon ECS<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>


|  |  | 
| --- |--- |
|  Supports service\-specific policy condition keys  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

Amazon ECS supports the following service\-specific condition keys that you can use to provide fine\-grained filtering for your IAM policies:


| Condition Key | Description | Evaluation Types | 
| --- | --- | --- | 
|  aws:RequestTag/$\{TagKey\}  | The context key is formatted `"aws:RequestTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag key–value pair is present in an AWS request\. For example, you could check to see that the request includes the tag key `"Dept"` and that it has the value `"Accounting"`\.  |  String  | 
|  aws:ResourceTag/$\{TagKey\}  |  The context key is formatted `"aws:ResourceTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag attached to the identity resource \(user or role\) matches the specified key name and value\.  | String | 
|  aws:TagKeys  |  This context key is formatted `"aws:TagKeys":"tag-key"` where *tag\-key* is a list of tag keys without values \(for example, `["Dept","Cost-Center"]`\)\. Checks the tag keys that are present in an AWS request\.  | String | 
|  ecs:ResourceTag/$\{TagKey\}  |  The context key is formatted `"ecs:ResourceTag/tag-key":"tag-value"` where *tag\-key*and *tag\-value* are a tag key and value pair\. Checks that the tag attached to the identity resource \(user or role\) matches the specified key name and value\.  | String | 
|  ecs:cluster  |  The context key is formatted `"ecs:cluster":"cluster-arn"` where *cluster\-arn* is the ARN for the Amazon ECS cluster\.  |  ARN, Null  | 
|  ecs:container\-instances  |  The context key is formatted `"ecs:container-instances":"container-instance-arns"` where *container\-instance\-arns* is one or more container instance ARNs\.  |  ARN, Null  | 
| ecs:container\-name |  The context key is formatted `"ecs:container-name":"container-name"` where *container\-instance\-* is the name of an Amazon ECS container which is defined in the ECS task definition\.   | String | 
| ecs:enable\-execute\-command | The context key is formatted "ecs:enable\-execute\-command":"value" where value\- is "true" or "false"\. | String | 
|  ecs:task\-definition  |  The context key is formatted `"ecs:task-definition":"task-definition-arn"` where *task\-definition\-arn* is the ARN for the Amazon ECS task definition\.  | ARN, Null | 
|  ecs:service  |  The context key is formatted `"ecs:service":"service-arn"` where *service\-arn* is the ARN for the Amazon ECS service\.  | ARN, Null | 

To see a list of Amazon ECS condition keys, see [Condition keys for Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html#amazonelasticcontainerservice-policy-keys) in the *Service Authorization Reference*\. To learn with which actions and resources you can use a condition key, see [Actions defined by Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html#amazonelasticcontainerservice-actions-as-permissions)\.

To view examples of Amazon ECS identity\-based policies, see [Identity\-based policy examples for Amazon Elastic Container Service](security_iam_id-based-policy-examples.md)\.

## Access control lists \(ACLs\) in Amazon ECS<a name="security_iam_service-with-iam-acls"></a>


|  |  | 
| --- |--- |
|  Supports ACLs  |    No   | 

Access control lists \(ACLs\) control which principals \(account members, users, or roles\) have permissions to access a resource\. ACLs are similar to resource\-based policies, although they do not use the JSON policy document format\.

## Attribute\-based access control \(ABAC\) with Amazon ECS<a name="security_iam_service-with-iam-tags"></a>


|  |  | 
| --- |--- |
|  Supports ABAC \(tags in policies\)  |    Yes  | 

Attribute\-based access control \(ABAC\) is an authorization strategy that defines permissions based on attributes\. In AWS, these attributes are called *tags*\. You can attach tags to IAM entities \(users or roles\) and to many AWS resources\. Tagging entities and resources is the first step of ABAC\. Then you design ABAC policies to allow operations when the principal's tag matches the tag on the resource that they are trying to access\.

ABAC is helpful in environments that are growing rapidly and helps with situations where policy management becomes cumbersome\.

To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `aws:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\.

For more information about ABAC, see [What is ABAC?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html) in the *IAM User Guide*\. To view a tutorial with steps for setting up ABAC, see [Use attribute\-based access control \(ABAC\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html) in the *IAM User Guide*\.

For more information about tagging Amazon ECS resources, see [Resources and tags](ecs-resource-tagging.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Describing Amazon ECS Services Based on Tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-cluster-tags)\.

## Using Temporary credentials with Amazon ECS<a name="security_iam_service-with-iam-roles-tempcreds"></a>


|  |  | 
| --- |--- |
|  Supports temporary credentials  |    Yes  | 

Some AWS services don't work when you sign in using temporary credentials\. For additional information, including which AWS services work with temporary credentials, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

You are using temporary credentials if you sign in to the AWS Management Console using any method except a user name and password\. For example, when you access AWS using your company's single sign\-on \(SSO\) link, that process automatically creates temporary credentials\. You also automatically create temporary credentials when you sign in to the console as a user and then switch roles\. For more information about switching roles, see [Switching to a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) in the *IAM User Guide*\.

You can manually create temporary credentials using the AWS CLI or AWS API\. You can then use those temporary credentials to access AWS\. AWS recommends that you dynamically generate temporary credentials instead of using long\-term access keys\. For more information, see [Temporary security credentials in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\.

## Cross\-service principal permissions for Amazon ECS<a name="security_iam_service-with-iam-principal-permissions"></a>


|  |  | 
| --- |--- |
|  Supports principal permissions  |    Yes  | 

  When you use an IAM user or role to perform actions in AWS, you are considered a principal\. Policies grant permissions to a principal\. When you use some services, you might perform an action that then triggers another action in a different service\. In this case, you must have permissions to perform both actions\. To see whether an action requires additional dependent actions in a policy, see [Actions, resources, and condition keys for Amazon Elastic Container Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonelasticcontainerservice.html) in the *Service Authorization Reference*\. 

## Service roles for Amazon ECS<a name="security_iam_service-with-iam-roles-service"></a>


|  |  | 
| --- |--- |
|  Supports service roles  |    Yes  | 

  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

**Warning**  
Changing the permissions for a service role might break Amazon ECS functionality\. Edit service roles only when Amazon ECS provides guidance to do so\.

## Service\-linked roles for Amazon ECS<a name="security_iam_service-with-iam-roles-service-linked"></a>


|  |  | 
| --- |--- |
|  Supports service\-linked roles  |    Yes  | 

  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 

For details about creating or managing Amazon ECS service\-linked roles, see [Using service\-linked roles for Amazon ECS](using-service-linked-roles.md)\.