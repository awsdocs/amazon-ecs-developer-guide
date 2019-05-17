# Amazon Resource Names \(ARNs\) and IDs<a name="ecs-resource-ids"></a>

When Amazon ECS resources are created, we assign each resource a unique Amazon Resource Name \(ARN\) and resource identifier \(ID\)\. If you are using a command line tool or the Amazon ECS API to work with Amazon ECS, resource ARNs or IDs are required for certain commands\. For example, if you are using the [stop\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/stop-task.html) AWS CLI command to stop a task, you must specify the task ARN or ID in the command\.

We're gradually introducing a new ARN and resource ID format for Amazon ECS tasks, services and container instances\. The following sections describe how the formats are changing\. For more information on the transition to the new formats, see [Amazon Elastic Container Service FAQ](http://aws.amazon.com/ecs/faqs/)\.

**Amazon Resource Name \(ARN\) Format**  
Some resources have a friendly name \(for example, a service named `production`\)\. However, sometimes you are required to specify a resource using the Amazon Resource Name \(ARN\) format\. We're gradually introducing a new ARN format for Amazon ECS tasks, services and container instances which includes the cluster name\. For details on how to opt in to the new ARN format, see [Working with Account Settings](#ecs-resource-ids-working-with)\.

**Note**  
The new ARN format is not available in the GovCloud \(US\-East\) region\.

The following table shows both the current \(old\) format and the new format for each resource type:


|  Resource type  |  ARN  | 
| --- | --- | 
|  Container instance  |  Old: `arn:aws:ecs:region:aws_account_id:container-instance/container-instance-id` New: `arn:aws:ecs:region:aws_account_id:container-instance/cluster-name/container-instance-id`  | 
|  Amazon ECS service  |  Old: `arn:aws:ecs:region:aws_account_id:service/service-name` New: `arn:aws:ecs:region:aws_account_id:service/cluster-name/service-name`  | 
|  Amazon ECS task  |  Old: `arn:aws:ecs:region:aws_account_id:task/task-id` New: `arn:aws:ecs:region:aws_account_id:task/cluster-name/task-id`  | 

**Resource ID Length**  
A resource ID takes the form of a unique combination of letters and numbers\. We're gradually introducing shorter length IDs for Amazon ECS tasks and container instances\. The length of the ID was in an 36\-character format; the new IDs are in a 32\-character format that will not include any hyphens\. For details on how to opt in to the new resource ID format, see [Working with Account Settings](#ecs-resource-ids-working-with)\.

**Note**  
The new resource ID format is not available in the GovCloud \(US\-East\) region\.

**Timeline**  
The new formats have an opt\-in period, during which you can choose to accept the new formats\. The following lists the important dates related to this change:
+ Now until March 31, 2019 \- The option to opt\-in to the new Amazon Resource Name \(ARN\) and resource ID formats begins\. The ability to opt\-in and opt\-out is provided on a per\-region basis\. Any new accounts created will be opted\-out by default\.
+ April 1, 2019 \- December 31, 2019 \- All new accounts will be opted\-in by default\. The ability to opt\-in and opt\-out will continue to be available on a per\-region basis\.
+ January 1, 2020 \- All accounts will be opted\-in by default\. All new resources created will receive the new format\.

You can opt\-in or opt\-out of the new Amazon Resource Name \(ARN\) and resource ID format at any time during the opt\-in period\. After you have opted in, any new resources that you create are created with the new format\.

**Note**  
A resource ID does not change after it's created\. Therefore, opting in or out of the new format during the opt\-in period does not affect your existing resource IDs\.

## Working with Account Settings<a name="ecs-resource-ids-working-with"></a>

For each Region, you can opt in or opt out of the new ARN and resource ID format at the account level or for a specific IAM user or role\. The following are supported scenarios:
+ An IAM user or role can opt in or opt out for their invididual user account\.
+ An IAM user or role can set the default opt in or opt out setting for all users on the account\.
+ The root user has the ability to opt in or opt out any specific IAM role or user on the account\. If the account setting for the root user is changed, it sets the default setting for all the IAM users and roles for which no individual account setting has been set\.

The opt in and opt out account setting must be set for each Amazon ECS resource separately\. The ARN and resource ID format of a resource is defined by the opt\-in status of the IAM user or role that created the resource\.

