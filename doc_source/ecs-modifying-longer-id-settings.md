# Modifying account settings<a name="ecs-modifying-longer-id-settings"></a>

You can use the AWS Management Console and AWS CLI tools to modify the account settings\.

**To modify account settings using the console**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to modify your account settings\.

1. From the dashboard, choose **Account Settings**\.

1. On the **Amazon ECS ARN and resource ID settings**, **AWSVPC Trunking**, and **CloudWatch Container Insights** sections, you can select or deselect the check boxes for each account setting for the authenticated IAM user and role\. Choose **Save** once finished\.
**Important**  
IAM users and IAM roles need the `ecs:PutAccountSetting` permission to perform this action\.

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
  Write-ECSAccountSettingDefault -Name serviceLongArnFormat -Value enabled -Region us-east-1 -Force
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
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/principalName -Region us-east-1 -Force
  ```