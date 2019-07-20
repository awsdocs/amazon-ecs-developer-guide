# Amazon ECS CloudWatch Container Insights<a name="cloudwatch-container-insights"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. The metrics include utilization for resources such as CPU, memory, disk, and network\. The metrics are available in CloudWatch automatic dashboards\. For a full list of Amazon ECS Container Insights metrics, see [Amazon ECS Container Insights Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-metrics-ECS.html) in the *Amazon CloudWatch User Guide*\.

Operational data is collected as performance log events\. These are entries that use a structured JSON schema that enables high\-cardinality data to be ingested and stored at scale\. From this data, CloudWatch creates higher\-level aggregated metrics at the cluster and service level as CloudWatch metrics\. For more information, see [Container Insights Structured Logs for Amazon ECS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-reference-structured-logs-ECS.html) in the *Amazon CloudWatch User Guide*\.

**Important**  
CloudWatch Container Insights are provided at an additional cost\. For information about the default monitoring metrics that are provided at no additional cost, see [Amazon ECS CloudWatch Metrics](cloudwatch-metrics.md)\.

## Working With Container Insights\-Enabled Clusters<a name="cloudwatch-container-insights-working"></a>

Container Insights can be enabled for all clusters by opting in to the `containerInsights` account setting, or on individual clusters by enabling it using the cluster settings during cluster creation\. Opting in to the Container Insights account setting can be done with both the Amazon ECS console and the AWS CLI\. For more information on creating Amazon ECS clusters, see [Creating a Cluster](create_cluster.md)\.

For clusters containing tasks or services using the EC2 launch type, your container instances must be running version 1\.29\.0 or later of the Amazon ECS agent\. For more information, see [Amazon ECS Container Agent Versions](container_agent_versions.md)\.

**To opt in in all IAM users or roles on your account to Container Insights\-enabled clusters using the console**

1. As the root user of the account, open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation bar at the top of the screen, select the Region for which to opt in to Container Insights\-enabled clusters\.

1. From the dashboard, choose **Account Settings**\.

1. For **IAM user or role**, ensure your root user or container instance IAM role is selected\.

1. For **Container Insights**, select the check box\. Choose **Save** once finished\.
**Important**  
IAM users and IAM roles need the `ecs:PutAccountSetting` permission to perform this action\.

1. On the confirmation screen, choose **Confirm** to save the selection\.

**To opt in all IAM users or roles on your account to Container Insights\-enabled clusters using the command line**

Any user on an account can use one of the following commands to modify the default account setting for all IAM users or roles on your account\. These changes apply to the entire AWS account unless an IAM user or role explicitly overrides these settings for themselves\.
+ [put\-account\-setting\-default](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting-default.html) \(AWS CLI\)

  ```
  aws ecs put-account-setting-default --name containerInsights --value enabled --region us-east-1
  ```
+ [Write\-ECSAccountSettingDefault](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSettingDefault.html) \(AWS Tools for Windows PowerShell\)

  ```
  Write-ECSAccountSettingDefault -Name containerInsights -Value enabled -Region us-east-1 -Force
  ```

**To opt in an IAM user or container instance IAM role to Container Insights\-enabled clusters as the root user using the command line**

The root user on an account can use one of the following commands and specify the ARN of the principal IAM user or container instance IAM role in the request to modify the account settings\.
+ [put\-account\-setting](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-account-setting.html) \(AWS CLI\)

  The following example is for modifying the account setting of a specific IAM user:

  ```
  aws ecs put-account-setting --name containerInsights --value enabled --principal-arn arn:aws:iam::aws_account_id:user/userName --region us-east-1
  ```
+ [Write\-ECSAccountSetting](https://docs.aws.amazon.com/powershell/latest/reference/items/Write-ECSAccountSetting.html) \(AWS Tools for Windows PowerShell\)

  The following example is for modifying the account setting of a specific IAM user:

  ```
  Write-ECSAccountSetting -Name containerInsights -Value enabled -PrincipalArn arn:aws:iam::aws_account_id:user/userName -Region us-east-1 -Force
  ```