Only resources launched after opting in will receive the new ARN and resource ID format\. All existing resources keep their ARN and resource ID and aren't affected\. For Amazon ECS services and tasks to transition to the new format, the service or task must be recreated\. To transition a container instance to the new format, the container instance must be drained and a new container instance registered to the cluster\.

**Note**  
Tasks launched by an Amazon ECS service can only receive the new ARN and resource ID format if the service was created on or after November 16, 2018, and the IAM user who created the service has opted in for the new format for tasks\.

**Topics**
+ [Viewing Account Settings](#ecs-viewing-longer-id-settings)
+ [Modifying Account Settings](#ecs-modifying_longer_id_settings)

### Viewing Account Settings<a name="ecs-viewing-longer-id-settings"></a>

You can use the AWS Management Console and AWS CLI tools to view the resource types that support the new ARN and ID formats\.

**To view your account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to view your account settings\. 

1. From the dashboard, under **Clusters**, choose **Configure ECS ARN setting**\.

1. On the **Amazon ECS ARN and resource ID settings** section, you can view your account settings for each resource type for the authenticated IAM user and role\.

**To view your account settings using the command line**

Use one of the following commands to view your account settings\.
+ [list\-account\-settings](https://docs.aws.amazon.com/cli/latest/reference/ecs/list-account-settings.html) \(AWS CLI\)

  ```
  aws ecs list-account-settings --effective-settings --region us-east-1
  ```
+ [Get\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Get-ECSAccountSetting -EffectiveSetting true -Region us-east-1
  ```

**To view the account settings for a specific IAM user or IAM role using the command line**

Use one of the following commands and specify the ARN of an IAM user, IAM role, or root account user in the request to view their account settings\.
+ [list\-account\-settings](https://docs.aws.amazon.com/cli/latest/reference/ecs/list-account-settings.html) \(AWS CLI\)

  ```
  aws ecs list-account-settings --principal-arn arn:aws:iam::aws_account_id:user/principalName --effective-settings --region us-east-1
  ```
+ [Get\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Get-ECSAccountSetting -PrincipalArn arn:aws:iam::aws_account_id:user/principalName -EffectiveSetting true -Region us-east-1
  ```

### Modifying Account Settings<a name="ecs-modifying_longer_id_settings"></a>

You can use the AWS Management Console and AWS CLI tools to modify account settings for resource types that are still within their opt\-in period\.

**To modify account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the region for which to modify the longer ID settings\.

1. From the dashboard, choose **Configure ECS ARN setting**\.

1. To enable the new ARN and ID format for each supported resource type, select the option in the **My IAM user/role opt\-in setting** column\. This will change the opt in setting for the authenticated IAM user\.

1. On the confirmation screen, choose **Confirm** to save the selection\.

**To modify the default account settings for all IAM users or roles on your account using the command line**

Use one of the following commands to modify the default account setting for all IAM users or roles on your account\. These changes apply to the entire AWS account unless an IAM user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting-default --name serviceLongArnFormat --value enabled --region us-east-2
  ```

  You can also use the command to modify the account settings for all tasks \(`taskLongArnFormat`\) and container instances \(`containerInstanceLongArnFormat`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSettingDefault -Name serviceLongArnFormat -Value enabled -Force
  ```

**To modify the account settings for your IAM user account using the command line**

Use one of the following commands to modify the account settings for your IAM user\. If you’re using these commands as the root user, changes apply to the entire AWS account unless an IAM user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting --name serviceLongArnFormat --value enabled --region us-east-1
  ```

  You can also use the command to modify the account settings for all tasks \(`taskLongArnFormat`\) and container instances \(`containerInstanceLongArnFormat`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -Force
  ```

**To modify the account settings for a specific IAM user or IAM role using the command line**

Use one of the following commands and specify the ARN of an IAM user, IAM role, or root user in the request to modify the account settings for a specific IAM user or IAM role\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting --name serviceLongArnFormat --value enabled --principal-arn arn:aws:iam::aws_account_id:user/principalName --region us-east-1
  ```

  You can also use the command to modify the account settings for all tasks \(`taskLongArnFormat`\) and container instances \(`containerInstanceLongArnFormat`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/principalName -Force
  ```