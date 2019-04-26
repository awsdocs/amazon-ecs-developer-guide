# Supported Resource\-Level Permissions for Amazon ECS API Actions<a name="ecs-supported-iam-actions-resources"></a>

The term *Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

The following table describes the Amazon ECS API actions that currently support resource\-level permissions, as well as the supported resources, resource ARNs, and condition keys for each action\.

**Important**  
If an Amazon ECS API action is not listed in this table, then it does not support resource\-level permissions\. If an Amazon ECS API action does not support resource\-level permissions, you can grant users permission to use the action, but you have to specify a \* for the resource element of your policy statement\.


| Actions | Resource Types | Condition Keys | 
| --- | --- | --- | 
|  CreateCluster  | N/A |  aws:RequestTag  | 
|  CreateService  |  N/A  |  N/A  | 
|  DeleteAccountSettings  |  N/A  |  N/A  | 
|  DeleteAttributes  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  DeleteCluster  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  DeleteService  |  N/A  |  N/A  | 
|  DeregisterContainerInstance  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  DeregisterTaskDefinition  |  N/A  |  N/A  | 
|  DescribeClusters  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster1*, arn:aws:ecs:*region*:*account*:cluster/*my\-cluster2*  |  N/A  | 
|  DescribeContainerInstances  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id1*, arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id2*  |  ecs:cluster  | 
|  DescribeServices  |  N/A  |  N/A  | 
|  DescribeTaskDefinition  |  N/A  |  N/A  | 
|  DescribeTasks  |  Task \(required\) arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a*, arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252b*  |  ecs:cluster  | 
|  DiscoverPollEndpoint  |  N/A  |  N/A  | 
|  ListAccountSettings  |  N/A  |  N/A  | 
|  ListAttributes  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  ListClusters  |  N/A  |  N/A  | 
|  ListContainerInstances  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  ListServices  |  N/A  |  N/A  | 
|  ListTagsForResource  |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id* Task arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  N/A  | 
|  ListTaskDefinitionFamilies  |  N/A  |  N/A  | 
|  ListTaskDefinitions  |  N/A  |  N/A  | 
|  ListTasks  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  Poll  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  PutAccountSetting  |  N/A  |  N/A  | 
|  PutAccountSettingDefault  |  N/A  |  N/A  | 
|  PutAttributes  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  RegisterContainerInstance  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  aws:RequestTag  | 
|  RegisterTaskDefinition  |  N/A  |  aws:RequestTag  | 
|  RunTask  |  Task definition \(required\) arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  ecs:cluster aws:RequestTag  | 
|  StartTask  |  Task definition \(required\) arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  ecs:cluster ecs:container\-instances aws:RequestTag  | 
|  StartTelemetrySession  |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  StopTask  |  Task \(required\) arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a *  |  ecs:cluster  | 
|  SubmitContainerStateChange  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  SubmitTaskStateChange  |  Cluster \(required\) arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
|  TagResource  |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id* Task arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  aws:RequestTag  | 
|  UntagResource  |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster* Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id* Task arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a* Task definition arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  N/A  | 
|  UpdateContainerAgent  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  UpdateContainerInstancesState  |  Container instance \(required\) arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
|  UpdateService  |  N/A  |  N/A  | 