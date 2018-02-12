# Target Tracking Scaling Policies<a name="service-autoscaling-targettracking"></a>

With target tracking scaling policies, you select a metric and set a target value\. Amazon ECS creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value\. The scaling policy adds or removes service tasks as required to keep the metric at, or close to, the specified target value\. In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to the fluctuations in the metric due to a fluctuating load pattern and minimizes rapid fluctuations in the number of tasks running in your service\.

You can create multiple target tracking scaling policies for an Amazon ECS service, provided that each of them uses a different metric\. The service scales based on the policy that provides the largest task capacity\. This enables you to cover multiple scenarios and ensure that there is always enough capacity to process your application workloads\.

To ensure application availability, the service scales out proportionally to the metric as fast as it can, but scales in more gradually\.

Do not edit or delete the CloudWatch alarms that Amazon ECS manages for a target tracking scaling policy\. Amazon ECS deletes the alarms automatically when you delete the target tracking scaling policy\.

## Considerations<a name="targettracking-considerations"></a>

Keep the following considerations in mind when creating a target tracking scaling policy:

+ A target tracking scaling policy assumes that it should perform scale out when the specified metric is above the target value\. You cannot use a target tracking scaling policy to scale out when the specified metric is below the target value\.

+ A target tracking scaling policy does not perform scaling when the specified metric has insufficient data\. It does not perform scale in because it does not interpret insufficient data as low utilization\. To scale in when a metric has insufficient data, create a step scaling policy and have an alarm invoke the scaling policy when it changes to the `INSUFFICIENT_DATA` state\.

+ You may see gaps between the target value and the actual metric data points\. This is because Application Auto Scaling always acts conservatively by rounding up or down when it determines how much capacity to add or remove\. This prevents it from adding insufficient capacity or removing too much capacity\. However, for a scalable target with small capacity, the actual metric data points might seem far from the target value\. For a scalable target with larger capacity, adding or removing capacity causes less of a gap between the target value and the actual metric data points\.

+ We recommend that you scale based on metrics with a 1\-minute frequency because that ensures a faster response to utilization changes\. Scaling on metrics with a 5\-minute frequency can result in slower response time and scaling on stale metric data\.

+ To ensure application availability, Application Auto Scaling scales out proportionally to the metric as fast as it can, but scales in more gradually\.

+ Do not edit or delete the CloudWatch alarms that Application Auto Scaling manages for a target tracking scaling policy\. Application Auto Scaling deletes the alarms automatically when you delete the scaling policy\.

## Tutorial: Service Auto Scaling with Target Tracking<a name="targettracking-tutorial"></a>

The following procedures help you to create an Amazon ECS cluster and a service that uses Application Auto Scaling to scale out \(and in\) using target tracking\. 

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. You can use these service utilization metrics to scale your service up to deal with high demand at peak times, and to scale your service down to reduce costs during periods of low utilization\. For more information, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\. 

In this tutorial, you create a cluster and a service \(that runs behind an Elastic Load Balancing load balancer\) using the Amazon ECS first run wizard\. Then you configure Service Auto Scaling on the service with CloudWatch alarms that use the `CPUUtilization` metric to scale your service up or down, depending on the current application load\. 

When the CPU utilization of your service rises above 75% \(meaning that more than 75% of the CPU that is reserved for the service is being used\), the scale\-out alarm triggers Service Auto Scaling to add another task to your service to help out with the increased load\. Conversely, when the CPU utilization of your service drops below 25%, the scale in alarm triggers a decrease in the service's desired count to free up those cluster resources for other tasks and services\.

### Prerequisites<a name="tt-prereqs"></a>

This tutorial assumes that you have an AWS account, an IAM administrator with permissions to perform all of the actions described within, and an Amazon EC2 key pair in the current region\. If you do not have these resources, or your are not sure, you can create them by following the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.

If you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, then you might need to add this permission\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

### Step 1: Create a Cluster and a Service<a name="tt-create-cluster-service"></a>

After you have enabled CloudWatch metrics for your clusters and services, you can create a cluster and service using the Amazon ECS first\-run wizard\. The first\-run wizard takes care of creating the necessary IAM roles and policies for this tutorial, an Auto Scaling group for your container instances, and a service that runs behind a load balancer\. The wizard also makes the clean\-up process much easier, because you can delete the entire AWS CloudFormation stack in one step\.

For this tutorial, you create a cluster called `service-autoscaling` and a service called `sample-webapp`\.

**To create your cluster and service**

1. Open the Amazon ECS console first run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. From the navigation bar, choose the **US East \(N\. Virginia\)** region\.

1. On **Step 1: Container and Task**, for **Container definition**, select **sample\-app**\.

1. For **Task definition**, leave all of the default options and choose **Next**\.

