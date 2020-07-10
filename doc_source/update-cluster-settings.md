# Updating cluster settings<a name="update-cluster-settings"></a>

Cluster settings enable you to configure parameters for your existing Amazon ECS clusters\. You can update cluster settings using the Amazon ECS API, AWS CLI or SDKs\. Currently, the only supported cluster setting is `containerInsights`, which allows you to enable or disable CloudWatch Container Insights for an existing cluster\. To enable CloudWatch Container Insights for a new cluster, that can be done in the AWS Management Console during cluster creation\. For more information, see [Creating a cluster](create_cluster.md)\.

**Important**  
Currently, if you delete an existing cluster that does not have Container Insights enabled and then create a new cluster with the same name with Container Insights enabled, Container Insights will not actually be enabled\. If you want to preserve the same name for your existing cluster and enable Container Insights, you must wait 7 days before you can re\-create it\.

**To update the settings for a cluster using the command line**

Use one of the following commands to update the setting for a cluster\.
+ [update\-cluster\-settings](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-cluster-settings.html) \(AWS CLI\)

  ```
  aws ecs update-cluster-settings --cluster cluster_name_or_arn --settings name=containerInsights,value=enabled|disabled --region us-east-1
  ```