# Supported Resource\-Level Permissions for Amazon ECS API Actions<a name="ecs-supported-iam-actions-resources"></a>

The term *resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

For more information about the resources that are created or modified by the Amazon ECS actions, and the ARNs and Amazon ECS condition keys that you can use in an IAM policy statement, see [Actions, Resources, and Condition Keys for Amazon Elastic Container Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonelasticcontainerservice.html) in the *IAM User Guide*\.

## Considerations for Resource\-Level Permissions<a name="rlp-considerations"></a>

When controlling access to Amazon ECS API actions by specifying the Amazon Resource Name \(ARN\) of a resource in an IAM policy, be mindful that ECS has introduced an account setting that affects the ARN format for container instances, services, and tasks\. To use resource\-level permissions, we recommend that you opt\-in to the new, longer ARN format\. For more information, see [Amazon Resource Names \(ARNs\) and IDs](ecs-account-settings.md#ecs-resource-ids)\.

When an IAM policy is evaluated, the specified resources are evaluated based on their use of the new, longer ARN format\. The following are examples of how access is controlled\.

### Specifying a Service with a Cluster Only with a Wildcard<a name="rlp-example1"></a>

Example: `arn:aws:ecs:region:aws_account_id:service/cluster_name*`

In this example, access will be controlled to the following services:
+ All services using the new ARN format that are in the `cluster_name*` cluster\.
+ All services using the old ARN format that are in the `cluster_name*` cluster\.

**Important**  
This will **NOT** control access to services using the old ARN format that have a service name with the `cluster_name` prefix that are not in the `cluster_name*` cluster\.

### Specifying a Service with both a Cluster and Service Name with a Wildcard<a name="rlp-example2"></a>

Example: `arn:aws:ecs:region:aws_account_id:service/cluster_name/service_name*`

In this example, access will be controlled to the following services:
+ All services using the new ARN format that are in the `cluster_name` cluster with the `service_name` prefix\.
+ All services using the old ARN format that are in the `cluster_name` cluster with the `service_name` prefix, even though the actual ARN of the service will still have the `arn:aws:ecs:region:aws_account_id:service/service_name*` ARN format\.

### Specifying a Service with a full ARN<a name="rlp-example3"></a>

Example: `arn:aws:ecs:region:aws_account_id:service/cluster_name/service_name`

In this example, access will be controlled to the following services:
+ All services using the new ARN format that are in the `cluster_name` cluster with the `service_name` service name\.
+ All services using the old ARN format that are in the `cluster_name` cluster with the `service_name` service name, even though the actual ARN of the service will still have the `arn:aws:ecs:region:aws_account_id:service/service_name` ARN format\.