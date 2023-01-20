# Updating a capacity provider using the console<a name="update-capacity-provider-console-v2"></a>

When you use an Auto Scaling group as a capacity provider, you can modify the group's scaling policy\.

**To update a capacity provider for the cluster \(New Amazon ECS console\)**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : *name*** page, choose **Infrastructure**, and then choose **Update**\.

1. On the **Create capacity providers** page, configure the following options\.

   1. Under **Auto Scaling group**, under **Scaling policies**, configure the following options\.
     + To have Amazon ECS manage the scale\-in and scale\-out actions, select **Turn on managed scaling**\.
     + To prevent EC2 instance with running Amazon ECS tasks from being terminated, select **Turn on scaling protection**\.
     + For **Set target capacity**, enter the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\.

1. Choose **Update**\.