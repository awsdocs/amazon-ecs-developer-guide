# Step 5: Configuring your service to use Service Auto Scaling<a name="service-configure-auto-scaling"></a>

Your Amazon ECS service can optionally be configured to use Auto Scaling to adjust its desired count of tasks in your Amazon ECS service up or down in response to CloudWatch alarms\. 

Amazon ECS Service Auto Scaling supports the following types of scaling policies:
+ [Target Tracking Scaling Policies](service-autoscaling-targettracking.md) \(Recommended\)—Increase or decrease  the number of tasks that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select temperature and the thermostat does the rest\.
+ [Step Scaling Policies](service-autoscaling-stepscaling.md)—Increase or decrease the number of tasks that your service runs based on a set of scaling adjustments, known as step adjustments, which vary based on the size of the alarm breach\.

For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

**To configure basic Service Auto Scaling parameters**

1. If you have not done so already, follow the basic service configuration procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. On the **Set Auto Scaling** page, select **Configure Service Auto Scaling to adjust your service’s desired count**\.

1. For **Minimum number of tasks**, enter the lower limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted below this amount\.

1. For **Desired number of tasks**, this field is pre\-populated with the value that you entered earlier\. You can change your service's desired count at this time, but this value must be between the minimum and maximum number of tasks specified on this page\.

1. For **Maximum number of tasks**, enter the upper limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted above this amount\.

1. For **IAM role for Service Auto Scaling**, choose the `ecsAutoscaleRole`\. If this role does not exist, choose **Create new role** to have the console create it for you\.

1. The following procedures provide steps for creating either target tracking or step scaling policies for your service\. Choose your desired scaling policy type\.

These steps help you create target tracking scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. 

**To configure target tracking scaling policies for your service**

1. For **Scaling policy type**, choose **Target tracking**\.

1. For **Policy name**, enter a descriptive name for your policy\.

1. For **ECS service metric**, choose the metric to track\. The following metrics are available: 
   + **ECSServiceAverageCPUUtilization**—Average CPU utilization of the service\.
   + **ECSServiceAverageMemoryUtilization**—Average memory utilization of the service\.
   + **ALBRequestCountPerTarget**—Number of requests completed per target in an Application Load Balancer target group\.

1. For **Target value**, enter the metric value that the policy should maintain\. For example, use a target value of `1000` for `ALBRequestCountPerTarget`, or a target value of `75`\(%\) for `ECSServiceAverageCPUUtilization`\.

1. For **Scale\-out cooldown period**, enter the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. While the scale\-out cooldown period is in effect, the capacity that has been added by the previous scale\-out activity that initiated the cooldown is calculated as part of the desired capacity for the next scale out\. The intention is to continuously \(but not excessively\) scale out\.

1. For **Scale\-in cooldown period**, enter the amount of time, in seconds, after a scale\-in activity completes before another scale\-in activity can start\. The scale\-in cooldown period is used to block subsequent scale\-in requests until it has expired\. The intention is to scale in conservatively to protect your application's availability\. However, if another alarm triggers a scale out activity during the cooldown period after a scale\-in, Service Auto Scaling scales out your scalable target immediately\. 

1. \(Optional\) To disable the scale\-in actions for this policy, choose **Disable scale\-in**\. This allows you to create a separate scaling policy for scale\-in later\.

1. Choose **Next step**\.

These steps help you create step scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. You can create a **Scale out** alarm to increase the desired count of your service, and a **Scale in** alarm to decrease the desired count of your service\.

**To configure step scaling policies for your service**

1. <a name="policy-name-step"></a>For **Scaling policy type**, choose **Step scaling**\.

1. For **Policy name**, enter a descriptive name for your policy\.

1. For **Execute policy when**, select the CloudWatch alarm to use to scale your service up or down\.

   You can use an existing CloudWatch alarm that you have previously created, or you can choose to create a new alarm\. The **Create new alarm** workflow allows you to create CloudWatch alarms that are based on the `CPUUtilization` and `MemoryUtilization` of the service that you are creating\. To use other metrics, you can create your alarm in the CloudWatch console and then return to this wizard to choose that alarm\.

1. \(Optional\) If you've chosen to create a new alarm, complete the following steps\.

   1. For **Alarm name**, enter a descriptive name for your alarm\. For example, if your alarm should trigger when your service CPU utilization exceeds 75%, you could call the alarm `service_name-cpu-gt-75`\.

   1. For **ECS service metric**, choose the service metric to use for your alarm\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

   1. For **Alarm threshold**, enter the following information to configure your alarm:
      + Choose the CloudWatch statistic for your alarm \(the default value of **Average** works in many cases\)\. For more information, see [Statistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistic) in the *Amazon CloudWatch User Guide*\.
      + Choose the comparison operator for your alarm and enter the value that the comparison operator checks against \(for example, `>` and `75`\)\.
      + Enter the number of consecutive periods before the alarm is triggered and the period length\. For example, two consecutive periods of 5 minutes would take 10 minutes before the alarm triggered\. Because your Amazon ECS tasks can scale up and down quickly, consider using a low number of consecutive periods and a short period duration to react to alarms as soon as possible\.

   1. Choose **Save**\.

1. <a name="scaling-action-step-adjustment"></a>For **Scaling action**, enter the following information to configure how your service responds to the alarm:
   + Choose whether to add to, subtract from, or set a specific desired count for your service\.
   + If you chose to add or subtract tasks, enter the number of tasks \(or percent of existing tasks\) to add or subtract when the scaling action is triggered\. If you chose to set the desired count, enter the desired count that your service should be set to when the scaling action is triggered\. 
   + \(Optional\) If you chose to add or subtract tasks, choose whether the previous value is used as an integer or a percent value of the existing desired count\.
   + Enter the lower boundary of your step scaling adjustment\. By default, for your first scaling action, this value is the metric amount where your alarm is triggered\. For example, the following scaling action adds 100% of the existing desired count when the CPU utilization is greater than 75%\.  
![\[Scaling activity example\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scaling-activity.png)

1. \(Optional\) You can repeat [Step 5](#scaling-action-step-adjustment) to configure multiple scaling actions for a single alarm \(for example, to add one task if CPU utilization is between 75\-85%, and to add two tasks if CPU utilization is greater than 85%\)\.

1. \(Optional\) If you chose to add or subtract a percentage of the existing desired count, enter a minimum increment value for **Add tasks in increments of *N* task\(s\)**\.

1. <a name="cooldown-period-step"></a>For **Cooldown period**, enter the number of seconds between scaling actions\.

1. Repeat [Step 1](#policy-name-step) through [Step 8](#cooldown-period-step) for the **Scale in** policy and choose **Save**\.

1. Choose **Next step** to proceed and navigate to [Step 6: Review and create your service](create-service-review.md)\.