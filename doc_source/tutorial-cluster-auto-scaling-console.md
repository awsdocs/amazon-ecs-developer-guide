# Tutorial: Using cluster auto scaling with the AWS Management Console<a name="tutorial-cluster-auto-scaling-console"></a>

This tutorial walks you through creating the resources for cluster auto scaling using the AWS Management Console\. Where resources require a name, we will use the prefix `ConsoleTutorial` to ensure they all have unique names and to make them easy to locate\.

**Topics**
+ [Prerequisites](#console-tutorial-prereqs)
+ [Step 1: Create an Amazon ECS cluster](#console-tutorial-cluster)
+ [Step 2: Create the Auto Scaling resources](#console-tutorial-asg)
+ [Step 3: Create a capacity provider](#console-tutorial-capacity-provider)
+ [Step 4: Set a default capacity provider strategy for the cluster](#console-tutorial-default-strategy)
+ [Step 5: Register a task definition](#console-tutorial-register-task-definition)
+ [Step 6: Run a task](#console-tutorial-run-task)
+ [Step 7: Verify](#console-tutorial-verify)
+ [Step 8: Clean up](#console-tutorial-cleanup)

## Prerequisites<a name="console-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS first\-run wizard permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ The Amazon ECS container instance IAM role is created\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.
+ The Amazon ECS service\-linked IAM role is created\. For more information, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.
+ The Auto Scaling service\-linked IAM role is created\. For more information, see [Service\-Linked Roles for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ You have a VPC and security group created to use\. For more information, see [Create a virtual private cloud](get-set-up-for-amazon-ecs.md#create-a-vpc)\.

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

## Step 2: Create the Auto Scaling resources<a name="console-tutorial-asg"></a>

Use the following steps to create an Amazon EC2 launch template and Auto Scaling group\.

**To create an Amazon EC2 launch template**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, under **Instances**, choose **Launch Templates**\.

1. On the next page, choose **Create launch template**\.

1. On the **Create launch template** page, complete the following steps\.

   1. For **Launch template name**, enter `ConsoleTutorial-LaunchTemplate`\.

   1. For **Template version description**, provide a description for the launch template\.

   1. For **Amazon Machine Image \(AMI\)**, search for the latest Amazon ECS\-optimized AMI\. The AMI ID can be retrieved using the following link: [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/%252Faws%252Fservice%252Fecs%252Foptimized-ami%252Famazon-linux-2%252Frecommended%252Fimage_id/description?region=us-west-2#)\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

   1. For **Instance type**, select an instance type\. For the purposes of this tutorial, the `t2.micro` instance type works\.

   1. For **Security groups**, select one or more security groups\.

   1. Expand the **Advanced Details** section to specify the IAM instance profile and user data for your Amazon ECS container instances\.

      1. For **IAM instance profile**, select your container instance IAM role\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

      1. For **User data**, paste the following script into the field\. The `ConsoleTutorial-cluster` cluster was created in the first step\.

         ```
         #!/bin/bash
         echo ECS_CLUSTER=ConsoleTutorial-cluster >> /etc/ecs/ecs.config
         ```

1. Choose **Create launch template**\.

Next, create an Auto Scaling group using that launch template\.

**To create an Auto Scaling group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. On the navigation pane, under **Auto Scaling**, choose **Auto Scaling Groups**, **Create Auto Scaling group**\.

1. On the **Create Auto Scaling group** page, complete the following steps\.

   1. For **Auto Scaling group name**, enter `ConsoleTutorial-ASG` for the Auto Scaling group name\.

   1. For **Launch template**, select the `ConsoleTutorial-LaunchTemplate` launch template\.

   1. Review the contents of the launch template, then choose **Next**\.

   1. On the **Configure settings** step, under **Network**, select a **VPC** and one or more **Subnets** to use and then choose **Next**\.

   1. On the **Configure advanced options** step, choose **Next**\.

   1. On the **Configure group size and scaling policies** step, for **Desired capacity**, enter `0`\. This tutorial uses Amazon ECS managed scaling so there is no need to have the Auto Scaling group launch any initial instances\. For **Minimum capacity**, enter `0`\. For **Maximum capacity**, enter `2`\.

   1. For **Instance scale\-in protection**, select **Enable instance scale\-in protection** and then choose **Next**\. This enables you to use managed termination protection for the instances in the Auto Scaling group, which prevents your container instances that contain tasks from being terminated during scale\-in actions\.

   1. On the **Add notifications** and **Add tags** steps, choose **Next**\.

   1. On the **Review** step, review the Auto Scaling group settings and then choose **Create Auto Scaling group**\.

1. Repeat steps 3 and 4 to create a second Auto Scaling group\. For **Auto Scaling group name** use `ConsoleTutorial-ASG-burst` and when setting the group size, set the **Desired capacity** and **Minimum capacity** to `0` and the Maximum capacity to `20`\.

## Step 3: Create a capacity provider<a name="console-tutorial-capacity-provider"></a>

Use the following steps to create an Amazon ECS capacity provider\. See [ Amazon ECS capacity providersCapacity providers  Amazon ECS capacity providers are used to manage the infrastructure the tasks in your clusters use\. Each cluster can have one or more capacity providers and an optional default capacity provider strategy\. The capacity provider strategy determines how the tasks are spread across the cluster's capacity providers\. When you run a standalone task or create a service, you may either use the cluster's default capacity provider strategy or specify a capacity provider strategy that overrides the cluster's default strategy\.  Capacity provider concepts  Capacity providers consist of the following components\.  

Capacity provider  
A *capacity provider* is associated with a cluster and is used in a capacity provider strategy to determine the infrastructure that a task runs on\.  
For Amazon ECS on AWS Fargate users, there is a `FARGATE` and a `FARGATE_SPOT` capacity provider\. The AWS Fargate capacity providers are reserved and don't need to be created nor can they be deleted\. After you associate them with your cluster, you may add them to a capacity provider strategy\. For more information, see [AWS Fargate capacity providers](fargate-capacity-providers.md)\.  
For Amazon ECS on Amazon EC2 users, a capacity provider consists of a capacity provider name, an Auto Scaling group, and the settings for managed scaling and managed termination protection\. With managed scaling, Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group which provides auto scaling for your cluster's infrastructure\. For more information, see [Auto Scaling group capacity providers](asg-capacity-providers.md)\. 

Default capacity provider strategy  
A *default capacity provider strategy* is associated with an Amazon ECS cluster\. This determines the capacity provider strategy used creating a service or running a standalone task in the cluster when there isn't a custom capacity provider strategy or launch type specified\. It is considered best practice to define a default capacity provider strategy for each cluster\. 

Capacity provider strategy  
A *capacity provider strategy* is specified when creating a service or running a standalone task when the default capacity provider strategy for a cluster does not meet your needs\.  
Only capacity providers that are already associated with a cluster and have an `ACTIVE` or `UPDATING` status can be used in a capacity provider strategy\. A capacity provider can be associated with a cluster either during cluster creation or by using the PutClusterCapacityProviders API after a cluster has been created\.  
A capacity provider strategy consists of one or more capacity providers\. An optional *base* and *weight* value may be specified for finer control of a capacity provider\.  
The *base* value designates how many tasks, at a minimum, to run on the specified capacity provider\. Only one capacity provider in a capacity provider strategy can have a base defined\.  
The *weight* value designates the relative percentage of the total number of launched tasks that should use the specified capacity provider\. For example, if you have a strategy that contains two capacity providers, and both have a weight of `1`, then when the base is satisfied, the tasks will be split evenly across the two capacity providers\. Using that same logic, if you specify a weight of `1` for *capacityProviderA* and a weight of `4` for *capacityProviderB*, then for every one task that is run using *capacityProviderA*, four tasks would use *capacityProviderB*\.    Capacity provider types  The infrastructure your Amazon ECS workloads are run on determines the type of capacity provider you can use\. For Amazon ECS workloads hosted on Fargate, the following predefined capacity providers are available:   Fargate   Fargate Spot   For Amazon ECS workloads hosted on Amazon EC2 instances, you must create and maintain a capacity provider that consists of the following components:   A name   An Auto Scaling group   The settings for managed scaling and managed termination protection\.     Capacity provider considerations  The following should be considered when using capacity providers:   A capacity provider must be associated with a cluster prior to being specified in a capacity provider strategy\.   When you specify a capacity provider strategy, the number of capacity providers that can be specified is limited to six\.   A service using an Auto Scaling group capacity provider can't be updated to use a Fargate capacity provider and vice versa\.   In a capacity provider strategy, if no `weight` value is specified for a capacity provider in the console then the default value of `1` is used\. If using the API or AWS CLI, the default value of `0` is used\.   When multiple capacity providers are specified within a capacity provider strategy, at least one of the capacity providers must have a weight value greater than zero and any capacity providers with a weight of `0` will not be used to place tasks\. If you specify multiple capacity providers in a strategy that all have a weight of `0`, any `RunTask` or `CreateService` actions using the capacity provider strategy will fail\.   In a capacity provider strategy, only one capacity provider can have a *base* value defined\. If no base value is specified, the default value of `0` is used\.   A cluster may contain a mix of both Auto Scaling group capacity providers and Fargate capacity providers, however a capacity provider strategy may only contain one or the other but not both\.   A cluster may contain a mix of services and standalone tasks using both capacity providers and launch types\. A service may be updated to use a capacity provider strategy rather than a launch type, however you must force a new deployment when doing so\.   When you use managed termination protection, you must also use managed scaling otherwise managed termination protection won't work\.    Using capacity providers is not supported when using Classic Load Balancers for your services\.     AWS Fargate capacity providers  Amazon ECS on AWS Fargate capacity providers allow you to use both Fargate and Fargate Spot capacity with your Amazon ECS tasks\. For more information about capacity providers, see [Amazon ECS capacity providers](cluster-capacity-providers.md)\. With Fargate Spot you can run interruption tolerant Amazon ECS tasks at a discounted rate compared to the Fargate price\. Fargate Spot runs tasks on spare compute capacity\. When AWS needs the capacity back, your tasks will be interrupted with a two\-minute warning\. This is described in further detail below\.  Fargate capacity provider considerations  The following should be considered when using Fargate capacity providers\.   The Fargate Spot capacity provider is not supported for Windows containers on Fargate\.   The Fargate Spot capacity provider is not supported for Linux tasks with the ARM64 architecture, Fargate Spot only supports Linux tasks with the X86\_64 architecture\.   The Fargate and Fargate Spot capacity providers don't need to be created\. They are available to all accounts and only need to be associated with a cluster to be available for use\.   To associate Fargate and Fargate Spot capacity providers to an existing cluster, you must use the Amazon ECS API or AWS CLI\. For more information, see [Adding Fargate capacity providers to an existing cluster](#fargate-capacity-providers-existing-cluster)\.   The Fargate and Fargate Spot capacity providers are reserved and cannot be deleted\. You can disassociate them from a cluster using the `PutClusterCapacityProviders` API\.   When a new cluster is created using the Amazon ECS classic console along with the **Networking only** cluster template, the `FARGATE` and `FARGATE_SPOT` capacity providers are associated with the new cluster automatically\.   Using Fargate Spot requires that your task use platform version 1\.3\.0 or later \(for Linux\)\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.   When tasks using the Fargate and Fargate Spot capacity providers are stopped, a task state change event is sent to Amazon EventBridge\. The stopped reason describes the cause\. For more information, see [Task state change events](ecs_cwe_events.md#ecs_task_events)\.   A cluster may contain a mix of Fargate and Auto Scaling group capacity providers, however a capacity provider strategy may only contain either Fargate or Auto Scaling group capacity providers, but not both\. For more information, see [Auto Scaling Group Capacity Providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-auto-scaling.html#asg-capacity-providers) in the *Amazon Elastic Container Service Developer Guide*\.     Handling Fargate Spot termination notices  When tasks using Fargate Spot capacity are stopped due to a Spot interruption, a two\-minute warning is sent before a task is stopped\. The warning is sent as a task state change event to Amazon EventBridge and a SIGTERM signal to the running task\. When using Fargate Spot as part of a service, the service scheduler will receive the interruption signal and attempt to launch additional tasks on Fargate Spot if capacity is available\. A service with only one task will be interrupted until capacity is available\. To ensure that your containers exit gracefully before the task stops, the following can be configured:   A `stopTimeout` value of `120` seconds or less can be specified in the container definition that the task is using\. Specifying a `stopTimeout` value gives you time between the moment the task state change event is received and the point at which the container is forcefully stopped\. If you don't specify a `stopTimeout` value, the default value of 30 seconds is used\. For more information, see [Container timeouts](task_definition_parameters.md#container_definition_timeout)\.   The SIGTERM signal must be received from within the container to perform any cleanup actions\. Failure to process this signal will result in the task receiving a SIGKILL signal after the configured `stopTimeout` and may result in data loss or corruption\.   The following is a snippet of a task state change event displaying the stopped reason and stop code for a Fargate Spot interruption\. 

```
{
  "version": "0",
  "id": "9bcdac79-b31f-4d3d-9410-fbd727c29fab",
  "detail-type": "ECS Task State Change",
  "source": "aws.ecs",
  "account": "111122223333",
  "resources": [
    "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6f1cebef"
  ],
  "detail": {
    "clusterArn": "arn:aws:ecs:us-east-1:111122223333:cluster/default",
    "createdAt": "2016-12-06T16:41:05.702Z",
    "desiredStatus": "STOPPED",
    "lastStatus": "RUNNING",
    "stoppedReason": "Your Spot Task was interrupted.",
    "stopCode": "TerminationNotice",
    "taskArn": "arn:aws:ecs:us-east-1:111122223333:task/b99d40b3-5176-4f71-9a52-9dbd6fEXAMPLE",
    ...
  }
}
``` The following is an event pattern that is used to create an EventBridge rule for Amazon ECS task state change events\. You can optionally specify a cluster in the `detail` field to receive task state change events for\. For more information, see [Creating an EventBridge Rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\. 

```
{
    "source": [
        "aws.ecs"
    ],
    "detail-type": [
        "ECS Task State Change"
    ],
    "detail": {
        "clusterArn": [
            "arn:aws:ecs:us-west-2:111122223333:cluster/default"
        ]
    }
}
```   Creating a new cluster that uses Fargate capacity providers  When a new Amazon ECS cluster is created, you can specify one or more capacity providers to associate with the cluster\. The capacity providers are used to define a capacity provider strategy which determine the infrastructure your tasks run on\. When using the AWS Management Console, the `FARGATE` and `FARGATE_SPOT` capacity providers are associated with the cluster automatically when using the **Networking only** cluster template\. For more information, see [Creating a cluster using the classic console](create_cluster.md)\.  To create an Amazon ECS cluster using Fargate capacity providers \(AWS CLI\)  Use the following command to create a new cluster and associate both the Fargate and Fargate Spot capacity providers with it\.   [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\) 

  ```
  aws ecs create-cluster \
       --cluster-name FargateCluster \
       --capacity-providers FARGATE FARGATE_SPOT \
       --region us-west-2
  ```      Adding Fargate capacity providers to an existing cluster  You can update the pool of available capacity providers for an existing Amazon ECS cluster by using the `PutClusterCapacityProviders` API\. Adding either the Fargate or Fargate Spot capacity providers to an existing cluster is not supported in the AWS Management Console\. You must either create a new Fargate cluster in the console or add the Fargate or Fargate Spot capacity providers to the existing cluster using the Amazon ECS API or AWS CLI\.  To add the Fargate capacity providers to an existing cluster \(AWS CLI\)  Use the following command to add the Fargate and Fargate Spot capacity providers to an existing cluster\. If the specified cluster has existing capacity providers associated with it, you must specify all existing capacity providers in addition to any new ones you want to add\. Any existing capacity providers associated with a cluster that are omitted from a `PutClusterCapacityProviders` API call will be disassociated from the cluster\. You can only disassociate an existing capacity provider from a cluster if it's not being used by any existing tasks\. These same rules apply to the cluster's default capacity provider strategy\. If the cluster has an existing default capacity provider strategy defined, it must be included in the `PutClusterCapacityProviders` API call\. Otherwise, it will be overwritten\.   [put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html) \(AWS CLI\) 

  ```
  aws ecs put-cluster-capacity-providers \
       --cluster FargateCluster \
       --capacity-providers FARGATE FARGATE_SPOT existing_capacity_provider1 existing_capacity_provider2 \
       --default-capacity-provider-strategy existing_default_capacity_provider_strategy \
       --region us-west-2
  ```      Running tasks using a Fargate capacity provider  You can run a task or create a service using either the Fargate or Fargate Spot capacity providers by specifying a capacity provider strategy\. If no capacity provider strategy is provided, the cluster's default capacity provider strategy is used\. Running a task using the Fargate or Fargate Spot capacity providers is supported in the AWS Management Console\. You must add the Fargate or Fargate Spot capacity providers to cluster's default capacity provider strategy if using the AWS Management Console\. When using the Amazon ECS API or AWS CLI you can specify either a capacity provider strategy or use the cluster's default capacity provider strategy\.  To run a task using a Fargate capacity provider \(AWS CLI\)  Use the following command to run a task using the Fargate and Fargate Spot capacity providers\.   [run\-task](https://docs.aws.amazon.com/cli/latest/reference/ecs/run-task.html) \(AWS CLI\) 

  ```
  aws ecs run-task \
       --capacity-provider-strategy capacityProvider=FARGATE,weight=1 capacityProvider=FARGATE_SPOT,weight=1 \
       --cluster FargateCluster \
       --task-definition task-def-family:revision \
       --network-configuration "awsvpcConfiguration={subnets=[string,string],securityGroups=[string,string],assignPublicIp=string}" \
       --count integer \
       --region us-west-2
  ```    When running standalone tasks using Fargate Spot it is important to note that the task may be interrupted before it is able to complete and exit\. It is therefore important that you code your application to gracefully exit within 2 minutes when it receives a SIGTERM signal and be able to be resumed\. For more information, see [Handling Fargate Spot termination notices](#fargate-capacity-providers-termination)\.    Create a service using a Fargate capacity provider \(AWS CLI\)  Use the following command to create a service using the Fargate and Fargate Spot capacity providers\.   [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html) \(AWS CLI\) 

  ```
  aws ecs create-service \
       --capacity-provider-strategy capacityProvider=FARGATE,weight=1 capacityProvider=FARGATE_SPOT,weight=1 \
       --cluster FargateCluster \
       --service-name FargateService \
       --task-definition task-def-family:revision \
       --network-configuration "awsvpcConfiguration={subnets=[string,string],securityGroups=[string,string],assignPublicIp=string}" \
       --desired-count integer \
       --region us-west-2
  ```       Auto Scaling group capacity providers  Amazon ECS capacity providers can use Auto Scaling groups to manage the Amazon EC2 instances registered to their clusters\. You can use the managed scaling feature to have Amazon ECS manage the scale\-in and scale\-out actions of the Auto Scaling group or you can manage the scaling actions yourself\. For more information, see [Amazon ECS cluster Auto Scaling](cluster-auto-scaling.md)\. Topics  Auto Scaling group capacity providers considerations  Consider the following when using Auto Scaling group capacity providers in the classic console:   We recommend that you create a new empty Auto Scaling group to use with a capacity provider rather than using an existing one\. If you use an existing Auto Scaling group, any Amazon EC2 instances associated with the group that were already running and registered to an Amazon ECS cluster prior to the Auto Scaling group being used to create a capacity provider may not be properly registered with the capacity provider\. This may cause issues when using the capacity provider in a capacity provider strategy\. The `DescribeContainerInstances` API can confirm whether a container instance is associated with a capacity provider or not\.  To create an empty Auto Scaling group, set the desired count to zero\. After you have created the capacity provider and associated it with a cluster, you can then scale it out\.    An Auto Scaling group must have a `MaxSize` greater than zero to scale out\.   If the Auto Scaling group is unable to scale out to accommodate the number of tasks run, the tasks will fail to transition beyond the `PROVISIONING` state\.   When you use managed termination protection, you must also use managed scaling otherwise managed termination protection won't work\.    When using managed scaling, the Auto Scaling group shouldn't have any scaling policies attached to it other than the ones Amazon ECS creates, otherwise the Amazon ECS created scaling plans will receive an `ActiveWithProblems` error\. For more information, see [Avoiding the ActiveWithProblems error](https://docs.aws.amazon.com/autoscaling/plans/userguide/gs-best-practices.html#gs-activewithproblems) in the *AWS Auto Scaling User Guide*\.   You can add a warm pool to your Auto Scaling group\. A warm pool is a group of pre\-initialized Amazon ECS instances that are ready to be included in the cluster whenever your application needs to scale out\. For more information about warm pools, see [Using a warm pool for your Auto Scaling group](asg-capacity-providers-create-auto-scaling-group.md#using-warm-pool)    The Auto Scaling group can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.     Creating an Auto Scaling group  When creating an Auto Scaling group, you use an Amazon EC2 launch template\. Amazon EC2 launch templates specify the Amazon EC2 instance configuration, including the AMI, the instance type, a key pair, security groups, and the other parameters that you use to launch Amazon EC2 instances\.  When using the Amazon ECS console **Create Cluster** wizard with the **EC2 Linux \+ Networking** option, Amazon ECS creates an Amazon EC2 Auto Scaling launch configuration and Auto Scaling group on your behalf as part of the AWS CloudFormation stack\. They are prefixed with `EC2ContainerService-<ClusterName>`, which makes them easy to identify\. That Auto Scaling group could then be used in a capacity provider for that cluster\. For more information on replacing an Auto Scaling launch configuration with an Amazon EC2 launch template, see [Replacing a launch configuration with a launch template](https://docs.aws.amazon.com/autoscaling/ec2/userguide/replace-launch-config.html) in the *Amazon EC2 Auto Scaling User Guide*  The following should be considered when creating an Auto Scaling group for a capacity provider\.   If managed termination protection is enabled when you create a capacity provider, the Auto Scaling group and each Amazon EC2 instance in the Auto Scaling group must have instance protection from scale in enabled as well\. For more information, see [Instance Protection](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html#instance-protection) in the *AWS Auto Scaling User Guide*\.   If managed scaling is enabled when you create a capacity provider, the Auto Scaling group desired count can be set to `0`\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group\.   The Auto Scaling group can't have instance weighting settings\. Instance weighting isn't supported when used with an Amazon ECS capacity provider\.   For more information on creating an Amazon EC2 Auto Scaling launch template, see [Launch Templates](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchTemplates.html) in the *Amazon EC2 Auto Scaling User Guide*\. For more information on creating an Amazon EC2 Auto Scaling group, see [Auto Scaling groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.  Using a warm pool for your Auto Scaling groupUsing a warm pool  Amazon ECS supports Amazon EC2 Auto Scaling warm pools\. A warm pool is a group of pre\-initialized Amazon EC2 instances ready to be placed into service\. Whenever your application needs to scale out, Amazon EC2 Auto Scaling uses the pre\-initialized instances from the warm pool rather than launching cold instances, allows for any final initialization process to run, and then places the instance into service\. To learn more about warm pools and how to add a warm pool to your Auto Scaling group, see [Warm pools for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-warm-pools.html) *in the Amazon EC2 Auto Scaling User Guide*\. To use warm pools with your Amazon ECS cluster, set the `ECS_WARM_POOLS_CHECK` agent configuration variable to `true` in the **User data** field of your Amazon EC2 Auto Scaling group launch template\. The following shows an example of how the agent configuration variable can be specified in the **User data** field of an Amazon EC2 launch template\. 

```
#!/bin/bash
cat <<'EOF' >> /etc/ecs/ecs.config
ECS_CLUSTER=MyCluster
ECS_WARM_POOLS_CHECK=true
EOF
``` The `ECS_WARM_POOLS_CHECK` variable is only supported on agent versions `1.59.0` and later\. For more information about the variable, see the [Available Parameters](ecs-agent-config.md#ecs-agent-availparam) page\.    Creating an Auto Scaling group capacity provider using the classic console  A *capacity provider* is used in association with a cluster to determine the infrastructure that a task runs on\. When creating a capacity provider, you specify the following details:   An Auto Scaling group Amazon Resource Name \(ARN\)   Whether or not to turn on managed scaling\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.   Whether or not to turn on managed termination protection\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled\.   Use the following steps to create a new capacity provider for an existing Amazon ECS cluster\. To create an Auto Scaling group capacity provider \(classic AWS Management Console\) Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  From the navigation bar, select the Region your cluster exists in\.   In the navigation pane, choose **Clusters**\.   On the **Clusters** page, select your cluster\.   On the **Cluster : *name*** page, choose **Capacity Providers**, and then choose **Create**\.   For **Capacity provider name**, enter a capacity provider name\.   For **Auto Scaling group**, select the Auto Scaling group to associate with the capacity provider\. The Auto Scaling group must already be created\. For more information, see [Creating an Auto Scaling group](asg-capacity-providers-create-auto-scaling-group.md)\.   For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.   For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.   For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled and if managed scaling is enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.   Choose **Create** to complete the capacity provider creation\.   To create an Auto Scaling group capacity provider \(AWS CLI\)  Use the following command to create a new capacity provider\.   [create\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-capacity-provider.html) \(AWS CLI\) 

    ```
    aws ecs create-capacity-provider \
         --name CapacityProviderName \
         --auto-scaling-group-provider autoScalingGroupArn="AutoScalingGroupARN",managedScaling=\{status='ENABLED|DISABLED',targetCapacity=integer,minimumScalingStepSize=integer,maximumScalingStepSize=integer\},managedTerminationProtection="ENABLED|DISABLED" \
         --region us-east-2
    ``` If you prefer to use a JSON input file with the `create-capacity-provider` command, use the following command to generate a CLI skeleton\. 

    ```
    aws ecs create-capacity-provider --generate-cli-skeleton
    ```       Updating an Auto Scaling group capacity provider using the classic console  A capacity provider can be updated to change its managed scaling and managed termination protection settings\. Use the following steps to update an existing capacity provider\. To update an Auto Scaling group capacity provider \(classic AWS Management Console\) Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  From the navigation bar, select the Region the cluster the capacity provider is associated with exists in\.   In the navigation pane, choose **Clusters**\.   On the **Clusters** page, select your cluster\.   On the **Cluster : *name*** page, choose the **Capacity Providers** tab\.   Select the capacity provider to update and choose **Update**\.   On the **Update Capacity Provider** page, the following parameters can be updated\.   For **Managed scaling**, choose your managed scaling option\. When managed scaling is enabled, Amazon ECS manages the scale\-in and scale\-out actions of the Auto Scaling group through the use of AWS Auto Scaling scaling plans\. When managed scaling is disabled, you manage your Auto Scaling groups yourself\.   For **Target capacity %**, if managed scaling is enabled, specify an integer between `1` and `100`\. The target capacity value is used as the target value for the CloudWatch metric used in the Amazon ECS\-managed target tracking scaling policy\. This target capacity value is matched on a best effort basis\. For example, a value of `100` will result in the Amazon EC2 instances in your Auto Scaling group being completely utilized and any instances not running any tasks will be scaled in, but this behavior is not guaranteed at all times\.   For **Managed termination protection**, choose your managed termination protection option\. When managed termination protection is enabled, Amazon ECS prevents Amazon EC2 instances that contain tasks and that are in an Auto Scaling group from being terminated during a scale\-in action\. Managed termination protection can only be enabled if the Auto Scaling group also has instance protection from scale in enabled and if managed scaling is enabled\. Managed termination protection is only supported on standalone tasks or tasks in a service using the replica scheduling strategy\. For tasks in a service using the daemon scheduling strategy, the instances are not protected\.     Choose **Update** to request capacity provider update\.   To verify whether the capacity provider update was successful, check the **Update Status** column on the **Capacity Providers** tab\.   To update an Auto Scaling group capacity provider \(AWS CLI\)  Use the following command to create a new capacity provider\.   [update\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/update-capacity-provider.html) \(AWS CLI\) 

    ```
    aws ecs update-capacity-provider \
         --name CapacityProviderName \
         --auto-scaling-group-provider managedScaling=\{status='ENABLED|DISABLED',targetCapacity=integer,minimumScalingStepSize=integer,maximumScalingStepSize=integer\},managedTerminationProtection="ENABLED|DISABLED" \
         --region us-east-2
    ``` If you prefer to use a JSON input file with the `create-capacity-provider` command, use the following command to generate a CLI skeleton\. 

    ```
    aws ecs update-capacity-provider --generate-cli-skeleton
    ```       Creating a cluster with an Auto Scaling group capacity provider  When a new Amazon ECS cluster is created, you can specify one or more capacity providers to associate with the cluster\. The associated capacity providers determine the infrastructure to run your tasks on\. For AWS Management Console steps, see [Creating a cluster using the classic console](create_cluster.md)\.  To create a cluster with an Auto Scaling group capacity provider \(AWS CLI\)  Use the following command to create a new cluster and associate one or more capacity providers with it\.   [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\) 

  ```
  aws ecs create-cluster \
       --cluster-name ASGCluster \
       --capacity-providers CapacityProviderA CapacityProviderB \
       --default-capacity-provider-strategy capacityProvider=CapacityProviderA,weight=1,base=1 capacityProvider=CapacityProviderB,weight=1 \
       --region us-west-2
  ``` If you prefer to use a JSON input file with the `create-cluster` command, use the following command to generate a CLI skeleton\. 

  ```
  aws ecs create-cluster --generate-cli-skeleton
  ```       Deleting an Auto Scaling group capacity provider using the classic console  If you are finished using an Auto Scaling group capacity provider, you can delete it\. Once deleted, the Auto Scaling group capacity provider will transition to the `INACTIVE` state\. Capacity providers with an `INACTIVE` status may remain discoverable in your account for a period of time\. However, this behavior is subject to change in the future, so you should not rely on `INACTIVE` capacity providers persisting\. Prior to an Auto Scaling group capacity provider being deleted, the capacity provider must be removed from the capacity provider strategy from all services\. The `UpdateService` API or the update service workflow in the AWS Management Console can be used to remove a capacity provider from a service's capacity provider strategy\. The force new deployment option can be used to ensure that any tasks using the Amazon EC2 instance capacity provided by the capacity provider are transitioned to use the capacity from the remaining capacity providers\. There are other prerequisites that must be performed to delete a capacity provider but they are specific to the tool used and are mentioned in the following steps\. Use the following steps to delete an Auto Scaling group capacity provider\.  To delete an Auto Scaling group capacity provider \(classic AWS Management Console\)  When deleting a capacity provider using the classic AWS Management Console, the console goes through two steps\. The capacity provider is first disassociated from the cluster completely and then it is deleted\. In rare cases, the capacity provider may be successfully disassociated from the cluster but is unable to be deleted\. In those cases, you must use either the Amazon ECS API or the AWS CLI to view the status of the capacity provider and delete it\.  Only capacity providers that are currently associated with a cluster are visible in the AWS Management Console\. To delete a capacity provider that is not associated with a cluster, you must use the Amazon ECS API, SDK, or AWS CLI\.   Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.  From the navigation bar, select the Region your cluster exists in\.   In the navigation pane, choose **Clusters**\.   On the **Clusters** page, select your cluster\.   On the **Cluster : *name*** page, choose the **Capacity Providers** tab\.   Select the capacity provider you want to delete and then choose **Delete**\.     To delete an Auto Scaling group capacity provider \(AWS CLI\)  When using the AWS CLI to delete a capacity provider, the capacity provider must first be disassociated from the cluster\. The following options are available to disassociate a capacity provider from a cluster\. Option 1: Use the delete command to delete the cluster\. This will disassociate the capacity provider from the cluster upon successful deletion of the cluster\.   [delete\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-cluster.html) \(AWS CLI\) 

  ```
  aws ecs delete-cluster \
       --cluster MyCluster
  ```   Option 2: Use the put\-cluster\-capacity\-providers command to disassociate a capacity provider from a cluster\. If you have other capacity providers associated with the cluster that you want to have remain associated with the cluster, you must include those when using the command\. The following example will remove all existing capacity providers from the specified cluster\.   [put\-cluster\-capacity\-providers](https://docs.aws.amazon.com/cli/latest/reference/ecs/put-cluster-capacity-providers.html) \(AWS CLI\) 

  ```
  aws ecs put-cluster-capacity-providers \
       --cluster MyCluster \
       --capacity-providers [] \
       --default-capacity-provider-strategy []
  ```   Use the delete\-capacity\-provider command to delete a capacity provider\. You can specify the capacity provider using its short name or the full Amazon Resource Name \(ARN\)\.   [delete\-capacity\-provider](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-capacity-provider.html) \(AWS CLI\) Example using the short name: 

  ```
  aws ecs delete-capacity-provider \
       --capacity-provider ExampleCapacityProvider
  ``` Example using the full ARN: 

  ```
  aws ecs delete-capacity-provider \
       --capacity-provider arn:aws:ecs:us-west-2:123456789012:capacity-provider/ExampleCapacityProvider
  ```      ](cluster-capacity-providers.md#cluster-capacity-providers.title) for more information\.

**To create a capacity provider**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **Capacity Providers** tab, choose **Create**\.

1. On the **Create Capacity Provider** page, complete the following steps\.

   1. For **Capacity provider name**, enter `ConsoleTutorial-capacityprovider` for the name\.

   1. For **Auto Scaling group**, select the `ConsoleTutorial-ASG` Auto Scaling group you created\.

   1. For **Managed scaling**, choose **Enabled**\. This enables Amazon ECS to manage the scale\-in and scale\-out actions for the capacity provider\.

   1. For **Target capacity %**, enter `100`\.

   1. For **Managed termination protection**, choose **Enabled**\. This prevents your container instances that contain tasks and that are in the Auto Scaling group from being terminated during a scale\-in action\.

   1. Choose **Create**\.

   1. Choose **View in cluster** to see your new capacity provider\.

   1. Repeat steps 4 to 6, creating a second capacity provider with name `ConsoleTutorial-capacityprovider-burst` with your `ConsoleTutorial-ASG-burst` Auto Scaling group\.

## Step 4: Set a default capacity provider strategy for the cluster<a name="console-tutorial-default-strategy"></a>

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

## Step 5: Register a task definition<a name="console-tutorial-register-task-definition"></a>

Before you can run a task on your cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example is a simple task definition that uses an `amazonlinux` image from Docker Hub and simply sleeps\. For more information about the available task definition parameters, see [Amazon ECS task definitions](task_definitions.md)\.

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

## Step 6: Run a task<a name="console-tutorial-run-task"></a>

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

1. On the **All metrics** tab, choose `AWS/ECS/ManagedScaling`\.

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

**To verify the instances in the Auto Scaling group**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. On the navigation bar at the top of the screen, select the **US West \(Oregon\)** Region\.

1. In the navigation pane, choose **Clusters**\.

1. On the **Clusters** page, select your `ConsoleTutorial-cluster` cluster\.

1. On the **ECS Instances** tab, confirm you see four instances registered, which matches your Auto Scaling group values\.

1. On the **Tasks** tab, confirm you see five tasks in `RUNNING` status\.

## Step 8: Clean up<a name="console-tutorial-cleanup"></a>

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