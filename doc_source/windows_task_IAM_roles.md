# Additional configuration for Windows IAM roles for tasks<a name="windows_task_IAM_roles"></a>

**Important**  
For Windows containers on Fargate that use task roles, no further action is necessary\. For Windows containers on EC2 that use task roles, follow these steps\.

The IAM roles for tasks with Windows features requires additional configuration on EC2, but much of this configuration is similar to configuring IAM roles for tasks on Linux container instances\. The following requirements must be met to configure IAM roles for tasks for Windows containers\.
+ When you launch your container instances, you must set the `-EnableTaskIAMRole` option in the container instances user data script\. The `EnableTaskIAMRole` turns on the Task IAM roles feature for the tasks\. For example:

  ```
  <powershell>
  Import-Module ECSTools
  Initialize-ECSAgent -Cluster 'windows' -EnableTaskIAMRole 
  </powershell>
  ```
+ You must bootstrap your container with the networking commands that are provided in [IAM roles for task container bootstrap script](#windows_task_IAM_roles_bootstrap)\.
+ You must create an IAM role and policy for your tasks\. For more information, see [Creating an IAM role and policy for your tasks](task-iam-roles.md#create_task_iam_policy_and_role)\.
+ You must specify the IAM role you created for your tasks when you register the task definition, or as an override when you run the task\. For more information, see [Specifying an IAM role for your tasks](task-iam-roles.md#specify-task-iam-roles)\.
+ The IAM roles for the task credential provider use port 80 on the container instance\. Therefore, if you configure IAM roles for tasks on your container instance, your containers can't use port 80 for the host port in any port mappings\. To expose your containers on port 80, we recommend configuring a service for them that uses load balancing\. You can use port 80 on the load balancer\. By doing so, traffic can be routed to another host port on your container instances\. For more information, see [Service load balancing](service-load-balancing.md)\.
+ If your Windows instance is restarted, you must delete the proxy interface and initialize the Amazon ECS container agent again to bring the credential proxy back up\.

## IAM roles for task container bootstrap script<a name="windows_task_IAM_roles_bootstrap"></a>

Before containers can access the credential proxy on the container instance to get credentials, the container must be bootstrapped with the required networking commands\. The following code example script should be run on your containers when they start\.

**Note**  
You do not need to run this script when you use `awsvpc` network mode on Windows\.

If you run Windows containers which include Powershell, then use the following script:

```
# Copyright Amazon.com Inc. or its affiliates. All Rights Reserved.
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
New-NetRoute -DestinationPrefix 169.254.170.2/32 -InterfaceIndex $ifIndex -NextHop $gateway -PolicyStore ActiveStore # credentials API
New-NetRoute -DestinationPrefix 169.254.169.254/32 -InterfaceIndex $ifIndex -NextHop $gateway -PolicyStore ActiveStore # metadata API
```

If you run Windows containers that only have the Command shell, then use the following script:

```
# Copyright Amazon.com Inc. or its affiliates. All Rights Reserved.
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
 
for /f "tokens=1" %i  in ('netsh interface ipv4 show interfaces ^| findstr /x /r ".*vEthernet.*"') do set interface=%i
for /f "tokens=3" %i  in ('netsh interface ipv4 show addresses %interface% ^| findstr /x /r ".*Default.Gateway.*"') do set gateway=%i
netsh interface ipv4 add route prefix=169.254.170.2/32 interface="%interface%" nexthop="%gateway%" store=active # credentials API
netsh interface ipv4 add route prefix=169.254.169.254/32 interface="%interface%" nexthop="%gateway%" store=active # metadata API
```