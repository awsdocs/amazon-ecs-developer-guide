# Tutorial: Scaling Container Instances with CloudWatch Alarms<a name="cloudwatch_alarm_autoscaling"></a>

The following procedures help you to create an Auto Scaling group for an Amazon ECS cluster that contain container instances that you can scale up \(and down\) using CloudWatch alarms\. 

Depending on the Amazon EC2 instance types you use in your clusters, and quantity of container instances you have in a cluster, your tasks have a limited amount of resources that they can use when they are run\. Amazon ECS monitors the resources available in the cluster to work with the schedulers to place tasks\. If your cluster runs low on any of these resources, such as memory, you will eventually be unable to launch more tasks until you add more container instances, reduce the number of desired tasks in a service, or stop some of the running tasks in your cluster to free up the constrained resource\.

In this tutorial, you create a CloudWatch alarm using the `MemoryReservation` metric for your cluster\. When the memory reservation of your cluster rises above 75% \(meaning that only 25% of the memory in your cluster is available to for new tasks to reserve\), the alarm triggers the Auto Scaling group to add another instance and provide more resources for your tasks and services\.

## Prerequisites<a name="as-cw-tutorial-prereqs"></a>

This tutorial assumes that you have enabled CloudWatch metrics for your clusters and services\. Metrics are not available until the clusters and services send the metrics to CloudWatch, and you cannot create CloudWatch alarms for metrics that do not exist yet\.

Your Amazon ECS container instances require at least version 1\.4\.0 of the container agent to enable CloudWatch metrics\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

Your Amazon ECS container instances also require `ecs:StartTelemetrySession` permission on the IAM role that you launch your container instances with\. If you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, then you might need to add this permission\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

## Step 1: Create a CloudWatch Alarm for a Metric<a name="create-cw-alarms"></a>

After you have enabled CloudWatch metrics for your clusters and services, and the metrics for your cluster are visible in the CloudWatch console, you can set alarms on the metrics\. For more information, see [Creating Amazon CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

For this tutorial, you create an alarm on the cluster `MemoryReservation` metric to alert when the cluster's memory reservation is above 75%\.

**To create a CloudWatch alarm on a metric**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the left navigation, choose **Alarms**\.

1. Choose **Create Alarm**\.

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

## Step 2: Create a Launch Configuration for an Auto Scaling Group<a name="create-as-group"></a>

Now that you have enabled CloudWatch metrics and created an alarm based on one of those metrics, you can create a launch configuration and an Auto Scaling group for your cluster\. For more information and other configuration options, see the [Amazon EC2 Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/latest/userguide/)\.

**To create an Auto Scaling launch configuration**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the left navigation, choose **Auto Scaling Groups**\.

1. On the **Welcome to Auto Scaling** page, choose **Create Auto Scaling Group**\.

1. On the **Create Auto Scaling Group** page, choose **Create launch configuration**\.

1. On the **Choose AMI** step of the **Create Auto Scaling Group** wizard, choose **Community AMIs**\.

1. Choose the ECS\-optimized AMI for your Auto Scaling group\.

   To use the Amazon ECS\-optimized AMI, type **amazon\-ecs\-optimized** in the **Search community AMIs** field and press the **Enter** key\. Choose **Select** next to the **amzn\-ami\-2017\.09\.j\-amazon\-ecs\-optimized** AMI\.

   The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch_alarm_autoscaling.html)

1. On the **Choose Instance Type** step of the **Create Auto Scaling Group** wizard, choose an instance type for your Auto Scaling group and choose **Next: Configure details**\.

