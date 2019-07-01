# Supported Resource\-Level Permissions for Amazon ECS API Actions<a name="ecs-supported-iam-actions-resources"></a>

The term *resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

The following table describes the Amazon ECS API actions that currently support resource\-level permissions, as well as the supported resources, resource ARNs, and condition keys for each action\.

**Important**  
If an Amazon ECS API action in this table is marked `N/A` for `Resource` and `Condition keys`, then it does not support resource\-level permissions\. You can grant users permission to use the action, but you should specify a \* for the resource element of your policy statement\.


| API action | Resource | Condition keys | 
| --- | --- | --- | 
|  CreateCluster  | N/A |  aws:RequestTag/$\{TagKey\} aws:TagKeys  | 
|  CreateService  |  Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag ecs:task\-definition ecs:cluster aws:TagKeys  | 
|  CreateTaskSet  |  N/A  |  ecs:service ecs:task\-definition ecs:cluster  | 
|  DeleteAccountSetting  |  N/A  |  N/A  | 
|  DeleteAttributes  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DeleteCluster  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DeleteService  |  Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DeleteTaskSet  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*service\-name*/*taskset\-id*  |  ecs:service ecs:cluster  | 
|  DeregisterContainerInstance  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DeregisterTaskDefinition  |  N/A  |  N/A  | 
|  DescribeClusters  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster1*, arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster2*  |  aws:ResourceTag ecs:ResourceTag  | 
|  DescribeContainerInstances  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeServices  |  Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeTaskDefinition  |  N/A  |  N/A  | 
|  DescribeTasks  |  Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  DescribeTaskSets  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*service\-name*/*taskset\-id*  |  ecs:service ecs:cluster  | 
|  DiscoverPollEndpoint  |  N/A  |  N/A  | 
|  ListAccountSettings  |  N/A  |  N/A  | 
|  ListAttributes  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
| ListClusters | N/A | N/A | 
|  ListContainerInstances  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  ListServices  |  N/A  |  ecs:cluster  | 
|  ListTagsForResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:ResourceTag ecs:ResourceTag  | 
|  ListTaskDefinitionFamilies  |  N/A  |  N/A  | 
|  ListTaskDefinitions  |  N/A  |  N/A  | 
|  ListTasks  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  Poll  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  PutAccountSetting  |  N/A  |  N/A  | 
|  PutAccountSettingDefault  |  N/A  |  N/A  | 
|  PutAttributes  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  RegisterContainerInstance  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  RegisterTaskDefinition  |  N/A  |  aws:RequestTag/$\{TagKey\} aws:TagKeys  | 
|  RunTask  |  Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys ecs:cluster  | 
|  StartTask  |  Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys ecs:cluster ecs:container\-instances  | 
|  StartTelemetrySession  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  StopTask  |  Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  SubmitContainerStateChange  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  SubmitTaskStateChange  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster*  |  aws:ResourceTag ecs:ResourceTag  | 
|  TagResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8* Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:RequestTag/$\{TagKey\} aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  UntagResource  |  Cluster arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id* Task arn:aws:ecs:*region*:*aws\_account\_id*:task/*cluster\-name*/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*aws\_account\_id*:task\-definition/*hello\_world:8* Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag aws:TagKeys  | 
|  UpdateContainerAgent  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateContainerInstancesState  |  Container instance arn:aws:ecs:*region*:*aws\_account\_id*:container\-instance/*cluster\-name*/*container\-instance\-id*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateService  |  Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster ecs:task\-definition  | 
|  UpdateServicePrimaryTaskSet  |  Service arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*cluster\-name*/*service\-name*  |  aws:ResourceTag ecs:ResourceTag ecs:cluster  | 
|  UpdateTaskSet  |  TaskSet arn:aws:ecs:*region*:*aws\_account\_id*:cluster/*service\-name*/*taskset\-id*  |  ecs:cluster ecs:service  | 