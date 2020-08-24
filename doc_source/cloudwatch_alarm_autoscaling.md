# Tutorial: Scaling container instances with CloudWatch alarms<a name="cloudwatch_alarm_autoscaling"></a>

**Note**  
In December 2019, Amazon ECS launched cluster auto scaling, as an alternative method for scaling container instances\. For more information, see [Amazon ECS cluster auto scaling](cluster-auto-scaling.md)\. 

The following procedures help you to create an Auto Scaling group for an Amazon ECS cluster\. The Auto Scaling group contains container instances that you can scale out \(and in\) using CloudWatch alarms\. 

Depending on the Amazon EC2 instance types that you use in your clusters, and quantity of container instances that you have in a cluster, your tasks have a limited amount of resources that they can use while running\. Amazon ECS monitors the resources available in the cluster to work with the schedulers to place tasks\. If your cluster runs low on any of these resources, such as memory, you are eventually unable to launch more tasks until you add more container instances, reduce the number of desired tasks in a service, or stop some of the running tasks in your cluster to free up the constrained resource\.

In this tutorial, you create a CloudWatch alarm and a step scaling policy using the `MemoryReservation` metric for your cluster\. When the memory reservation of your cluster rises above 75% \(meaning that only 25% of the memory in your cluster is available for new tasks to reserve\), the alarm triggers the Auto Scaling group to add another instance and provide more resources for your tasks and services\.

## Prerequisites<a name="as-cw-tutorial-prereqs"></a>

