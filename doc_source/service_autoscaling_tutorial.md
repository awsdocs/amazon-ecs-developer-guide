# Tutorial: Service Auto Scaling with CloudWatch Service Utilization Metrics<a name="service_autoscaling_tutorial"></a>

The following procedures help you to create an Amazon ECS cluster and a service that uses Service Auto Scaling to scale up \(and down\) using CloudWatch alarms\. 

Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage\. You can use these service utilization metrics to scale your service up to deal with high demand at peak times, and to scale your service down to reduce costs during periods of low utilization\. For more information, see [Service Utilization](cloudwatch-metrics.md#service_utilization)\. 

In this tutorial, you create a cluster and a service \(that runs behind an Elastic Load Balancing load balancer\) using the Amazon ECS first run wizard\. Then you configure Service Auto Scaling on the service with CloudWatch alarms that use the `CPUUtilization` metric to scale your service up or down, depending on the current application load\. 

When the CPU utilization of your service rises above 75% \(meaning that more than 75% of the CPU that is reserved for the service is being used\), the scale out alarm triggers Service Auto Scaling to add another task to your service to help out with the increased load\. Conversely, when the CPU utilization of your service drops below 25%, the scale in alarm triggers a decrease in the service's desired count to free up those cluster resources for other tasks and services\.

## Prerequisites<a name="sas-tutorial-prereqs"></a>

This tutorial assumes that you have an AWS account and an IAM administrative user with permissions to perform all of the actions described within, and an Amazon EC2 key pair in the current region\. If you do not have these resources, or your are not sure, you can create them by following the steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)\.

