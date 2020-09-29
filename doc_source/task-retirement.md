# Task retirement<a name="task-retirement"></a>

Amazon ECS task retirement affects tasks of both Fargate and EC2 launch types and you will be notified by email of the pending retirement\.

A task can be scheduled for retirement in the following scenarios:
+ AWS detects the irreparable failure of the underlying hardware hosting the task\.
+ Your task uses the Fargate launch type and is running on a platform version that has a security vulnerability that requires you to replace the tasks by launching new tasks using a patched platform version\.

If your task is scheduled for retirement, you receive an email before the event with the task ID and retirement date\. This email is sent to the address that's associated with your account, the same email address that you use to log in to the AWS Management Console\. If you use an email account that you do not check regularly, then you can use the [AWS Personal Health Dashboard](http://aws.amazon.com/premiumsupport/phd/) to determine if any of your tasks are scheduled for retirement\. To update the contact information for your account, go to the [Account Settings](https://console.aws.amazon.com/billing/home?#/account) page\.

When a task reaches its scheduled retirement date, it is stopped or terminated by AWS\. If the task is part of a service, then the task is automatically stopped and the service scheduler launches a new one to replace it\. If you are using standalone tasks, then you receive notification of the task retirement and must launch new tasks to replace them\.

## Working with tasks scheduled for retirement<a name="task-retirement-working"></a>

If the task is part of a service, then the task is automatically stopped, The service scheduler starts a new one to replace it after it reaches its scheduled retirement date\. If you would like to update your service tasks before the retirement date, you can use the following steps\. For more information, see [Updating a service](update-service.md)\.

**To update a running service \(AWS Management Console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar, select the Region that your cluster is in\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select the name of the cluster in which your service resides\.

1. On the **Cluster: *name*** page, choose **Services**\.

1. Check the box to the left of the service to update and choose **Update**\.

1. On the **Configure service** page, your service information is pre\-populated\. Select **Force new deployment** and choose **Next step**\.
**Note**  
For tasks using the Fargate launch type, forcing a new deployment launches new tasks using the patched platform version\. Your tasks do not require you select a different platform version\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.

1. On the **Configure network** and **Set Auto Scaling \(optional\)** pages, choose **Next step**\.

1. Choose **Update Service** to finish and update your service\.

**To update a running service \(AWS CLI\)**

1. Obtain the ARN for the service\.

   ```
   aws ecs list-services --cluster cluster_name --region region
   ```

   Output:

   ```
   {
       "serviceArns": [
           "arn:aws:ecs:region:aws_account_id:service/MyService"
       ]
   }
   ```

1. Update your service, forcing a new deployment that deploys new tasks\.

   ```
   aws ecs update-service --service serviceArn --force-new-deployment --cluster cluster_name --region region
   ```

If you are using standalone tasks, then you can start a new task to replace it\. For more information, see [Running tasks](ecs_run_task.md)\.