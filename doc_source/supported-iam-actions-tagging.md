# Grant permission to tag resources on creation<a name="supported-iam-actions-tagging"></a>

The following tag\-on create Amazon ECS API actions allow you to specify tags when you create the resource\. If tags are specified in the resource\-creating action, AWS performs additional authorization to verify that the correct permissions are assigned to create tags\. 
+ `CreateCapacityProvider`
+ `CreateCluster`
+ `CreateService`
+ `CreateTaskSet`
+ `RegisterContainerInstance`
+ `RegisterTaskDefinition`
+ `RunTask`
+ `StartTask`

You can use resource tags to implement attribute\-based control \(ABAC\)\. For more information, see [Control access to Amazon ECS resources using resource tags](control-access-with-tags.md) and [Tagging your Amazon ECS resources](ecs-using-tags.md)\.

To allow tagging on creation, create or modify a policy to include both the permissions to use the action that creates the resource, such as `ecs:CreateCluster` or `ecs:RunTask` and the `ecs:TagResource` action\. 

The following example demonstrates a policy that allows users to create clusters and run tasks, but can only add tags during the cluster creation\. Users are not permitted to tag any existing resources \(they cannot call the `ecs:TagResource` action directly\)\.

```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
         "ecs:CreateCluster",
         "ecs:RunTask"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
         "ecs:TagResource"
      ],
      "Resource": "*",
      "Condition": {
         "StringEquals": {
             "ecs:CreateAction" : "CreateCluster"
          }
       }
    }
  ]
}
```

The `ecs:TagResource` action is only evaluated if tags are applied during the resource\-creating action\. Therefore, a user that has permissions to create a resource \(assuming there are no tagging conditions\) does not require permissions to use the `ecs:TagResource` action if no tags are specified in the request\. However, if the user attempts to create a resource with tags, the request fails if the user does not have permissions to use the `ecs:TagResource` action\.

## Control access to specific tags<a name="control-tagging"></a>

You can use additional conditions in the `Condition` element of your IAM policies to control the tag keys and values that can be applied to resources\.

The following condition keys can be used with the examples in the preceding section:
+ `aws:RequestTag`: To indicate that a particular tag key or tag key and value must be present in a request\. Other tags can also be specified in the request\.
  + Use with the `StringEquals` condition operator to enforce a specific tag key and value combination, for example, to enforce the tag `cost-center`=`cc123`:

    ```
    "StringEquals": { "aws:RequestTag/cost-center": "cc123" }
    ```
  + Use with the `StringLike` condition operator to enforce a specific tag key in the request; for example, to enforce the tag key `purpose`:

    ```
    "StringLike": { "aws:RequestTag/purpose": "*" }
    ```
+ `aws:TagKeys`: To enforce the tag keys that are used in the request\.
  + Use with the `ForAllValues` modifier to enforce specific tag keys if they are provided in the request \(if tags are specified in the request, only specific tag keys are allowed; no other tags are allowed\)\. For example, the tag keys `environment` or `cost-center` are allowed:

    ```
    "ForAllValues:StringEquals": { "aws:TagKeys": ["environment","cost-center"] }
    ```
  + Use with the `ForAnyValue` modifier to enforce the presence of at least one of the specified tag keys in the request\. For example, at least one of the tag keys `environment` or `webserver` must be present in the request:

    ```
    "ForAnyValue:StringEquals": { "aws:TagKeys": ["environment","webserver"] }
    ```

These condition keys can be applied to resource\-creating actions that support tagging, as well as the `ecs:TagResource` action\. To learn whether an Amazon ECS API action supports tagging, see [Actions, resources, and condition keys for Amazon ECS](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonecs.html)\.

To force users to specify tags when they create a resource, you must use the `aws:RequestTag` condition key or the `aws:TagKeys` condition key with the `ForAnyValue` modifier on the resource\-creating action\. The `ecs:TagResource` action is not evaluated if a user does not specify tags for the resource\-creating action\.

For conditions, the condition key is not case\-sensitive and the condition value is case\-sensitive\. Therefore, to enforce the case\-sensitivity of a tag key, use the `aws:TagKeys` condition key, where the tag key is specified as a value in the condition\.

 For more information about multi\-value conditions, see [Creating a Condition That Tests Multiple Key Values](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_multi-value-conditions.html) in the *IAM User Guide*\. 