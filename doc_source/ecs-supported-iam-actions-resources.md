# Supported Resource\-Level Permissions for Amazon ECS API Actions<a name="ecs-supported-iam-actions-resources"></a>

The term *resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

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

## Resource\-Level Permissions<a name="rlp-table"></a>

The following table describes the Amazon ECS API actions that currently support resource\-level permissions, as well as the supported resources, resource ARNs, and condition keys for each action\.

**Important**  
If an Amazon ECS API action in this table is marked `N/A` for `Resource` and `Condition keys`, then it does not support resource\-level permissions\. You can grant users permission to use the action, but you should specify a `*` for the resource element of your policy statement\.


| API action | Resource | Condition keys | 
| --- | --- | --- | 
|  CreateCluster  | N/A |  aws:RequestTag/$\{TagKey\} aws:TagKeys  | 
|  CreateService  | Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name* |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag ecs:task\-definition ecs:cluster aws:TagKeys  | 
|  CreateTaskSet  |  N/A  |  ecs:service ecs:task\-definition ecs:cluster  | 
|  DeleteAccountSetting  |  N/A  |  N/A  | 
|  DeleteAttributes  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DeleteCluster  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DeleteService  | Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DeleteTaskSet  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:task\-set/*cluster\-name*/*service\-name*/*taskset\-id*  |  ecs:service ecs:cluster  | 
|  DeregisterContainerInstance  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DeregisterTaskDefinition  |  N/A  |  N/A  | 
|  DescribeClusters  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster1*, arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster2*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DescribeContainerInstances  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeServices  | Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeTaskDefinition  |  N/A  |  N/A  | 
|  DescribeTasks  | Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*task\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeTaskSets  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:task\-set/*cluster\-name*/*service\-name*/*taskset\-id*  |  ecs:service ecs:cluster  | 
|  DiscoverPollEndpoint  |  N/A  |  N/A  | 
|  ListAccountSettings  |  N/A  |  N/A  | 
|  ListAttributes  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
| ListClusters | N/A | N/A | 
|  ListContainerInstances  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  ListServices  |  N/A  |  ecs:cluster  | 
|  ListTagsForResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*task\-id* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:ResourceTag ecs:ResourceTag  | 
|  ListTaskDefinitionFamilies  |  N/A  |  N/A  | 
|  ListTaskDefinitions  |  N/A  |  N/A  | 
|  ListTasks  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  Poll  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  PutAccountSetting  |  N/A  |  N/A  | 
|  PutAccountSettingDefault  |  N/A  |  N/A  | 
|  PutAttributes  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  RegisterContainerInstance  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  RegisterTaskDefinition  |  N/A  |  aws:RequestTag/$\{TagKey\} aws:TagKeys  | 
|  RunTask  |  Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys ecs:cluster  | 
|  StartTask  |  Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys ecs:cluster ecs:container\-instances  | 
|  StartTelemetrySession  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  StopTask  | Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*task\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  SubmitContainerStateChange  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  SubmitTaskStateChange  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  TagResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*task\-id* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8* Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  UntagResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*task\-id* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8* Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  UpdateContainerAgent  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateContainerInstancesState  | Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateService  | Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name* |  aws:ResourceTag ecs:ResourceTag ecs:cluster ecs:task\-definition  | 
|  UpdateServicePrimaryTaskSet  | Service arn:aws:ecs:*region*:*aws\_account\_id*:service/*cluster\-name*/*service\-name* |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateTaskSet  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:task\-set/*cluster\-name*/*service\-name*/*taskset\-id*  |  ecs:cluster ecs:service  | 