This tutorial assumes that you have enabled CloudWatch metrics for your clusters and services\. Metrics are not available until the clusters and services send the metrics to CloudWatch, and you cannot create CloudWatch alarms for metrics that do not exist yet\. For more information, see [Enabling CloudWatch metrics](cloudwatch-metrics.md#enable_cloudwatch)\.

## Step 1: Create a CloudWatch alarm for a metric<a name="create-cw-alarms"></a>

After you have enabled CloudWatch metrics for your clusters and services, and the metrics for your cluster are visible in the CloudWatch console, you can set alarms on the metrics\. For more information, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

For this tutorial, you create an alarm on the cluster `MemoryReservation` metric to alert when the cluster's memory reservation is above 75%\.

**To create a CloudWatch alarm on a metric**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the left navigation, choose **Alarms**, **Create Alarm**\.

1. In the **CloudWatch Metrics by Category** section, choose **ECS Metrics > ClusterName**\.

1. On the **Modify Alarm** page, choose the `MemoryReservation` metric for the default cluster and choose **Next**\.

1. In the **Alarm Threshold** section, enter a name and description for your alarm\.
   + **Name:** `memory-above-75-pct`
   + **Description:** `Cluster memory reservation above 75%`

1. Set the threshold and time period requirement to `MemoryReservation` greater than 75% for 1 period\.  
![\[CloudWatch alarm threshold\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/alarm-threshold.png)

1. \(Optional\) Configure a notification to send when the alarm is triggered\. You can also choose to delete the notification if you don't want to configure one now\.

1. Choose **Create Alarm**\. Now you can use this alarm to trigger your Auto Scaling group to add a container instance when the memory reservation is above 75%\.

1. \(Optional\) You can also create another alarm that triggers when the memory reservation is below 25%, which you can use to remove a container instance from your Auto Scaling group\.

## Step 2: Create a launch configuration for an Auto Scaling group<a name="create-as-group"></a>

Now that you have enabled CloudWatch metrics and created an alarm based on one of those metrics, you can create a launch configuration and an Auto Scaling group for your cluster\. For more information and other configuration options, see [Launch Configurations](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchConfiguration.html) in the *Amazon EC2 Auto Scaling User Guide*\.

**To create an Auto Scaling launch configuration**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the left navigation pane, choose **Auto Scaling Groups**\.

1. On the **Welcome to Auto Scaling** page, choose **Create Auto Scaling Group**\.

1. On the **Create Auto Scaling Group** page, choose **Create a new launch configuration**\.

1. On the **Choose AMI** step of the **Create Auto Scaling Group** wizard, choose **Community AMIs**\.

1. Choose the latest Amazon ECS\-optimized Amazon Linux 2 AMI for your Auto Scaling group\. For information on how to retrieve the latest Amazon ECS\-optimized Amazon Linux 2 AMI, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

1. On the **Choose Instance Type** step of the **Create Auto Scaling Group** wizard, choose an instance type for your Auto Scaling group and choose **Next: Configure details**\.

1. On the **Configure details** step of the **Create Auto Scaling Group** wizard, enter the following information\. The other fields are optional\. For more information, see [Creating Launch Configurations](https://docs.aws.amazon.com/autoscaling/latest/userguide/WorkingWithLaunchConfig.html) in the *Amazon EC2 Auto Scaling User Guide*\.
   + **Name:** Enter a name for your launch configuration\.
   + **IAM role:** Select the `ecsInstanceRole` for your container instances\. If you do not have this role configured, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.
   + **IP Address Type:** Select the IP address type option for your container instances\. To allow external traffic to be able to reach your containers, choose **Assign a public IP address to every instance\.**

1. Expand the **Advanced Details** section to specify user data for your Amazon ECS container instances\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

   Paste the following script into the **User data** field\. Reference the cluster name that you are working with\. 

   ```
   #!/bin/bash
   echo ECS_CLUSTER=my-cluster >> /etc/ecs/ecs.config
   ```

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** step of the **Create Auto Scaling Group** wizard, make any storage configuration changes needed for your instances and choose **Next: Configure Security Group**\.

1. On the **Configure Security Group** step of the **Create Auto Scaling Group** wizard, select an existing security group that meets the needs of your containers, or create a new security group, and choose **Review**\.

1. Review your launch configuration and choose **Create launch configuration**\.

1. Select a private key to use for connecting to your instances with SSH and choose **Create launch configuration**\. Move on to creating an Auto Scaling group with your new launch configuration\.

## Step 3: Create an Auto Scaling group with step scaling policies<a name="create-as-group-cluster"></a>

After the launch configuration is complete, continue with the following procedure to create an Auto Scaling group that uses your launch configuration\.

**To create an Auto Scaling group with step scaling policies**

1. On the **Configure Auto Scaling group details** step of the **Create Auto Scaling Group** wizard, enter the following information and then choose **Next: Configure scaling policies**:
   + **Group name:** Enter a name for your Auto Scaling group\.
   + **Group size:** Specify the number of container instances with which your Auto Scaling group should start\.
   + **Network:** Select a VPC into which to launch your container instances\.
   + **Subnet:** Select the subnets into which to launch your container instances\. For a highly available cluster, we recommend that you enable all of the subnets in the Region\.

1. On the **Configure scaling policies** step of the **Create Auto Scaling Group** wizard, choose **Use scaling policies to adjust the capacity of this group**\.

1. Enter the minimum and maximum number of container instances for your Auto Scaling group\. 

1. Choose **Scale the Auto Scaling group using step or simple scaling policies**\.

1. In the **Increase Group Size** section, enter the following information:
   + **Execute policy when:** Select the `memory-above-75-pct` CloudWatch alarm that you configured earlier\.
   + **Take the action:** Enter the number of capacity units \(instances\) to add to your cluster when the alarm is triggered\.

1. If you configured an alarm to trigger a group size reduction, set that alarm in the **Decrease Group Size** section and specify how many instances to remove if that alarm is triggered\. Otherwise, collapse the **Decrease Group Size** section by choosing the **X** in the upper\-right\-hand corner of the section\.
**Note**  
If you configure your Auto Scaling group to remove container instances, any tasks running on the removed container instances are stopped\. If your tasks are running as part of a service, Amazon ECS restarts those tasks on another instance if the required resources are available \(CPU, memory, ports\)\. However, tasks that were started manually are not restarted automatically\.

1. Choose **Review**, **Create Auto Scaling Group**\.

## Step 4: Verify and test your Auto Scaling group<a name="verify-as-group"></a>

Now that you've created your Auto Scaling group, you should see your instances launching in the Amazon EC2 console **Instances** page\. These instances should register into your ECS cluster as well after they launch\. 

Verify that the EC2 instances are registered with the cluster\. From the ECS console, select the cluster that you registered your instances with\. On the **Cluster** page, choose **ECS Instances**\. Verify that the **Agent Connected** value is **True** for the instances displayed\.

To test that your Auto Scaling group is configured properly, create some tasks that consume a considerable amount of memory and start launching them into your cluster\. After your cluster exceeds the 75% memory reservation from the CloudWatch alarm for the specified number of periods, you should see a new instance launch in the Amazon EC2 console\.

## Step 5: Cleaning up<a name="cleanup-as"></a>

After you no longer need a step scaling policy, you can delete it\. You also need to delete the CloudWatch alarms\. Deleting a step scaling policy deletes the underlying alarm action, but does not delete the CloudWatch alarm associated with the scaling policy, even if it no longer has an associated action\. 

**To delete a step scaling policy and its associated CloudWatch alarm**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

1. Select the Auto Scaling group\.

1. On the **Scaling Policies** tab, choose **Actions**, **Delete**\. 

1. When prompted for confirmation, choose **Yes, Delete**\.

1. Do the following to delete the CloudWatch alarm that was associated with the policy\.

   1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   1. On the navigation pane, choose **Alarms**\.

   1. Choose the alarm and choose **Action**, **Delete**\.

   1. When prompted for confirmation, choose **Delete**\.

When you have completed this tutorial, you may choose to keep your Auto Scaling group and Amazon EC2 instances in service for your cluster\. However, if you are not actively using these resources, you should consider cleaning them up so your account does not incur unnecessary charges\. You can delete your Auto Scaling group to terminate the Amazon EC2 instances within it, but your launch configuration remains intact\. You can create a new Auto Scaling group with the launch configuration later, if you choose\.

**To delete your Auto Scaling group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the left navigation pane, choose **Auto Scaling Groups**\.

1. Choose the Auto Scaling group that you created earlier\.

1. Choose **Actions**, **Delete**\.

1. Choose **Yes, Delete**\.