# Tutorial: Using cluster auto scaling with the AWS Management Console<a name="tutorial-cluster-auto-scaling-console"></a>

Amazon ECS cluster auto scaling can be set up and configured using the AWS Management Console, AWS CLI, or Amazon ECS API\.

This tutorial walks you through creating the resources for cluster auto scaling using the AWS Management Console\. Where resources require a name, we will use the prefix `ConsoleTutorial` to ensure they all have unique names and to make them easy to locate\.

For an AWS CLI tutorial, see [Tutorial: Using cluster auto scaling with the AWS CLI](tutorial-cluster-auto-scaling-cli.md)\.

**Topics**
+ [Prerequisites](#console-tutorial-prereqs)
+ [Step 1: Create an Amazon ECS cluster](#console-tutorial-cluster)
+ [Step 2: Create the Auto Scaling Resources](#console-tutorial-asg)
+ [Step 3: Create a Capacity Provider](#console-tutorial-capacity-provider)
+ [Step 4: Set a Default Capacity Provider Strategy for the Cluster](#console-tutorial-default-strategy)
+ [Step 5: Register a Task Definition](#console-tutorial-register-task-definition)
+ [Step 6: Run a Task](#console-tutorial-run-task)
+ [Step 7: Verify](#console-tutorial-verify)
+ [Step 8: Clean Up](#console-tutorial-cleanup)

## Prerequisites<a name="console-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ The Amazon ECS container instance IAM role is created\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.
+ The Amazon ECS service\-linked IAM role is created\. For more information, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\.
+ The Auto Scaling service\-linked IAM role is created\. For more information, see [Service\-Linked Roles for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

## Step 1: Create an Amazon ECS cluster<a name="console-tutorial-cluster"></a>

Use the following steps to create an Amazon ECS cluster\. This tutorial uses an empty cluster so that we can manually create the Auto Scaling resources\. When you use the AWS Management Console to create a non\-empty cluster, Amazon ECS creates an AWS CloudFormation stack along with Auto Scaling resources\. We want to avoid creating this AWS CloudFormation stack when using the cluster auto scaling feature\.

**To create an empty cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, choose **Create Cluster**\.

1. For **Select cluster compatibility**, choose **EC2 Linux \+ Networking** and then choose **Next step**\.

1. For **Cluster name**, enter `ConsoleTutorial-cluster` for the cluster name\.

1. Select **Create an empty cluster** and then choose **Create**\.

## Step 2: Create the Auto Scaling Resources<a name="console-tutorial-asg"></a>

**Note**  
Use the old version of the EC2 Console for the auto scaling group sections of this tutorial\.

Use the following steps to create an Auto Scaling launch configuration and Auto Scaling group\.

**To create an Auto Scaling launch configuration**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, under **Auto Scaling**, choose **Launch Configurations**\.

1. On the next page, choose **Create launch configuration**\.

1. On the **Choose AMI** page, search for and choose the latest Amazon ECS\-optimized Amazon Linux 2 AMI in the `us-west-2` Region\. The AMI ID can be retrieved using the following link: [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/%252Faws%252Fservice%252Fecs%252Foptimized-ami%252Famazon-linux-2%252Frecommended%252Fimage_id/description?region=us-west-2#)\.

1. On the **Choose Instance Type** page, select `t2.micro`, then choose **Next: Configure details**\.

1. On the **Configure details** page, do the following:

   1. For **Name**, enter `ConsoleTutorial-ASGlaunchconfig` for the launch configuration name\.

   1. For **IAM role**, select your container instance IAM role\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.

   1. Expand the **Advanced Details** section to specify user data for your Amazon ECS container instances\.

      Paste the following script into the **User data** field\. The `ConsoleTutorial-cluster` cluster was created in the first step\.

      ```
      #!/bin/bash
      echo ECS_CLUSTER=ConsoleTutorial-cluster >> /etc/ecs/ecs.config
      ```

   1. Choose **Skip to review**\.

1. Choose **Create launch configuration**\.

Next, create an Auto Scaling group using that launch configuration\.

**To create an Auto Scaling group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, under **Auto Scaling**, choose **Launch Configurations**\.

1. On the next page, select the launch configuration we created in step 1 and choose **Create Auto Scaling group**\.

1. On the **Configure Auto Scaling group details** page, do the following:

   1. For **Group name**, enter `ConsoleTutorial-ASG` for the Auto Scaling group name\.

   1. For **Group size**, enter `0`\. The tutorial uses Amazon ECS managed scaling so there is no need to have the Auto Scaling group launch any initial instances\.

   1. For **Network**, choose a VPC for your Auto Scaling group\.

   1. For **Subnet**, choose a subnet in your VPC\.

   1. Expand the **Advanced Details** section\. For **Instance Protection**, choose **Protect From Scale In**\. This enables you to use managed termination protection for the instances in the Auto Scaling group, which prevents your container instances that contain tasks from being terminated during scale\-in actions\.

1. Choose **Next: Configure scaling policies**\.

1. On the **Configure scaling policies** page, select **Keep this group at its initial size**\. The tutorial uses Amazon ECS managed scaling so there is no need to create a scaling policy\.

1. Choose **Review**, **Create Auto Scaling group**\.

1. Repeat steps 3 to 8 to create a second Auto Scaling group but for **Group name** use `ConsoleTutorial-ASG-burst`\.

1. Use the following steps to edit the max capacity value for each of your Auto Scaling groups\.

   1. Choose **View your Auto Scaling groups**\.

   1. Select your `ConsoleTutorial-ASG` scaling group\. From the **Details** tab, choose **Edit**\.

   1. For **Max**, enter `100`, then choose **Save**\.

1. Repeat step 10 for your `ConsoleTutorial-ASG-burst` scaling group\.

## Step 3: Create a Capacity Provider<a name="console-tutorial-capacity-provider"></a>

Use the following steps to create an Amazon ECS capacity provider\. See [Amazon ECS capacity providers](cluster-capacity-providers.md) for more information\.

**To create a capacity provider**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **Capacity Providers** tab, choose **Create**\.

1. On the **Create Capacity Providers** window, do the following\.

   1. For **Capacity provider name**, enter `ConsoleTutorial-capacityprovider` for the name\.

   1. For **Auto Scaling group**, select the `ConsoleTutorial-ASG` Auto Scaling group you created\.

   1. For **Managed scaling**, choose **Enabled**\. This enables Amazon ECS to manage the scale\-in and scale\-out actions for the capacity provider\.

   1. For **Target capacity %**, enter `100`\.

   1. For **Managed termination protection**, choose **Enabled**\. This prevents your container instances that contain tasks and that are in the Auto Scaling group from being terminated during a scale\-in action\.

   1. Choose **Create**\.
**Important**  
If you receive an error during this step, try logging out and back in to the console\. If the error does not clear, we recommend using the AWS CLI tutorial\. For more information, see [Tutorial: Using cluster auto scaling with the AWS CLI](tutorial-cluster-auto-scaling-cli.md)\.

   1. Choose **View in cluster** to see your new capacity provider\.

   1. Repeat steps 4 to 6, creating a second capacity provider with name `ConsoleTutorial-capacityprovider-burst` with your `ConsoleTutorial-ASG-burst` Auto Scaling group\.

## Step 4: Set a Default Capacity Provider Strategy for the Cluster<a name="console-tutorial-default-strategy"></a>

When running a task or creating a service, the Amazon ECS console uses the default capacity provider strategy for the cluster\. The default capacity provider strategy can be defined by updating the cluster\.

**To define a default capacity provider strategy**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **Cluster : ConsoleTutorial\-cluster** page, choose **Update Cluster**\.

1. For **Default capacity provider strategy** choose, **Add provider**\.

1. Select your `ConsoleTutorial-capacityprovider` capacity provider\.

1. Choose **Add provider**, select your `ConsoleTutorial-capacityprovider-burst` capacity provider\.

1. For **Provider 1**, leave the **Base** value at `0` and leave the **Weight** value at `1`\.

1. Choose **Update**\. This will add the capacity providers to the default capacity provider strategy for the cluster\.

1. Choose **View cluster**\.

## Step 5: Register a Task Definition<a name="console-tutorial-register-task-definition"></a>

Before you can run a task on your cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example is a simple task definition that uses an `amazonlinux` image from Docker Hub and simply sleeps\. For more information about the available task definition parameters, see [Amazon ECS Task definitions](task_definitions.md)\.

**To register a task definition**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Task Definitions**, **Create new Task Definition**\.

1. On the **Create new Task Definition** page, select **EC2**, **Next step**\.

1. Choose **Configure via JSON** and copy and paste the following contents and then choose **Save**, **Create**\.

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

## Step 6: Run a Task<a name="console-tutorial-run-task"></a>

After you have registered a task definition for your account, you can run a task in the cluster\. For this tutorial, you run five instances of the `ConsoleTutorial-taskdef` task definition in your `ConsoleTutorial-cluster` cluster\.

**To run a task**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Task Definitions**\.

1. Select your **ConsoleTutorial\-taskdef** task definition\.

1. From the **Actions** menu, choose **Run Task**\.

1. Use the following steps to complete the run task workflow\.

   1. For **Capacity provider strategy**, the default capacity provider strategy for the cluster must be selected\.

   1. For **Cluster**, select your **ConsoleTutorial\-cluster** cluster\.

   1. For **Number of tasks**, enter `5`\.

   1. For **Placement Templates**, choose **BinPack**\.

   1. Choose **Run Task**\.

## Step 7: Verify<a name="console-tutorial-verify"></a>

At this point in the tutorial, you should have two Auto Scaling groups with one capacity provider for each of them\. The capacity providers have Amazon ECS managed scaling enabled\. A cluster was created and five tasks are running\.

We can verify that everything is working properly by viewing the CloudWatch metrics, the Auto Scaling group settings, and finally the Amazon ECS cluster task count\.

**To view the CloudWatch metrics for your cluster**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab, choose **AWS/ECS/ManagedScaling**\.

1. Choose **CapacityProviderName, ClusterName**\.

1. Choose the metric that corresponds to the **ConsoleTutorial\-capacityprovider** capacity provider\.

1. On the **Graphed metrics** tab, change **Period** to **30 seconds** and **Statistic** to **Maximum**\.

   The value displayed in the graph shows the target capacity value for the capacity provider\. It should begin at `100`, which was the target capacity percent we set\. You should see it scale up to `200`, which will trigger an alarm for the target tracking scaling policy\. The alarm will then trigger the Auto Scaling group to scale out\.  
![\[Capacity provider metrics view\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/capacity-provider-scaling.png)

1. Steps 5 to 6 can be repeated for your **ConsoleTutorial\-capacityprovider\-burst** metric\.

Use the following steps to view your Auto Scaling group details to confirm that the scale\-out action occurred\.

**To verify the Auto Scaling group scaled out**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

1. For each of your Auto Scaling groups, view the values in the **Instances** and **Desired** columns to confirm your group scaled out to two instances for each group\.

Use the following steps to view your Amazon ECS cluster to confirm that the Amazon EC2 instances were registered with the cluster and your tasks transitioned to a `RUNNING` status\.

**To verify the Auto Scaling group scaled out**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **ECS Instances** tab, confirm you see four instances registered, which matches your Auto Scaling group values\.

1. On the **Tasks** tab, confirm you see five tasks in `RUNNING` status\.

## Step 8: Clean Up<a name="console-tutorial-cleanup"></a>

When you have finished this tutorial, clean up the resources associated with it to avoid incurring charges for resources that you aren't using\. Deleting capacity providers and task definitions are not supported, but there is no cost associated with these resources\.

**To clean up the tutorial resources**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. From the **Tasks** tab, choose **Stop All**\. Enter the verification and choose **Stop all** again\.

1. Delete the Auto Scaling groups using the following steps\.

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

   1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**\.

   1. Select your **ConsoleTutorial\-ASG** Auto Scaling group, then from the **Actions** menu choose **Delete\.**

   1. Select your **ConsoleTutorial\-ASG\-burst** Auto Scaling group, then from the **Actions** menu choose **Delete\.**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. Choose **Delete Cluster**, enter the confirmation phrase, and choose **Delete**\.