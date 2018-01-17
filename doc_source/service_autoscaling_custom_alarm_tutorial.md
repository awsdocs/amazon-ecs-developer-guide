# Tutorial: Service Auto Scaling with Custom CloudWatch Metrics<a name="service_autoscaling_custom_alarm_tutorial"></a>

The following procedures help you to create a custom CloudWatch alarm and associate it with Service Auto Scaling to scale your service up \(and down\) \. 

In this tutorial, you configure Service Auto Scaling on the service with a custom CloudWatch alarm that uses the `HealthyHostCount` metric to scale your service up or down, depending on the number of healthy hosts behind your Application Load Balancer\.

## Prerequisites<a name="sas-tutorial-custom-prereqs"></a>

This tutorial assumes that you have an AWS account and an IAM administrator with permissions to perform all of the actions described within\. This tutorial also assumes that you have already created your cluster and service that includes an Application Load Balancer\. If you do not have these resources, or you are not sure, you can create them by following the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.

Your Amazon ECS container instances also require `ecs:StartTelemetrySession` permissions on the IAM role that you launch your container instances with\. If you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, then you might need to add this permission\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

## Step 1: Create a CloudWatch Alarm<a name="sas-tutorial-create-cloudwatch-alarm"></a>

Create the custom CloudWatch alarm to use as an scaling trigger\.

**Creating a CloudWatch alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Under **CloudWatch Metrics by Category**, choose the **ApplicationELB Metrics** category\.

1. Select the row with the **Per AppELB, per TG Metrics** and the **HealthyHostCount** metric\.

1. For the statistic, choose **Minimum**\.

1. For the period, choose **1 Minute**\.

1. Choose **Next**\.

1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, **HealthyHostCount**\) and a description of the alarm \(for example, **Alarm when all hosts are unhealthy**\)\.

1. Under **Whenever**, for **is**, select **<** and type **1**\. For **for**, type **3**\.

1. Under **Additional settings**, for **Treat missing data as**, choose **ignore \(maintain alarm state\)** so that missing data points do not trigger alarm state changes\.

1. Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose an existing SNS topic or create a new one\.

   To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, **HealthyHostCount**\), and for **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent\.

1. Choose **Create Alarm**\.

## Step 2: Update a Service with Auto Scaling Configuration<a name="sas-tutorial-update-cluster-service"></a>

After you have created a CloudWatch alarm for your cluster and service, you can update your service to associate it with the alarm\.

**To update a running service**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster that your service resides in\.

1. On the **Cluster: *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Update**\.

1. On the **Update Service** page, choose **Configure Service Auto Scaling**\.

1. On the **Service Auto Scaling** page, do the following:

   1. Select **Configure Service Auto Scaling to adjust your service’s desired count**\.

   1. For **Minimum number of tasks**, enter **1**\.

   1. For **Desired number of tasks**, enter **2**\.

   1. For **Maximum number of tasks**, enter **3**\.

   1. For **IAM role for Service Auto Scaling**, choose an IAM role to authorize the Application Auto Scaling service to adjust your service's desired count on your behalf\. If you have not previously created such a role, choose **Create new role** and the role is created for you\. For future reference, the role that is created for you is called `ecsAutoscaleRole`\. For more information, see [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

1. Under the **Automatic task scaling policies** section, choose **Add scaling policy**\.

1. On the **Add policy** page, do the following:

   1. For **Policy name**, enter a descriptive name for your policy \(for example, **HealthyHostCount**\)\.

   1. For **Execute policy when**, select **Use an existing Alarm** and choose the alarm that you created in the previous section\.

   1. For **Scaling action**, select **Add** and then enter **1** for the number of **tasks** when **0** > HealthyHostCount > \-infinity\.

   1. \(Optional\) You can repeat [[ERROR] BAD/MISSING LINK TEXT](#scaling-action-step) to configure multiple scaling actions for a single alarm \(for example, to remove one task if HealthyHostCount is above 3\)\.

   1. For **Cooldown period**, enter **300** as the number of seconds between scaling actions\.

   1. Choose **Save**\.

1. On the **Service Auto Scaling** page, choose **Save** to complete the update of your service\.