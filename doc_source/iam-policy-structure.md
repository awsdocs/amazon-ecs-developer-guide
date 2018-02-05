# Policy Structure<a name="iam-policy-structure"></a>

The following topics explain the structure of an IAM policy\.


+ [Policy Syntax](#policy-syntax)
+ [Actions for Amazon ECS](#UsingWithECS_Actions)
+ [Amazon Resource Names for Amazon ECS](#ECS_ARN_Format)
+ [Condition Keys for Amazon ECS](#amazon-ecs-keys)
+ [Checking that Users Have the Required Permissions](#check-required-permissions)

## Policy Syntax<a name="policy-syntax"></a>

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as follows:

```
{
  "Statement":[{
    "Effect":"effect",
    "Action":"action",
    "Resource":"arn",
    "Condition":{
      "condition":{
        "key":"value"
        }
      }
    }
  ]
}
```

There are various elements that make up a statement:

+ **Effect:** The *effect* can be `Allow` or `Deny`\. By default, IAM users don't have permission to use resources and API actions, so all requests are denied\. An explicit allow overrides the default\. An explicit deny overrides any allows\.

+ **Action**: The *action* is the specific API action for which you are granting or denying permission\. To learn about specifying *action*, see [Actions for Amazon ECS](#UsingWithECS_Actions)\. 

+ **Resource**: The resource that's affected by the action\. Some Amazon ECS API actions allow you to include specific resources in your policy that can be created or modified by the action\. To specify a resource in the statement, you need to use its Amazon Resource Name \(ARN\)\. For more information about specifying the *arn* value, see [Amazon Resource Names for Amazon ECS](#ECS_ARN_Format)\. For more information about which API actions support which ARNs, see [Supported Resource\-Level Permissions for Amazon ECS API Actions](ecs-supported-iam-actions-resources.md)\. If the API action does not support ARNs, use the \* wildcard to specify that all resources can be affected by the action\. 

+ **Condition**: Conditions are optional\. They can be used to control when your policy will be in effect\. For more information about specifying conditions for Amazon ECS, see [Condition Keys for Amazon ECS](#amazon-ecs-keys)\.

For more information about example IAM policy statements for Amazon ECS, see [Creating Amazon ECS IAM Policies](ECS_IAM_user_policies.md)\. 

## Actions for Amazon ECS<a name="UsingWithECS_Actions"></a>

In an IAM policy statement, you can specify any API action from any service that supports IAM\. For Amazon ECS, use the following prefix with the name of the API action: `ecs:`\. For example: `ecs:RunTask` and `ecs:CreateCluster`\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": ["ecs:action1", "ecs:action2"]
```

You can also specify multiple actions using wildcards\. For example, you can specify all actions whose name begins with the word "Describe" as follows:

```
"Action": "ecs:Describe*"
```

To specify all Amazon ECS API actions, use the \* wildcard as follows:

```
"Action": "ecs:*"
```

For a list of Amazon ECS actions, see [Actions](http://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Service API Reference*\.

## Amazon Resource Names for Amazon ECS<a name="ECS_ARN_Format"></a>

Each IAM policy statement applies to the resources that you specify using their ARNs\. 

**Important**  
Currently, not all API actions support individual ARNs; we'll add support for additional API actions and ARNs for additional Amazon ECS resources later\. For information about which ARNs you can use with which Amazon ECS API actions, as well as supported condition keys for each ARN, see [Supported Resource\-Level Permissions for Amazon ECS API Actions](ecs-supported-iam-actions-resources.md)\. 

An ARN has the following general syntax:

```
arn:aws:[service]:[region]:[account]:resourceType/resourcePath
```

*service*  
The service \(for example, `ecs`\)\.

*region*  
The region for the resource \(for example, `us-east-1`\)\.

*account*  
The AWS account ID, with no hyphens \(for example, `123456789012`\)\.

*resourceType*  
The type of resource \(for example, `instance`\)\.

*resourcePath*  
A path that identifies the resource\. You can use the \* wildcard in your paths\.

For example, you can indicate a specific cluster \(`default`\) in your statement using its ARN as follows: 

```
"Resource": "arn:aws:ecs:us-east-1:123456789012:cluster/default"
```

You can also specify all clusters that belong to a specific account by using the \* wildcard as follows:

```
"Resource": "arn:aws:ecs:us-east-1:123456789012:cluster/*"
```

To specify all resources, or if a specific API action does not support ARNs, use the \* wildcard in the `Resource` element as follows:

```
"Resource": "*"
```

The following table describes the ARNs for each type of resource used by the Amazon ECS API actions\. 


| Resource Type | ARN | 
| --- | --- | 
|  All Amazon ECS resources  |  arn:aws:ecs:\*  | 
|  All Amazon ECS resources owned by the specified account in the specified region  |  arn:aws:ecs:*region*:*account*:\*  | 
|  Cluster  |  arn:aws:ecs:*region*:*account*:cluster/*cluster\-name*  | 
|  Container instance  |  arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  | 
|  Task definition  |  arn:aws:ecs:*region*:*account*:task\-definition/*task\-definition\-family\-name*:*task\-definition\-revision\-number*  | 
|  Service  |  arn:aws:ecs:*region*:*account*:service/*service\-name*  | 
|  Task  |  arn:aws:ecs:*region*:*account*:task/*task\-id*  | 
|  Container  |  arn:aws:ecs:*region*:*account*:container/*container\-id*  | 

Many Amazon ECS API actions accept multiple resources\. To specify multiple resources in a single statement, separate their ARNs with commas, as follows:

```
"Resource": ["arn1", "arn2"]
```

For more general information about ARNs, see [Amazon Resource Names \(ARN\) and AWS Service Namespaces](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\. 

## Condition Keys for Amazon ECS<a name="amazon-ecs-keys"></a>

In a policy statement, you can optionally specify conditions that control when it is in effect\. Each condition contains one or more key\-value pairs\. Condition keys are not case\-sensitive\. We've defined AWS\-wide condition keys, plus additional service\-specific condition keys\.

If you specify multiple conditions, or multiple keys in a single condition, we evaluate them using a logical AND operation\. If you specify a single condition with multiple values for one key, we evaluate the condition using a logical OR operation\. For permission to be granted, all conditions must be met\.

You can also use placeholders when you specify conditions\. For more information, see [Policy Variables](http://docs.aws.amazon.com/IAM/latest/UserGuide/PolicyVariables.html) in the *IAM User Guide*\.

Amazon ECS implements the AWS\-wide condition keys \(see [Available Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#AvailableKeys)\), plus the following service\-specific condition keys\. \(We'll add support for additional service\-specific condition keys for Amazon ECS later\.\)


| Condition Key | Key/Value Pair | Evaluation Types | 
| --- | --- | --- | 
|  ecs:cluster  |  "ecs:cluster":"*cluster\-arn*" Where *cluster\-arn* is the ARN for the Amazon ECS cluster  |  ARN, Null  | 
|  ecs:container\-instances  |  "ecs:container\-instances":"*container\-instance\-arns*" Where *container\-instance\-arns* is one or more container instance ARNs\.  |  ARN, Null  | 

For information about which condition keys you can use with which Amazon ECS resources, on an action\-by\-action basis, see [Supported Resource\-Level Permissions for Amazon ECS API Actions](ecs-supported-iam-actions-resources.md)\. For example policy statements for Amazon ECS, see [Creating Amazon ECS IAM Policies](ECS_IAM_user_policies.md)\.

## Checking that Users Have the Required Permissions<a name="check-required-permissions"></a>

After you've created an IAM policy, we recommend that you check whether it grants users the permissions to use the particular API actions and resources they need before you put the policy into production\.

First, create an IAM user for testing purposes, and then attach the IAM policy that you created to the test user\. Then, make a request as the test user\. You can make test requests in the console or with the AWS CLI\. 

**Note**  
You can also test your policies with the [IAM Policy Simulator](https://policysim.aws.amazon.com/home/index.jsp?#)\. For more information on the policy simulator, see [Working with the IAM Policy Simulator](http://docs.aws.amazon.com/IAM/latest/UserGuide/policies_testing-policies.html) in the *IAM User Guide*\.

If the action that you are testing creates or modifies a resource, you should make the request using the `DryRun` parameter \(or run the AWS CLI command with the `--dry-run` option\)\. In this case, the call completes the authorization check, but does not complete the operation\. For example, you can check whether the user can terminate a particular instance without actually terminating it\. If the test user has the required permissions, the request returns `DryRunOperation`; otherwise, it returns `UnauthorizedOperation`\.

If the policy doesn't grant the user the permissions that you expected, or is overly permissive, you can adjust the policy as needed and retest until you get the desired results\. 

**Important**  
It can take several minutes for policy changes to propagate before they take effect\. Therefore, we recommend that you allow five minutes to pass before you test your policy updates\.

If an authorization check fails, the request returns an encoded message with diagnostic information\. You can decode the message using the `DecodeAuthorizationMessage` action\. For more information, see [DecodeAuthorizationMessage](http://docs.aws.amazon.com/STS/latest/APIReference/API_DecodeAuthorizationMessage.html) in the *AWS Security Token Service API Reference*, and [decode\-authorization\-message](http://docs.aws.amazon.com/cli/latest/reference/sts/decode-authorization-message.html) in the *AWS CLI Command Reference*\.