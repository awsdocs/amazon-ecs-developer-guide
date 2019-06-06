# Account Settings<a name="ecs-account-settings"></a>

Account settings allow you to opt in to or opt out of specific Amazon ECS settings\. 

Amazon ECS is introducing a new format for Amazon Resource Names \(ARNs\) and resource IDs for Amazon ECS tasks, container instances, and services\. You must opt in to the new format to use features such as resource tagging\. 

Amazon ECS supports launching container instances using Amazon EC2 instance families built on the Nitro system that have increased elastic network interface \(ENI\) limits\. To enable this feature, you must opt in to the `awsvpcTrunking` account setting\.

For each Region, you can opt in to or opt out of each account setting at the account level or for a specific IAM user or role\. The available account settings to opt in to or out of include the new ARN and resource ID format and the awsvpc trunking feature\.

The following are supported scenarios:
+ An IAM user or role can opt in or opt out for their individual user account\.
+ An IAM user or role can set the default opt in or opt out setting for all users on the account\.
+ The root user has the ability to opt in to or opt out of any specific IAM role or user on the account\. If the account setting for the root user is changed, it sets the default for all the IAM users and roles for which no individual account setting has been selected\.

The opt in and opt out option must be selected for each account setting separately\. The ARN and resource ID format of a resource is defined by the opt\-in status of the IAM user or role that created the resource\.

Only resources launched after opting in receive the new ARN and resource ID format or the increased ENI limits\. All existing resources are not affected\. In order for Amazon ECS services and tasks to transition to the new ARN and resource ID formats, the service or task must be re\-created\. To transition a container instance to the new ARN and resource ID format or the increased ENI limits, the container instance must be drained and a new container instance registered to the cluster\.

**Note**  
Tasks launched by an Amazon ECS service can only receive the new ARN and resource ID format if the service was created on or after November 16, 2018, and the IAM user who created the service has opted in to the new format for tasks\.

## Amazon Resource Names \(ARNs\) and IDs<a name="ecs-resource-ids"></a>

