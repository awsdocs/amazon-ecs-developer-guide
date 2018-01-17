# Supported Resource\-Level Permissions for Amazon ECS API Actions<a name="ecs-supported-iam-actions-resources"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Amazon ECS has partial support for resource\-level permissions\. This means that for certain Amazon ECS actions, you can control when users are allowed to use those actions based on conditions that have to be fulfilled, or specific resources that users are allowed to use\. For example, you can grant users permission to launch instances, but only of a specific type, and only using a specific AMI\.

The following table describes the Amazon ECS API actions that currently support resource\-level permissions, as well as the supported resources, resource ARNs, and condition keys for each action\.

**Important**  
If an Amazon ECS API action is not listed in this table, then it does not support resource\-level permissions\. If an Amazon ECS API action does not support resource\-level permissions, you can grant users permission to use the action, but you have to specify a \* for the resource element of your policy statement\.


| API action | Resource | Condition keys | 
| --- | --- | --- | 
| DeleteAttributes |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| DeleteCluster |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| DeregisterContainerInstance |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| DescribeClusters |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster1*, arn:aws:ecs:*region*:*account*:cluster/*my\-cluster2*  |  N/A  | 
| DescribeContainerInstances |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id1*, arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id2*  |  ecs:cluster  | 
| DescribeTasks |  Task arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a*, arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252b*  |  ecs:cluster  | 
| ListAttributes |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| ListContainerInstances |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| ListTasks |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| Poll |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| PutAttributes |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| RegisterContainerInstance |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| RunTask |  Task definition arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  ecs:cluster  | 
| StartTask |  Task definition arn:aws:ecs:*region*:*account*:task\-definition/*hello\_world:8*  |  ecs:cluster ecs:container\-instances  | 
| StartTelemetrySession |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| StopTask |  Task arn:aws:ecs:*region*:*account*:task/*1abf0f6d\-a411\-4033\-b8eb\-a4eed3ad252a *  |  ecs:cluster  | 
| SubmitContainerStateChange |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| SubmitTaskStateChange |  Cluster arn:aws:ecs:*region*:*account*:cluster/*my\-cluster*  |  N/A  | 
| UpdateContainerAgent |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 
| UpdateContainerInstancesState |  Container instance arn:aws:ecs:*region*:*account*:container\-instance/*container\-instance\-id*  |  ecs:cluster  | 