1. On **Step 2: Service**, for **Load balancer type**, choose **Application Load Balancer**, **Next**\.
**Important**  
Application Load Balancers do incur costs while they exist in your AWS resources\. For more information, see [Elastic Load Balancing Pricing](http://aws.amazon.com/elasticloadbalancing/pricing/)\.

1. On **Step 3: Cluster**, for **Cluster name**, enter `service-autoscaling` and choose **Next**\.

1. Review your choices and then choose **Create**\. 

   You are directed to a **Launch Status** page that shows the status of your launch and describes each step of the process \(this can take a few minutes to complete while your cluster resources are created and populated\)\.

1. When your cluster and service are created, choose **View service**\.

### Step 2: Configure Service Auto Scaling<a name="tt-configure-autoscaling"></a>

Now that you have launched a cluster and created a service in that cluster that is running behind a load balancer, you can configure Service Auto Scaling by creating scaling policies to scale your service out and in in response to CloudWatch alarms\.

**To configure basic Service Auto Scaling parameters**

1. On the **Service: sample\-app\-service** page, your service configuration should look similar to the image below \(although the task definition revision and load balancer name will likely be different\)\. Choose **Update** to update your new service\.  
![\[Choose your configuration options\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/sample-app-service.png)

1. On the **Update service** page, choose **Next step** until you get to **Step 3: Set Auto Scaling \(optional\)**\.

1. For **Service Auto Scaling**, choose **Configure Service Auto Scaling to adjust your service’s desired count**\.

1. For **Minimum number of tasks**, enter `1` for the lower limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted below this amount\.

1. For **Desired number of tasks**, this field is pre\-populated with the value that you entered earlier\. This value must be between the minimum and maximum number of tasks specified on this page\. Leave this value at `1`\.

1. For **Maximum number of tasks**, enter `2` for the upper limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count is not automatically adjusted above this amount\.

1. For **IAM role for Service Auto Scaling**, choose an IAM role to authorize the Application Auto Scaling service to adjust your service's desired count on your behalf\. If you have not previously created such a role, choose **Create new role** and the role is created for you\. For future reference, the role that is created for you is called `ecsAutoscaleRole`\. For more information, see [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

**To configure scaling policies for your service**

These steps help you create scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. You can create a scale\-out alarm to increase the desired count of your service, and a scale in alarm to decrease the desired count of your service\.

1. Choose **Add scaling policy** to configure your scaling policy\.

1. On the **Add policy** page, do the following:

   1. For **Scaling policy type**, choose **Target tracking**\.

   1. For **Policy name**, enter `TargetTrackingPolicy`\.

   1. For **ECS service metric**, choose **CPUUtilization**\.

   1. For **Target value**, enter `75`\.

   1. For **Scale\-out cooldown period**, enter `60`\. This is the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. During this time, resources that have been launched do not contribute to the Auto Scaling group metrics\.

   1. For **Scale\-in cooldown period**, enter `60`\. This is the amount of time, in seconds, after a scale in activity completes before another scale in activity can start\. During this time, resources that have been launched do not contribute to the Auto Scaling group metrics\.

   1. Choose **Save**\.

1. Choose **Next step**\.

1. Review all of your choices and then choose **Update Service**\.

1. When your service status is finished updating, choose **View Service**\.

### Step 3: Trigger a Scaling Activity<a name="tt-trigger-scaling"></a>

After your service is configured with Service Auto Scaling, you can trigger a scaling activity by pushing your service's CPU utilization into the `ALARM` state\. Because the example in this tutorial is a web application that is running behind a load balancer, you can send thousands of HTTP requests to your service \(using the ApacheBench utility\) to spike the service CPU utilization above the threshold amount\. This spike should trigger the alarm, which in turn triggers a scaling activity to add one task to your service\.

After the ApacheBench utility finishes the requests, the service CPU utilization should drop below your 25% threshold, triggering a scale in activity that returns the service's desired count to 1\.

**To trigger a scaling activity for your service**

1. From your service's main view page in the console, choose the load balancer name to view its details in the Amazon EC2 console\. You need the load balancer's DNS name, which should look something like `EC2Contai-EcsElast-SMAKV74U23PH-96652279.us-east-1.elb.amazonaws.com`\.

1. Use the ApacheBench \(ab\) utility to make thousands of HTTP requests to your load balancer in a short period of time\.
**Note**  
This command is installed by default on macOS, and it is available for many Linux distributions, as well\. For example, you can install ab on Amazon Linux with the following command:  

   ```
   $ sudo yum install -y httpd24-tools
   ```

   Run the following command, substituting your load balancer's DNS name\.

   ```
   $ ab -n 100000 -c 1000 http://EC2Contai-EcsElast-SMAKV74U23PH-96652279.us-east-1.elb.amazonaws.com/
   ```

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Alarms**\.

1. Wait for your ab HTTP requests to trigger the scale\-out alarm in the CloudWatch console\. You should see your Amazon ECS service scale out and add 1 task to your service's desired count\.

1. Shortly after your ab HTTP requests complete \(between 1 and 2 minutes\), your scale in alarm should trigger and the scale in policy reduces your service's desired count back to 1\.

### Step 4: Cleaning Up<a name="tt-cleanup"></a>

When you have completed this tutorial, you may choose to keep your cluster, Auto Scaling group, load balancer, and CloudWatch alarms\. However, if you are not actively using these resources, you should consider cleaning them up so that your account does not incur unnecessary charges\.

**To delete your cluster and CloudWatch alarms**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the left navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the **service\-autoscaling** cluster\.

1. Choose **Delete Cluster**, **Delete**\. It may take a few minutes for the cluster AWS CloudFormation stack to finish cleaning up\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Alarms** and select the alarms that begin with `TargetTracking-service-`\.

1. Choose **Delete**, **Yes, Delete**\.