1. On the **Configure details** step of the **Create Auto Scaling Group** wizard, enter the following information\. The other fields are optional\. For more information, see [Creating Launch Configurations](http://docs.aws.amazon.com/autoscaling/latest/userguide/WorkingWithLaunchConfig.html) in the *Amazon EC2 Auto Scaling User Guide*\.

   + **Name:** Enter a name for your launch configuration\.

   + **IAM role:** Select the `ecsInstanceRole` for your container instances\. If you do not have this role configured, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   + **IP Address Type:** Choose the IP address type option that you want for your container instances\. If you want external traffic to be able to reach your containers, choose **Assign a public IP address to every instance\.**

1. \(Optional\) If you have configuration information that you want to pass to your container instances with EC2 user data, choose **Advanced Details** and enter your user data in the **User data** field\. For more information, see [Amazon ECS Container Agent Configuration](ecs-agent-config.md)\.

1. Choose **Next: Add Storage**\.

1. On the **Add Storage** step of the **Create Auto Scaling Group** wizard, make any storage configuration changes you need for your instances and choose **Next: Configure Security Group**\.

1. On the **Configure Security Group** step of the **Create Auto Scaling Group** wizard, select an existing security group that meets the needs of your containers, or create a new security group and choose **Review**\.

1. Review your launch configuration and choose **Create launch configuration**\.

1. Select a private key to use for connecting to your instances with SSH and choose **Create launch configuration** to finish and move on to creating an Auto Scaling group with your new launch configuration\.

## Step 3: Create an Auto Scaling Group for your Cluster<a name="w3ab1c32c21c21c15"></a>

After the launch configuration is complete, continue with the following procedure to create an Auto Scaling group that uses your launch configuration\.

**To create an Auto Scaling group**

1. On the **Configure Auto Scaling group details** step of the **Create Auto Scaling Group** wizard, enter the following information and choose **Next: Configure scaling policies**\.

   + **Group name:** Enter a name for your Auto Scaling group\.

   + **Group size:** Specify the number of container instances your Auto Scaling group should start with\.

   + **Network:** Choose a VPC to launch your container instances into\.

   + **Subnet:** Choose the subnets you would like to launch your container instances into\. For a highly available cluster, we recommend that you enable all of the subnets in the region\.

1. On the **Configure scaling policies** step of the **Create Auto Scaling Group** wizard, choose **Use scaling policies to adjust the capacity of this group**\.

1. Enter the minimum and maximum number of container instances for your Auto Scaling group\. 

1. In the **Increase Group Size** section, enter the following information\.

   + **Execute policy when:** Choose the `memory-above-75-pct` CloudWatch alarm you configured earlier\.

   + **Take the action:** Enter the number of instances you would like to add to your cluster when the alarm is triggered\.

1. If you configured an alarm to trigger a group size reduction, set that alarm in the **Decrease Group Size** section and specify how many instances to remove if that alarm is triggered\. Otherwise, collapse the **Decrease Group Size** section by clicking the **X** in the upper\-right\-hand corner of the section\.
**Note**  
If you configure your Auto Scaling group to remove container instances, any tasks running on the removed container instances are killed\. If your tasks are running as part of a service, Amazon ECS restarts those tasks on another instance if the required resources are available \(CPU, memory, ports\); however, tasks that were started manually will are not restarted automatically\.

1. Choose **Review** to review your Auto Scaling group and then choose **Create Auto Scaling Group** to finish\.

## Step 4: Verify and Test your Auto Scaling Group<a name="w3ab1c32c21c21c17"></a>

Now that you've created your Auto Scaling group, you should be able to see your instances launching in the Amazon EC2 console **Instances** page\. These instances should register into your Amazon ECS cluster as well after they launch\.

To test that your Auto Scaling group is configured properly, you can create some tasks that consume a considerable amount of memory and start launching them into your cluster\. After your cluster exceeds the 75% memory reservation from the CloudWatch alarm for the specified number of periods, you should see a new instance launch in the EC2 console\.

## Step 5: Cleaning Up<a name="w3ab1c32c21c21c19"></a>

When you have completed this tutorial, you may choose to keep your Auto Scaling group and Amazon EC2 instances in service for your cluster\. However, if you are not actively using these resources, you should consider cleaning them up so your account does not incur unnecessary charges\. You can delete your Auto Scaling group to terminate the Amazon EC2 instances within it, but your launch configuration remains intact and you can create a new Auto Scaling group with it later if you choose\.

**To delete your Auto Scaling group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the left navigation, choose **Auto Scaling Groups**\.

1. Choose the Auto Scaling group you created for this tutorial\.

1. Choose **Actions** and then choose **Delete**\.

1. Choose **Yes, Delete** to delete your Auto Scaling group\.