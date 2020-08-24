# Viewing account settings<a name="ecs-viewing-longer-id-settings"></a>

You can use the AWS Management Console and AWS CLI tools to view the resource types that support the new ARN and ID formats or the increased ENI limits\.

**Important**  
Amazon ECS ARN and resource ID settings that have not been explicitly set will be affected by the format timeline\. The console displays an **Undefined** value and the AWS CLI returns an empty set when a value has not been explicitly set\. For more information, see [ARN and resource ID format timeline](ecs-account-settings.md#ecs-resource-arn-timeline)\.

**To view your account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to view your account settings\. 

1. From the dashboard, choose **Account Settings**\.

1. On the **Amazon ECS ARN and resource ID settings**, **AWSVPC Trunking**, and **CloudWatch Container Insights** sections, you can view your opt\-in status for each account setting for the authenticated IAM user and role\.

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