Your Amazon ECS container instances also require `ecs:StartTelemetrySession` permission on the IAM role that you launch your container instances with\. If you created your Amazon ECS container instance role before CloudWatch metrics were available for Amazon ECS, then you might need to add this permission\. For information about checking your Amazon ECS container instance role and attaching the managed IAM policy for container instances, see [To check for the `ecsInstanceRole` in the IAM console](instance_IAM_role.md#procedure_check_instance_role)\.

## Step 1: Create a Cluster and a Service<a name="sas-tutorial-create-cluster-service"></a>

After you have enabled CloudWatch metrics for your clusters and services, you can create a cluster and service using the Amazon ECS first run wizard\. The first run wizard takes care of creating the necessary IAM roles and policies for this tutorial, an Auto Scaling group for your container instances, and it creates a service that runs behind a load balancer\. The wizard also makes the later clean up process much easier, because you can delete the entire AWS CloudFormation stack in one step\.

For this tutorial, you create a cluster called `service-autoscaling` and a service called `sample-webapp`\.

**To create your cluster and service**

1. Open the Amazon ECS console first run wizard at [https://console\.aws\.amazon\.com/ecs/home\#/firstRun](https://console.aws.amazon.com/ecs/home#/firstRun)\.

1. By default, you are given the option to create an image repository and push an image to Amazon ECR\.  
![\[Choose your configuration options\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/deselect-ecr-option.png)

   For this tutorial, you will not use Amazon ECR, so be sure to clear the lower option\. Choose **Continue** to proceed\.

1. On the **Create a task definition** page, leave all of the default options and choose **Next step**\.

1. On the **Configure service** page, for **Container name: host port**, choose **simple\-app:80**\.
**Important**  
Elastic Load Balancing load balancers do incur cost while they exist in your AWS resources\. For more information, see [Elastic Load Balancing Pricing](http://aws.amazon.com/elasticloadbalancing/pricing/)\.

1. For **Select IAM role for service**, choose an existing Amazon ECS service \(`ecsServiceRole`\) role that you have already created, or choose **Create new role** to create the required IAM role for your service\.

1. The remaining default values here are set up for the sample application, so leave them as they are and choose **Next step**\.

1. On the **Configure cluster** page, enter the following information:

   1. For **Cluster name**, type `service-autoscaling`\.

   1. For instance type, choose any available instance type\. The default `t2.micro` works fine for this tutorial\.

   1. For **Number of instances**, enter the number of instances to launch into your cluster\. For the purposes of this tutorial, two instances are sufficient\.
**Important**  
Your AWS account incurs the standard Amazon EC2 usage fees for these instances from the time that you launch the instances until you terminate them \(which is the final task of this tutorial\), even if they remain idle\.

   1. \(Optional\) For **Key pair**, choose a key pair to use for SSH access to your instances\. This is not required, but it can be useful for diagnostic purposes if you need to troubleshoot your instances later\.

   1. For **Container instance IAM role**, choose an existing Amazon ECS container instance \(`ecsInstanceRole`\) role that you have already created, or choose **Create new role** to create the required IAM role for your container instances\.

   1. Choose **Review and Launch** to proceed\. Review your configurations and choose **Launch instance & run service** to finish\. 

   You are directed to a **Launch Status** page that shows the status of your launch and describes each step of the process \(this can take a few minutes to complete while your Auto Scaling group is created and populated\)\.

1. When your cluster and service are created, choose **View service** to view your new service\.

## Step 2: Configure Service Auto Scaling<a name="sas-tutorial-configure-autoscaling"></a>

Now that you have launched a cluster and created a service in that cluster that is running behind a load balancer, you can configure Service Auto Scaling by creating scaling policies to scale your service up and down in response to CloudWatch alarms\.

**To configure basic Service Auto Scaling parameters**

1. On the **Service: sample\-webapp** page, your service configuration should look similar to the image below \(although the task definition revision and load balancer name will likely be different\)\. Choose **Update** to update your new service\.  
![\[Choose your configuration options\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/sample-webapp-service.png)

1. On the **Update service** page, choose **Configure Service Auto Scaling**\.

1. For **Service Auto Scaling**, choose **Configure Service Auto Scaling to adjust your service’s desired count**\.  
![\[Choose your configuration options\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/service-auto-scaling-radio-button.png)

1. For **Minimum number of tasks**, enter 1 for the lower limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count will not be automatically adjusted below this amount\.

1. For **Desired number of tasks**, this field is pre\-populated with the value you entered earlier\. This value must be between the minimum and maximum number of tasks specified on this page\. Leave this value at 1\.

1. For **Maximum number of tasks**, enter 2 for the upper limit of the number of tasks for Service Auto Scaling to use\. Your service's desired count will not be automatically adjusted above this amount\.

1. For **IAM role for Service Auto Scaling**, choose an IAM role to authorize the Application Auto Scaling service to adjust your service's desired count on your behalf\. If you have not previously created such a role, choose **Create new role** and the role is created for you\. For future reference, the role that is created for you is called `ecsAutoscaleRole`\. For more information, see [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)\.

**To configure scaling policies for your service**

These steps will help you create scaling policies and CloudWatch alarms that can be used to trigger scaling activities for your service\. You can create a scale out alarm to increase the desired count of your service, and a scale in alarm to decrease the desired count of your service\.

1. On the **Service Auto Scaling \(optional\)** page, choose **Add scaling policy** to configure your `ScaleOutPolicy`\.

1. For **Policy name**, enter `ScaleOutPolicy`

1. For **Execute policy when**, choose **Create new alarm**\.

   1. For **Alarm name**, enter `sample-webapp-cpu-gt-75`\.

   1. For **ECS service metric**, choose **CPUUtilization**\.

   1. For **Alarm threshold**, enter the following information to match the image below\. This causes the CloudWatch alarm to trigger when the service's CPU utilization is greater than 75% for one minute\.  
![\[Scale out alarm\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scale-out-alarm.png)

   1. Choose **Save** to save your alarm\.

1. For **Scaling action**, enter the following information to match the image below\. This causes your service's desired count to increase by 1 task when the alarm is triggered\.  
![\[Scale out action\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scale-out-action.png)

1. For **Cooldown period**, enter 60 for the number of seconds between scaling actions and choose **Save** to save your `ScaleOutPolicy`\.

1. After you return to the **Service Auto Scaling \(optional\)** page, choose **Add scaling policy** to configure your `ScaleInPolicy`\.

1. For **Policy name**, enter `ScaleInPolicy`

1. For **Execute policy when**, choose **Create new alarm**\.

   1. For **Alarm name**, enter `sample-webapp-cpu-lt-25`\.

   1. For **ECS service metric**, choose **CPUUtilization**\.

   1. For **Alarm threshold**, enter the following information to match the image below\. This causes the CloudWatch alarm to trigger when the service's CPU utilization is less than 25% for one minute\.  
![\[Scale in alarm\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scale-in-alarm.png)

   1. Choose **Save** to save your alarm\.

1. For **Scaling action**, enter the following information to match the image below\. This causes your service's desired count to decrease by 1 task when the alarm is triggered\.  
![\[Scale in action\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scale-in-action.png)

1. For **Cooldown period**, enter 60 for the number of seconds between scaling actions and choose **Save** to save your `ScaleInPolicy`\.

1. After you return to the **Service Auto Scaling \(optional\)** page, choose **Save** to finish your Service Auto Scaling configuration\.

1. On the **Update Service** page, choose **Update Service**\.

1. When your service status is finished updating, choose **View Service**\.

## Step 3: Trigger a Scaling Activity<a name="sas-tutorial-trigger-scaling"></a>

After your service is configured with Service Auto Scaling, you can trigger a scaling activity by pushing your service's CPU utilization into the `ALARM` state\. Because the example in this tutorial is a web application that is running behind a load balancer, you can send thousands of HTTP requests to your service \(using the ApacheBench utility\) to spike the service CPU utilization above our threshold amount\. This spike should trigger the alarm, which in turn triggers a scaling activity to add one task to your service\.

After the ApacheBench utility finishes the requests, the service CPU utilization should drop below your 25% threshold, triggering a scale in activity that returns the service's desired count to 1\.

**To trigger a scaling activity for your service**

1. From your service's main view page in the console, choose the load balancer name to view its details in the Amazon EC2 console\. You need the load balancer's DNS name, which should look something like this: `EC2Contai-EcsElast-SMAKV74U23PH-96652279.us-east-1.elb.amazonaws.com`\.

1. Use the ApacheBench \(ab\) utility to make thousands of HTTP requests to your load balancer in a short period of time\.
**Note**  
This command is installed by default on Mac OSX, and it is available for many Linux distributions, as well\. For example, you can install ab on Amazon Linux with the following command:  

   ```
   $ sudo yum install -y httpd24-tools
   ```

   Run the following command, substituting your load balancer's DNS name\.

   ```
   $ ab -n 100000 -c 1000 http://EC2Contai-EcsElast-SMAKV74U23PH-96652279.us-east-1.elb.amazonaws.com/
   ```

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Alarms** in the left navigation pane\.

1. Wait for your ab HTTP requests to trigger the scale out alarm in the CloudWatch console\. You should see your Amazon ECS service scale out and add 1 task to your service's desired count\.

1. Shortly after your ab HTTP requests complete \(between 1 and 2 minutes\), your scale in alarm should trigger and the scale in policy reduces your service's desired count back to 1\.

## Step 4: Cleaning Up<a name="sas-tutorial-cleanup"></a>

When you have completed this tutorial, you may choose to keep your cluster, Auto Scaling group, load balancer, and EC2 instances\. However, if you are not actively using these resources, you should consider cleaning them up so that your account does not incur unnecessary charges\.

**To delete your cluster and CloudWatch alarms**

1. In the Amazon ECS console, switch to **Clusters** in the left navigation pane\.

1. On the **Clusters** page, choose the **x** in the upper right hand corner of the **service\-autoscaling** cluster to delete the cluster\.

1. Review and choose **Delete** to confirm your cluster deletion\. It may take a few minutes for the cluster AWS CloudFormation stack to finish cleaning up\.

1. In the CloudWatch console **Alarms** view, select the alarms that begin with **sample\-webapp\-cpu\-** and then choose **Delete** to delete the alarms\.

1. Choose **Yes, Delete** to confirm your alarm deletion\.