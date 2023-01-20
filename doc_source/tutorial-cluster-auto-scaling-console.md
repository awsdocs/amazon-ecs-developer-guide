# Tutorial: Using cluster auto scaling with the AWS Management Console and the Amazon ECS console<a name="tutorial-cluster-auto-scaling-console"></a>

This tutorial walks you through creating the resources for cluster auto scaling using the AWS Management Console and the classic Amazon ECS console\. Where resources require a name, we will use the prefix `ConsoleTutorial` to ensure they all have unique names and to make them easy to locate\.

**Topics**
+ [Prerequisites](#console-tutorial-prereqs)
+ [Step 1: Create an Amazon ECS cluster](#console-tutorial-cluster)
+ [Step 2: Register a task definition](#console-tutorial-register-task-definition)
+ [Step 3: Run a task](#console-tutorial-run-task)
+ [Step 4: Verify](#console-tutorial-verify)
+ [Step 5: Clean up](#console-tutorial-cleanup)

## Prerequisites<a name="console-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ The Amazon ECS container instance IAM role is created\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.
+ The Amazon ECS service\-linked IAM role is created\. For more information, see [Using service\-linked roles for Amazon ECS](using-service-linked-roles.md)\.
+ The Auto Scaling service\-linked IAM role is created\. For more information, see [Service\-Linked Roles for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ You have a VPC and security group created to use\. For more information, see [Create a virtual private cloud](get-set-up-for-amazon-ecs.md#create-a-vpc)\.

## Step 1: Create an Amazon ECS cluster<a name="console-tutorial-cluster"></a>

Use the following steps to create an Amazon ECS cluster\. 

Amazon ECS creates an Amazon EC2 Auto Scaling launch template and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. 

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. From the navigation bar, select the Region to use\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create cluster**\.

1. Under **Cluster configuration**, for **Cluster name**, enter `ConsoleTutorial-cluster`\.

1. Add Amazon EC2 instances to your cluster, expand **Infrastructure**, and then select **Amazon EC2 instances**\. Next, configure the Auto Scaling group which acts as the capacity provider\.

   1. Create a Auto Scaling group, from Auto Scaling group \(ASG\)\. Celect **Create new group**, and then provide the following details about the group:
     + For **Operating system/Architecture**, choose **Amazon Linux 2**\.
     + For **EC2 instance type**, choose **t3\.nano**\.
     + For **Capacity**, enter **0** for the minimum number and the maximum number of instances to launch in the Auto Scaling group\. 

1. \(Optional\) To manage the cluster tags, expand **Tags**, and then perform one of the following operations:

   \[Add a tag\] Choose **Add tag** and do the following:
   + For **Key**, enter the key name\.
   + For **Value**, enter the key value\.

   \[Remove a tag\] Choose **Remove** to the right of the tag’s Key and Value\.

1. Choose **Create**\.

## Step 2: Register a task definition<a name="console-tutorial-register-task-definition"></a>

Before you can run a task on your cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example is a simple task definition that uses an `amazonlinux` image from Docker Hub and simply sleeps\. For more information about the available task definition parameters, see [Amazon ECS task definitions](task_definitions.md)\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**\.

1. Choose **Create new task definition**, **Create new task definition with JSON**\.

1. In the **JSON editor** box, copy and paste the following contents\.

   ```
   {
       "family": "ConsoleTutorial-taskdef",
       "containerDefinitions": [
           {
               "name": "sleep",
               "image": "amazonlinux:2",
               "memory": 20,
               "essential": true,
               "command": [
                   "sh",
                   "-c",
                   "sleep infinity"
               ]
           }
       ],
       "requiresCompatibilities": [
           "EC2"
       ]
   }
   ```

1. Choose **Create**\.

## Step 3: Run a task<a name="console-tutorial-run-task"></a>

After you have registered a task definition for your account, you can run a task in the cluster\. For this tutorial, you run five instances of the `ConsoleTutorial-taskdef` task definition in your `ConsoleTutorial-cluster` cluster\.

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the **Clusters** page, choose **ConsoleTutorial\-cluster**\.

1. From the **Tasks** tab, choose **Create**\.

1. For **Launch type**, choose **EC2**\.

1. For **Application type**, choose **Task**\.

1. For **Task definition**, choose **ConsoleTutorial\-taskdef**\.

1. Choose **Create**\.

## Step 4: Verify<a name="console-tutorial-verify"></a>

At this point in the tutorial, you should have two Auto Scaling groups with one capacity provider for each of them\. The capacity providers have Amazon ECS managed scaling enabled\. A cluster was created and five tasks are running\.

We can verify that everything is working properly by viewing the CloudWatch metrics, the Auto Scaling group settings, and finally the Amazon ECS cluster task count\.

**To view the CloudWatch metrics for your cluster**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation bar at the top of the screen, select the Region\.

1. On the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab, choose `AWS/ECS/ManagedScaling`\.

1. Choose **CapacityProviderName, ClusterName**\.

1. Choose the metric that corresponds to the **ConsoleTutorial\-capacityprovider** capacity provider\.

1. On the **Graphed metrics** tab, change **Period** to **30 seconds** and **Statistic** to **Maximum**\.

   The value displayed in the graph shows the target capacity value for the capacity provider\. It should begin at `100`, which was the target capacity percent we set\. You should see it scale up to `200`, which will trigger an alarm for the target tracking scaling policy\. The alarm will then trigger the Auto Scaling group to scale out\.

1. Steps 5 to 6 can be repeated for your **ConsoleTutorial\-capacityprovider\-burst** metric\.

Use the following steps to view your Auto Scaling group details to confirm that the scale\-out action occurred\.

**To verify the Auto Scaling group scaled out**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the Region\.

1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

1. For each of your Auto Scaling groups, view the values in the **Instances** and **Desired** columns to confirm your group scaled out to two instances for each group\.

Use the following steps to view your Amazon ECS cluster to confirm that the Amazon EC2 instances were registered with the cluster and your tasks transitioned to a `RUNNING` status\.

**To verify the instances in the Auto Scaling group**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. On the navigation bar at the top of the screen, select the Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **Tasks** tab, confirm you see five tasks in `RUNNING` status\.

## Step 5: Clean up<a name="console-tutorial-cleanup"></a>

When you have finished this tutorial, clean up the resources associated with it to avoid incurring charges for resources that you aren't using\. Deleting capacity providers and task definitions are not supported, but there is no cost associated with these resources\.

**To clean up the tutorial resources**

1. Open the console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose the cluster\.

1. On the **Cluster : ConsoleTutorial\-cluster** page, choose the **Tasks** tab\. 

   Choose **Stop**, **Stop all**\.

1. On the **Stop confirmation page**, enter **Stop**, and then choose **Stop**\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select **ConsoleTutorial\-cluster**\.

1. In the upper\-right of the page, choose **Delete Cluster**\. 

   

1. In the confirmation box, enter **delete **ConsoleTutorial\-cluster****\.

1. Delete the Auto Scaling groups using the following steps\.

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. On the navigation bar at the top of the screen, select the Region\.

   1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

   1. Select your **ConsoleTutorial\-ASG** Auto Scaling group, then from the **Actions** menu choose **Delete\.**

   1. Select your **ConsoleTutorial\-ASG\-burst** Auto Scaling group, then from the **Actions** menu choose **Delete\.**