When Amazon ECS resources are created, each resource is assigned a unique Amazon Resource Name \(ARN\) and resource identifier \(ID\)\. If you are using a command line tool or the Amazon ECS API to work with Amazon ECS, resource ARNs or IDs are required for certain commands\. For example, if you are using the [stop\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/stop-task.html) AWS CLI command to stop a task, you must specify the task ARN or ID in the command\.

 The following sections describe how ARN and resource ID formats are changing\. For more information on the transition to the new formats, see [Amazon Elastic Container Service FAQ](https://aws.amazon.com/ecs/faqs/)\.

**Amazon Resource Name \(ARN\) Format**  
Some resources have a friendly name, such as a service named `production`\. In other cases, you must specify a resource using the Amazon Resource Name \(ARN\) format\. The new ARN format for Amazon ECS tasks, services, and container instances includes the cluster name\. For details about opting in to the new ARN format, see [Modifying Account Settings](#ecs-modifying-longer-id-settings)\.

**Note**  
The new ARN format is not available in the GovCloud \(US\-East\) region\.

The following table shows both the current \(old\) format and the new format for each resource type\.


|  Resource Type  |  ARN  | 
| --- | --- | 
|  Container instance  |  Old: `arn:aws:ecs:region:aws_account_id:container-instance/container-instance-id` New: `arn:aws:ecs:region:aws_account_id:container-instance/cluster-name/container-instance-id`  | 
|  Amazon ECS service  |  Old: `arn:aws:ecs:region:aws_account_id:service/service-name` New: `arn:aws:ecs:region:aws_account_id:service/cluster-name/service-name`  | 
|  Amazon ECS task  |  Old: `arn:aws:ecs:region:aws_account_id:task/task-id` New: `arn:aws:ecs:region:aws_account_id:task/cluster-name/task-id`  | 

**Resource ID Length**  
A resource ID takes the form of a unique combination of letters and numbers\. New resource ID formats include shorter IDs for Amazon ECS tasks and container instances\. The old resource ID format was 36 characters long\. The new IDs are in a 32\-character format that does not include any hyphens\. For details about opting in to the new resource ID format, see [Modifying Account Settings](#ecs-modifying-longer-id-settings)\.

**Note**  
The new resource ID format is not available in the GovCloud \(US\-East\) region\.

**Timeline**  
There is an opt\-in period for new formats\. Following are the important dates related to this change\.
+ From the initial format release until March 31, 2019 – The ability to opt in to and opt out of the new Amazon Resource Name \(ARN\) and resource IDs is provided on a per\-Region basis\. Any new accounts created are opted out by default\.
+ April 1, 2019 \- December 31, 2019 – All new accounts are be opted in by default\. The ability to opt in and opt out continues to be available on a per\-Region basis\.
+ January 1, 2020 – All accounts will be opted in by default\. All new resources created will receive the new format\.

You can opt in or opt out of the new Amazon Resource Name \(ARN\) and resource ID format at any time during the opt\-in period\. After you have opted in, any new resources that you create use the new format\.

**Note**  
A resource ID does not change after it's created\. Therefore, opting in or out of the new format during the opt\-in period does not affect your existing resource IDs\.

## AWSVPC Trunking<a name="ecs-eni-trunking"></a>

Amazon ECS supports launching container instances with increased ENI density using supported Amazon EC2 instance types\. When you use these instance types and opt in to the `awsvpcTrunking` account setting, additional ENIs are available on newly launched container instances\. This configuration allows you to place more tasks using the `awsvpc` network mode on each container instance\. Using this feature, a `c5.large` instance with `awsvpcTrunking` enabled has an increased ENI limit of ten\. The container instance will have a primary network interface and Amazon ECS creates and attaches a "trunk" network interface to the container instance\. Neither the primary network interface or the trunk network interface counts against the ENI limit, so this configuration allows you to launch ten tasks on the container instance instead of the current two tasks\. For more information, see [Elastic Network Interface Trunking](container-instance-eni.md)\.

## Viewing Account Settings<a name="ecs-viewing-longer-id-settings"></a>

You can use the AWS Management Console and AWS CLI tools to view the resource types that support the new ARN and ID formats\.

**To view your account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to view your account settings\. 

1. From the dashboard, choose **Account Settings**\.

1. On the **Amazon ECS ARN and resource ID settings** and **AWSVPC Trunking** sections, you can view your opt\-in status for each account setting for the authenticated IAM user and role\.

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

## Modifying Account Settings<a name="ecs-modifying-longer-id-settings"></a>

You can use the AWS Management Console and AWS CLI tools to modify the account settings\.

**To modify account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to modify your account settings\.

1. From the dashboard, choose **Account Settings**\.

1. On the **Amazon ECS ARN and resource ID settings** and **AWSVPC Trunking** sections, you can select or deselect the check boxes for each account setting for the authenticated IAM user and role\. Choose **Save** once finished\.

1. On the confirmation screen, choose **Confirm** to save the selection\.

**To modify the default account settings for all IAM users or roles on your account using the command line**

Use one of the following commands to modify the default account setting for all IAM users or roles on your account\. These changes apply to the entire AWS account unless an IAM user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting-default --name serviceLongArnFormat --value enabled --region us-east-2
  ```

  You can also use this command to modify the account settings for all tasks \(`taskLongArnFormat`\), container instances \(`containerInstanceLongArnFormat`\), and to opt in to the increased elastic network interface \(ENI\) limits for container instances \(`awsvpcTrunking`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
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

  You can also use this command to modify the account settings for all tasks \(`taskLongArnFormat`\), container instances \(`containerInstanceLongArnFormat`\), and to opt in to the increased elastic network interface \(ENI\) limits for container instances \(`awsvpcTrunking`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
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

  You can also use this command to modify the account settings for all tasks \(`taskLongArnFormat`\), container instances \(`containerInstanceLongArnFormat`\), and to opt in to the increased elastic network interface \(ENI\) limits for container instances \(`awsvpcTrunking`\)\. To do this, replace the `name` parameter with the corresponding resource type\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/principalName -Force
  ```
