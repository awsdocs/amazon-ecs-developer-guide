# Windows IAM roles for tasks<a name="windows_task_IAM_roles"></a>

The IAM roles for tasks with Windows features requires extra configuration, but much of this configuration is similar to enabling IAM roles for tasks on Linux container instances\. The following requirements must be met to enable IAM roles for tasks for Windows containers\.
+ When you launch your container instances, you must enable the feature by setting the `-EnableTaskIAMRole` option in the container instances user data script\. For example:

  ```
  <powershell>
  Import-Module ECSTools
  Initialize-ECSAgent -Cluster 'windows' -EnableTaskIAMRole
  </powershell>
  ```
+ You must bootstrap your container with the networking commands that are provided in [IAM roles for task container bootstrap script](#windows_task_IAM_roles_bootstrap)\.
+ You must create an IAM role and policy for your tasks\. For more information, see [Creating an IAM Role and Policy for your Tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.
+ Your container must use an AWS SDK that supports IAM roles for tasks\. For more information, see [Using a Supported AWS SDK](task-iam-roles.md#task-iam-roles-minimum-sdk)\.
+ You must specify the IAM role you created for your tasks when you register the task definition, or as an override when you run the task\. For more information, see [Specifying an IAM Role for your Tasks](task-iam-roles.md#specify-task-iam-roles)\.
+ The IAM roles for the task credential provider use port 80 on the container instance, so if you enable IAM roles for tasks on your container instance, your containers cannot use port 80 for the host port in any port mappings\. To expose your containers on port 80, we recommend configuring a service for them that uses load balancing\. You can use port 80 on the load balancer, and the traffic can be routed to another host port on your container instances\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ If your Windows instance is restarted, you must delete the proxy interface and initialize the Amazon ECS container agent again to bring the credential proxy back up\.

## IAM roles for task container bootstrap script<a name="windows_task_IAM_roles_bootstrap"></a>

Before containers can access the credential proxy on the container instance to get credentials, the container must be bootstrapped with the required networking commands\. The following code example script should be run on your containers when they start\.

```
# Copyright 2014-2016 Amazon.com, Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License"). You may
# not use this file except in compliance with the License. A copy of the
# License is located at
#
#	http://aws.amazon.com/apache2.0/
#
# or in the "license" file accompanying this file. This file is distributed
# on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either
# express or implied. See the License for the specific language governing
# permissions and limitations under the License.

$gateway = (Get-NetRoute | Where { $_.DestinationPrefix -eq '0.0.0.0/0' } | Sort-Object RouteMetric | Select NextHop).NextHop
$ifIndex = (Get-NetAdapter -InterfaceDescription "Hyper-V Virtual Ethernet*" | Sort-Object | Select ifIndex).ifIndex
New-NetRoute -DestinationPrefix 169.254.170.2/32 -InterfaceIndex $ifIndex -NextHop $gateway # credentials API
New-NetRoute -DestinationPrefix 169.254.169.254/32 -InterfaceIndex $ifIndex -NextHop $gateway # metadata API
```