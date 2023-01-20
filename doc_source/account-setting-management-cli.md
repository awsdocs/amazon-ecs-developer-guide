# Account setting management using the AWS CLI<a name="account-setting-management-cli"></a>

You can manage your account settings using the Amazon ECS API, AWS CLI or SDKs\. 

For information about the available API actions for task definitions see [Account setting actions actions](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/OperationList-query-account.html) in the *Amazon Elastic Container Service API Reference*\.

Use one of the following commands to modify the default account setting for all users or roles on your account\. These changes apply to the entire AWS account unless a user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting-default --name serviceLongArnFormat --value enabled --region us-east-2
  ```

  You can also use this command to modify other account settings\. To do this, replace the `name` parameter with the corresponding account setting\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSettingDefault -Name serviceLongArnFormat -Value enabled -Region us-east-1 -Force
  ```

**To modify the account settings for your user account \(AWS CLI\)**

Use one of the following commands to modify the account settings for your user\. If you’re using these commands as the root user, changes apply to the entire AWS account unless a; user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting --name serviceLongArnFormat --value enabled --region us-east-1
  ```

  You can also use this command to modify other account settings\. To do this, replace the `name` parameter with the corresponding account setting\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -Force
  ```

**To modify the account settings for a specific user or role \(AWS CLI\)**

Use one of the following commands and specify the ARN of a user, role, or root user in the request to modify the account settings for a specific user or role\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting --name serviceLongArnFormat --value enabled --principal-arn arn:aws:iam::aws_account_id:user/principalName --region us-east-1
  ```

  You can also use this command to modify other account settings\. To do this, replace the `name` parameter with the corresponding account setting\.
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSetting -Name serviceLongArnFormat -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/principalName -Region us-east-1 -Force
  ```