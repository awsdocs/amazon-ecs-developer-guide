# Amazon ECS CloudWatch Container Insights<a name="cloudwatch-container-insights"></a>

CloudWatch Container Insights collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. The metrics include utilization for resources such as CPU, memory, disk, and network\. The metrics are available in CloudWatch automatic dashboards\. For a full list of Amazon ECS Container Insights metrics, see [Amazon ECS Container Insights Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-metrics-ECS.html) in the *Amazon CloudWatch User Guide*\.

Operational data is collected as performance log events\. These are entries that use a structured JSON schema that enables high\-cardinality data to be ingested and stored at scale\. From this data, CloudWatch creates higher\-level aggregated metrics at the cluster and service level as CloudWatch metrics\. For more information, see [Container Insights Structured Logs for Amazon ECS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Container-Insights-reference-structured-logs-ECS.html) in the *Amazon CloudWatch User Guide*\.

**Important**  
Metrics collected by CloudWatch Container Insights are charged as custom metrics\. For more information about CloudWatch pricing, see [CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. Amazon ECS also provides monitoring metrics that are provided at no additional cost\. For more information, see [Amazon ECS CloudWatch metrics](cloudwatch-metrics.md)\.

## Container Insights considerations<a name="cloudwatch-container-insights-considerations"></a>

The following should be considered when using CloudWatch Container Insights\.
+ CloudWatch Container Insights metrics only reflect the resources with running tasks during the specified time range\. For example, if you have a cluster with one service in it but that service has no tasks in a `RUNNING` state, there will be no metrics sent to CloudWatch\. If you have two services and one of them has running tasks and the other doesn't, only the metrics for the service with running tasks will be sent\.
+ Network metrics are available for tasks using the Fargate or EC2 launch type with the `bridge` or awsvpc network mode\.

## Working with Container Insights\-enabled clusters<a name="cloudwatch-container-insights-working"></a>

Container Insights can be enabled for all new clusters created by opting in to the `containerInsights` account setting, on individual clusters by enabling it using the cluster settings during cluster creation, or on existing clusters by using the UpdateClusterSettings API\. 

Opting in to the `containerInsights` account setting can be done with both the Amazon ECS console and the AWS CLI\. You must be running version `1.16.200` or later of the AWS CLI to use this feature\. For more information on creating Amazon ECS clusters, see [Creating a cluster](create_cluster.md)\.

**Important**  
For clusters containing tasks or services using the EC2 launch type, your container instances must be running version 1\.29\.0 or later of the Amazon ECS agent\. For more information, see [Amazon ECS Container Agent Versions](ecs-agent-versions.md)\.

**To opt in all IAM users or roles on your account to Container Insights\-enabled clusters using the console**

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

**To update the settings for an existing cluster using the command line**

Use one of the following commands to update the setting for a cluster\.
+ [update\-cluster\-settings](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-cluster-settings.html) \(AWS CLI\)

  ```
  aws ecs update-cluster-settings --cluster cluster_name_or_arn --settings name=containerInsights,value=enabled|disabled --region us-east-1
  ```