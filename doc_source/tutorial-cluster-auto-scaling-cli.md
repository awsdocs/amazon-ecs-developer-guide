# Tutorial: Using cluster auto scaling with the AWS CLI<a name="tutorial-cluster-auto-scaling-cli"></a>

Amazon ECS cluster auto scaling can be set up and configured using the AWS Management Console, AWS CLI, or Amazon ECS API\.

This tutorial walks you through creating the resources for cluster auto scaling using the AWS CLI\. Where resources require a name, we will use the prefix `CLItutorial` to ensure they all have unique names and to make them easy to locate\.

For an AWS Management Console tutorial, see [Tutorial: Using cluster auto scaling with the AWS Management Console](tutorial-cluster-auto-scaling-console.md)\.

**Topics**
+ [Prerequisites](#cli-tutorial-prereqs)
+ [Step 1: Create the Auto Scaling resources](#cli-tutorial-asg)
+ [Step 2: Create the Amazon ECS resources](#cli-tutorial-cluster)
+ [Step 3: Register a task definition](#cli-tutorial-register-task-definition)
+ [Step 4: Run a task](#cli-tutorial-run-task)
+ [Step 5: Verify](#cli-tutorial-verify)
+ [Step 6: Clean up](#cli-tutorial-cleanup)

## Prerequisites<a name="cli-tutorial-prereqs"></a>

This tutorial assumes that the following prerequisites have been completed:
+ The latest version of the AWS CLI is installed and configured\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ The steps in [Setting up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ The Amazon ECS container instance IAM role is created\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\.
+ The Amazon ECS service\-linked IAM role is created\. For more information, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\.
+ The Auto Scaling service\-linked IAM role is created\. For more information, see [Service\-Linked Roles for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

## Step 1: Create the Auto Scaling resources<a name="cli-tutorial-asg"></a>

This step walks you through creating an Auto Scaling launch configuration and two Auto Scaling groups\. This step requires that you already have a VPC created along with at least one public subnet and a security group\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.

**To create the Auto Scaling resources**

1. Create an Auto Scaling launch configuration with the following steps\. For more information, see [Launch Configurations](https://docs.aws.amazon.com/autoscaling/ec2/userguide/LaunchConfiguration.html) in the *Amazon EC2 Auto Scaling User Guide*\.

   1. Create a file named `CLItutorial-launchconfig.json` with the following contents\. You must replace the following values:
      + Replace the `ImageId` with the latest Amazon Linux 2 Amazon ECS\-optimized AMI\. For more information, see [Amazon ECS\-optimized AMIs](ecs-optimized_AMI.md)\.
      + Replace the `SecurityGroups` value with your security group ID associated with your VPC\.
      + Replace the `IamInstanceProfile` value with the full Amazon Resource Name \(ARN\) of the instance profile for your Amazon ECS container instance IAM role\. An instance profile enables you to pass IAM role information to an Amazon EC2 instance when the instance starts\. If your Amazon ECS container instance IAM role is created already, you can retrieve the ARN of the instance profile with the following command\. Replace the container instance IAM role name in this example with the name of your container instance IAM role\.

        ```
        aws iam list-instance-profiles-for-role --role-name ecsInstanceRole
        ```

      ```
      {
          "LaunchConfigurationName": "CLItutorial-launchconfig",
          "ImageId": "ami-04240723d51aeeb2d",
          "SecurityGroups": [
              "sg-abcd1234"
          ],
          "InstanceType": "t2.micro",
          "BlockDeviceMappings": [
              {
                  "DeviceName": "/dev/xvdcz",
                  "Ebs": {
                      "VolumeSize": 22,
                      "VolumeType": "gp2",
                      "DeleteOnTermination": true,
                      "Encrypted": true
                  }
              }
          ],
          "InstanceMonitoring": {
              "Enabled": false
          },
          "IamInstanceProfile": "arn:aws:iam::111122223333:instance-profile/ecsInstanceRole",
          "AssociatePublicIpAddress": true
      }
      ```

   1. Create a file named `CLItutorial-userdata.txt` with the following contents\. This user data script will be used to register the Amazon EC2 instances created by the Auto Scaling group with the Amazon ECS cluster used in the tutorial, which we have named `CLItutorial-cluster`\.

      ```
      #!/bin/bash
      echo ECS_CLUSTER=CLItutorial-cluster >> /etc/ecs/ecs.config
      ```

   1. Create the Auto Scaling launch configuration\.

      ```
      aws autoscaling create-launch-configuration --cli-input-json file://CLItutorial-launchconfig.json --user-data file://CLItutorial-userdata.txt --region us-west-2
      ```

      If the command is successful, there will be no output\. Use the following command to display the details of your launch configuration\.

      ```
      aws autoscaling describe-launch-configurations --launch-configuration-names CLItutorial-launchconfig --region us-west-2
      ```

1. Create an Auto Scaling group with the following steps\. For more information, see [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html) in the *Amazon EC2 Auto Scaling User Guide*\.

   1. Create a file named `CLItutorial-asgconfig.json` with the following contents\. You must replace the following values:
      + Replace the `AvailabilityZones` value with the Availability Zone your subnet exists in\.
      + Replace the `VPCZoneIdentifier` value with the ID of a subnet in your VPC\.
      + Replace the `ServiceLinkedRoleARN` value with the full Amazon Resource Name \(ARN\) of your Auto Scaling service\-linked IAM role\. For more information, see [Service\-Linked Roles for Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/autoscaling-service-linked-role.html) in the *Amazon EC2 Auto Scaling User Guide*\.
**Important**  
Specifying `true` for the `NewInstancesProtectedFromScaleIn` value is required when using the Amazon ECS managed scaling feature for cluster auto scaling\. This tutorial demonstrates using the Amazon ECS managed scaling feature for cluster auto scaling in a later step\.

      ```
      {
          "LaunchConfigurationName": "CLItutorial-launchconfig",
          "MinSize": 0,
          "MaxSize": 100,
          "DesiredCapacity": 0,
          "DefaultCooldown": 300,
          "AvailabilityZones": [
              "us-west-2c"
          ],
          "HealthCheckType": "EC2",
          "HealthCheckGracePeriod": 300,
          "VPCZoneIdentifier": "subnet-abcd1234",
          "TerminationPolicies": [
              "DEFAULT"
          ],
          "NewInstancesProtectedFromScaleIn": true,
          "ServiceLinkedRoleARN": "arn:aws:iam::111122223333:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling"
      }
      ```

   1. Create an Auto Scaling group\.

      ```
      aws autoscaling create-auto-scaling-group --auto-scaling-group-name CLItutorial-asg --cli-input-json file://CLItutorial-asgconfig.json --region us-west-2
      ```

      If the command is successful, there will be no output\.

   1. To create a second scaling group, repeat the same command with a different Auto Scaling group name\.

      ```
      aws autoscaling create-auto-scaling-group --auto-scaling-group-name CLItutorial-asg-burst --cli-input-json file://CLItutorial-asgconfig.json --region us-west-2
      ```

      If the command is successful, there will be no output\.

   1. Retrieve the details of the two Auto Scaling groups you just created\.

      ```
      aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CLItutorial-asg CLItutorial-asg-burst --region us-west-2
      ```

      The output will display the full Amazon Resource Name \(ARN\) of the two Auto Scaling groups, which you will need for the next step\.

      ```
      {
          "AutoScalingGroups": [
              {
                  "AutoScalingGroupName": "CLItutorial-asg",
                  "AutoScalingGroupARN": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:24c44d96-606a-427f-826a-f64ba4cc918c:autoScalingGroupName/CLItutorial-asg",
                  "LaunchConfigurationName": "CLItutorial-launchconfig",
                  ...
              },
              {
                  "AutoScalingGroupName": "CLItutorial-asg-burst",
                  "AutoScalingGroupARN": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:407c3102-fb00-4a0c-a1a8-0b242203a262:autoScalingGroupName/CLItutorial-asg-burst",
                  "LaunchConfigurationName": "CLItutorial-launchconfig",
                  ...
              }
          ]
      }
      ```

## Step 2: Create the Amazon ECS resources<a name="cli-tutorial-cluster"></a>

This step will walk you through creating two Amazon ECS capacity providers and one Amazon ECS cluster\. You can associate one Auto Scaling group with each capacity provider\. This tutorial uses the `us-west-2` Region\.

**To create the Amazon ECS resources**

1. Create an Amazon ECS capacity provider with the following steps\.

   1. Create a file named `CLItutorial-capacityprovider.json` with the following contents\. Replace the `autoScalingGroupArn` value with the full Amazon Resource Name \(ARN\) of the first Auto Scaling group you created in step 1\.

      ```
      {
          "name": "CLItutorial-capacityprovider",
          "autoScalingGroupProvider": {
              "autoScalingGroupArn": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:24c44d96-606a-427f-826a-f64ba4cc918c:autoScalingGroupName/CLItutorial-asg",
              "managedScaling": {
                  "status": "ENABLED",
                  "targetCapacity": 100,
                  "minimumScalingStepSize": 1,
                  "maximumScalingStepSize": 100
              },
              "managedTerminationProtection": "ENABLED"
          }
      }
      ```

   1. Create the capacity provider\.

      ```
      aws ecs create-capacity-provider --cli-input-json file://CLItutorial-capacityprovider.json --region us-west-2
      ```

      The output returns a description of the capacity provider\.

      ```
      {
          "capacityProvider": {
              "capacityProviderArn": "arn:aws:ecs:us-west-2:111122223333:capacity-provider/CLItutorial-capacityprovider/1a484097-270b-45ea-be85-592924EXAMPLE",
              "name": "CLItutorial-capacityprovider",
              "status": "ACTIVE",
              "autoScalingGroupProvider": {
                  "autoScalingGroupArn": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:24c44d96-606a-427f-826a-f64ba4cc918c:autoScalingGroupName/CLItutorial-asg",
                  "managedScaling": {
                      "status": "ENABLED",
                      "targetCapacity": 100,
                      "minimumScalingStepSize": 1,
                      "maximumScalingStepSize": 100
                  },
                  "managedTerminationProtection": "ENABLED"
              },
              "tags": []
          }
      }
      ```

1. Create a second Amazon ECS capacity provider with the following steps\. The purpose of the second capacity provider will be to provide burst capacity to the cluster\. In production you may use Amazon EC2 Spot Instances, but for the purposes of this tutorial we will be using On\-Demand Instance\.

   1. Create a file named `CLItutorial-capacityprovider-burst.json` with the following contents\. Replace the `autoScalingGroupArn` value with the full Amazon Resource Name \(ARN\) of the second Auto Scaling group you created in step 1\.

      ```
      {
          "name": "CLItutorial-capacityprovider-burst",
          "autoScalingGroupProvider": {
              "autoScalingGroupArn": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:407c3102-fb00-4a0c-a1a8-0b242203a262:autoScalingGroupName/CLItutorial-asg-burst",
              "managedScaling": {
                  "status": "ENABLED",
                  "targetCapacity": 100,
                  "minimumScalingStepSize": 1,
                  "maximumScalingStepSize": 100
              },
              "managedTerminationProtection": "ENABLED"
          }
      }
      ```

   1. Create the capacity provider\.

      ```
      aws ecs create-capacity-provider --cli-input-json file://CLItutorial-capacityprovider-burst.json --region us-west-2
      ```

      The output returns a description of the capacity provider\.

      ```
      {
          "capacityProvider": {
              "capacityProviderArn": "arn:aws:ecs:us-west-2:111122223333:capacity-provider/CLItutorial-capacityprovider-burst/5e4344097-270b-78ea-be85-592924EXAMPLE",
              "name": "CLItutorial-capacityprovider-burst",
              "status": "ACTIVE",
              "autoScalingGroupProvider": {
                  "autoScalingGroupArn": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:407c3102-fb00-4a0c-a1a8-0b242203a262:autoScalingGroupName/CLItutorial-asg-burst",
                  "managedScaling": {
                      "status": "ENABLED",
                      "targetCapacity": 100,
                      "minimumScalingStepSize": 1,
                      "maximumScalingStepSize": 100
                  },
                  "managedTerminationProtection": "ENABLED"
              },
              "tags": []
          }
      }
      ```

1. Create an Amazon ECS cluster\. The cluster name must match the name you specified in the user data script specified in the Auto Scaling launch configuration created in step 1 of this tutorial\. The capacity providers we created in the previous step will be associated with this cluster\.

   When a task is run or a service is created, you specify a capacity provider strategy for the tasks to use\. Similarly, a default capacity provider strategy can be specified for a cluster\. This enables you to run tasks and create services without specifying a capacity provider strategy, as these tasks and actions will use the cluster's default capacity provider strategy\. When specifying a default capacity provider strategy, you may optionally specify both a base and weight value\. These values are useful when you are associating multiple capacity providers with a cluster\. For more information, see [Capacity provider concepts](cluster-capacity-providers.md#capacity-providers-concepts)\.

   ```
   aws ecs create-cluster --cluster-name CLItutorial-cluster --capacity-providers CLItutorial-capacityprovider CLItutorial-capacityprovider-burst --default-capacity-provider-strategy capacityProvider=CLItutorial-capacityprovider,weight=1 capacityProvider=CLItutorial-capacityprovider-burst,weight=1 --region us-west-2
   ```

   The output returns a description of the cluster, including the cluster status and the cluster attachment details\. The description, displays the AWS Auto Scaling scaling plans that Amazon ECS creates for you\.\. A scaling plan is created for each capacity provider\.

   ```
   {
       "cluster": {
           "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
           "clusterName": "CLItutorial-cluster",
           "status": "PROVISIONING",
           "registeredContainerInstancesCount": 0,
           "runningTasksCount": 0,
           "pendingTasksCount": 0,
           "activeServicesCount": 0,
           "statistics": [],
           "tags": [],
           "settings": [
               {
                   "name": "containerInsights",
                   "value": "disabled"
               }
           ],
           "capacityProviders": [
               "CLItutorial-capacityprovider",
               "CLItutorial-capacityprovider-burst"
           ],
           "defaultCapacityProviderStrategy": [
               {
                   "capacityProvider": "CLItutorial-capacityprovider",
                   "weight": 1,
                   "base": 0
               },
               {
                   "capacityProvider": "CLItutorial-capacityprovider-burst",
                   "weight": 1,
                   "base": 0
               }
           ],
           "attachments": [
               {
                   "id": "4aaee2ac-2a66-457c-b0df-a0bc871f5ead",
                   "type": "asp",
                   "status": "PRECREATED",
                   "details": [
                       {
                           "name": "capacityProviderName",
                           "value": "CLItutorial-capacityprovider"
                       },
                       {
                           "name": "scalingPlanName",
                           "value": "ECSManagedAutoScalingPlan-27eb1e2a-5698-4ae7-b382-1553b8ba1095"
                       }
                   ]
               },
               {
                   "id": "03e99543-935d-4ea2-9a96-4b9dd63d320f",
                   "type": "asp",
                   "status": "PRECREATED",
                   "details": [
                       {
                           "name": "capacityProviderName",
                           "value": "CLItutorial-capacityprovider-burst"
                       },
                       {
                           "name": "scalingPlanName",
                           "value": "ECSManagedAutoScalingPlan-f9ea310b-680e-4654-b8c6-1c4862b29a77"
                       }
                   ]
               }
           ],
           "attachmentsStatus": "UPDATE_IN_PROGRESS"
       }
   }
   ```

1. Before you continue to the next step, you must ensure that the cluster is in an `ACTIVE` state, that each of your cluster attachments are in a `CREATED` state, and that the attachment status is in `UPDATE_COMPLETE` state\. This can be done by describing the cluster\.

   ```
   aws ecs describe-clusters --clusters CLItutorial-cluster --include ATTACHMENTS --region us-west-2
   ```

   The output returns a description of your cluster\. Verify the cluster and attachment status fields\.

   ```
   {
       "cluster": {
           "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
           "clusterName": "CLItutorial-cluster",
           "status": "ACTIVE",
           "registeredContainerInstancesCount": 0,
           "runningTasksCount": 0,
           "pendingTasksCount": 0,
           "activeServicesCount": 0,
           "statistics": [],
           "tags": [],
           "settings": [
               {
                   "name": "containerInsights",
                   "value": "disabled"
               }
           ],
           "capacityProviders": [
               "CLItutorial-capacityprovider",
               "CLItutorial-capacityprovider-burst"
           ],
           "defaultCapacityProviderStrategy": [
               {
                   "capacityProvider": "CLItutorial-capacityprovider",
                   "weight": 1,
                   "base": 0
               },
               {
                   "capacityProvider": "CLItutorial-capacityprovider-burst",
                   "weight": 1,
                   "base": 0
               }
           ],
           "attachments": [
               {
                   "id": "4aaee2ac-2a66-457c-b0df-a0bc871f5ead",
                   "type": "asp",
                   "status": "CREATED",
                   "details": [
                       {
                           "name": "capacityProviderName",
                           "value": "CLItutorial-capacityprovider"
                       },
                       {
                           "name": "scalingPlanName",
                           "value": "ECSManagedAutoScalingPlan-27eb1e2a-5698-4ae7-b382-1553b8ba1095"
                       }
                   ]
               },
               {
                   "id": "03e99543-935d-4ea2-9a96-4b9dd63d320f",
                   "type": "asp",
                   "status": "CREATED",
                   "details": [
                       {
                           "name": "capacityProviderName",
                           "value": "CLItutorial-capacityprovider-burst"
                       },
                       {
                           "name": "scalingPlanName",
                           "value": "ECSManagedAutoScalingPlan-f9ea310b-680e-4654-b8c6-1c4862b29a77"
                       }
                   ]
               }
           ],
           "attachmentsStatus": "UPDATE_COMPLETE"
       }
   }
   ```

## Step 3: Register a task definition<a name="cli-tutorial-register-task-definition"></a>

Before you can run a task on your cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example is a simple task definition that uses an `amazonlinux` image from Docker Hub and just sleeps\. For more information about the available task definition parameters, see [Amazon ECS Task definitions](task_definitions.md)\.

**To register a task definition**

1. Create a file named `CLItutorial-taskdef.json` with the following contents\.

   ```
   {
       "family": "CLItutorial-taskdef",
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

1. Register the task definition\.

   ```
   aws ecs register-task-definition --cli-input-json file://CLItutorial-taskdef.json --region us-west-2
   ```

   The output returns a description of the task definition after it completes its registration\.

   ```
   {
       "taskDefinition": {
           "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
           "containerDefinitions": [
               {
                   "name": "sleep",
                   "image": "amazonlinux:2",
                   "cpu": 0,
                   "memory": 20,
                   "portMappings": [],
                   "essential": true,
                   "command": [
                       "sh",
                       "-c",
                       "sleep infinity"
                   ],
                   "environment": [],
                   "mountPoints": [],
                   "volumesFrom": []
               }
           ],
           "family": "sleep360",
           "revision": 1,
           "volumes": [],
           "status": "ACTIVE",
           "placementConstraints": [],
           "compatibilities": [
               "EC2"
           ],
           "requiresCompatibilities": [
               "EC2"
           ]
       }
   }
   ```

## Step 4: Run a task<a name="cli-tutorial-run-task"></a>

After you have registered a task definition for your account, you can run a task in the cluster\. For this tutorial, you run five instances of the `CLItutorial-taskdef:1` task definition in your `CLItutorial-cluster` cluster\.

**To run a task**
+ Run five instances of the `sleep360:1` task definition you registered in the previous step\.

  ```
  aws ecs run-task --cluster CLItutorial-cluster --count 5 --task-definition CLItutorial-taskdef:1 --region us-west-2
  ```

  The output returns a description of the tasks\. Each task will have a capacity provider associated with it\.

  ```
  {
      "tasks": [
          {
              "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/CLItutorial-cluster/12648317756d430e8e320bbc4aEXAMPLE",
              "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
              "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
              "overrides": {
                  "containerOverrides": [],
                  "inferenceAcceleratorOverrides": []
              },
              "lastStatus": "PROVISIONING",
              "desiredStatus": "RUNNING",
              "cpu": "0",
              "memory": "20",
              "containers": [],
              "version": 1,
              "createdAt": 1574320187.938,
              "group": "family:CLItutorial-taskdef",
              "launchType": "EC2",
              "capacityProviderName": "CLItutorial-capacityprovider-burst",
              "attachments": [],
              "tags": []
          },
          {
              "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/CLItutorial-cluster/e7f774f1570b4dddaa08626809EXAMPLE",
              "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
              "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
              "overrides": {
                  "containerOverrides": [],
                  "inferenceAcceleratorOverrides": []
              },
              "lastStatus": "PROVISIONING",
              "desiredStatus": "RUNNING",
              "cpu": "0",
              "memory": "20",
              "containers": [],
              "version": 1,
              "createdAt": 1574320187.938,
              "group": "family:CLItutorial-taskdef",
              "launchType": "EC2",
              "capacityProviderName": "CLItutorial-capacityprovider-burst",
              "attachments": [],
              "tags": []
          },
          {
              "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/CLItutorial-cluster/f0f06980486e43438bc75a2184EXAMPLE",
              "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
              "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
              "overrides": {
                  "containerOverrides": [],
                  "inferenceAcceleratorOverrides": []
              },
              "lastStatus": "PROVISIONING",
              "desiredStatus": "RUNNING",
              "cpu": "0",
              "memory": "20",
              "containers": [],
              "version": 1,
              "createdAt": 1574320187.938,
              "group": "family:CLItutorial-taskdef",
              "launchType": "EC2",
              "capacityProviderName": "CLItutorial-capacityprovider",
              "attachments": [],
              "tags": []
          },
          {
              "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/CLItutorial-cluster/7e3e0da4e71d4bf9ba8e4371dcEXAMPLE",
              "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
              "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
              "overrides": {
                  "containerOverrides": [],
                  "inferenceAcceleratorOverrides": []
              },
              "lastStatus": "PROVISIONING",
              "desiredStatus": "RUNNING",
              "cpu": "0",
              "memory": "20",
              "containers": [],
              "version": 1,
              "createdAt": 1574320187.938,
              "group": "family:CLItutorial-taskdef",
              "launchType": "EC2",
              "capacityProviderName": "CLItutorial-capacityprovider",
              "attachments": [],
              "tags": []
          },
          {
              "taskArn": "arn:aws:ecs:us-west-2:111122223333:task/CLItutorial-cluster/c71afd510c6e4ae58b86da3490EXAMPLE",
              "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
              "taskDefinitionArn": "arn:aws:ecs:us-west-2:111122223333:task-definition/CLItutorial-taskdef:1",
              "overrides": {
                  "containerOverrides": [],
                  "inferenceAcceleratorOverrides": []
              },
              "lastStatus": "PROVISIONING",
              "desiredStatus": "RUNNING",
              "cpu": "0",
              "memory": "20",
              "containers": [],
              "version": 1,
              "createdAt": 1574320187.938,
              "group": "family:CLItutorial-taskdef",
              "launchType": "EC2",
              "capacityProviderName": "CLItutorial-capacityprovider",
              "attachments": [],
              "tags": []
          }
      ],
      "failures": []
  }
  ```

## Step 5: Verify<a name="cli-tutorial-verify"></a>

At this point in the tutorial you should have two Auto Scaling groups with one capacity provider for each of them\. The capacity providers have Amazon ECS managed scaling enabled\. A cluster was created and five tasks are running\. The result should be your `CLItutorial-asg` scaling group should contain two instances, each with two tasks running on them, and your `CLItutorial-asg-burst` scaling group should contain two instances, with a single task running on one of them\.

**To verify the scaling**

1. Describe your cluster to determine how many container instances have been registered to it\.

   ```
   aws ecs describe-clusters --clusters CLItutorial-cluster --include ATTACHMENTS --region us-west-2
   ```

   The output returns a description of the cluster\. The following snippet confirms that the correct number of container instances were registered\.

   ```
   {
       "clusters": [
           {
               "clusterArn": "arn:aws:ecs:us-west-2:111122223333:cluster/CLItutorial-cluster",
               "clusterName": "CLItutorial-cluster",
               "status": "ACTIVE",
               "registeredContainerInstancesCount": 3,
               "runningTasksCount": 3,
               ...
               "capacityProviders": [
                   "CLItutorial-capacityprovider-burst",
                   "CLItutorial-capacityprovider"
               ],
               "defaultCapacityProviderStrategy": [
                   {
                       "capacityProvider": "CLItutorial-capacityprovider",
                       "weight": 1,
                       "base": 0
                   },
                   {
                       "capacityProvider": "CLItutorial-capacityprovider-burst",
                       "weight": 1,
                       "base": 0
                   }
               ],
               "attachments": [
                   {
                       "id": "09f0a708-a91c-421f-a4a8-85db689a2244",
                       "type": "asp",
                       "status": "CREATED",
                       "details": [
                           {
                               "name": "capacityProviderName",
                               "value": "CLItutorial-capacityprovider-burst"
                           },
                           {
                               "name": "scalingPlanName",
                               "value": "ECSManagedAutoScalingPlan-811242a0-b26c-418c-86c0-2da93feb23e3"
                           }
                       ]
                   },
                   {
                       "id": "b3f407db-3d5b-4b51-88cd-bcd233d70682",
                       "type": "asp",
                       "status": "CREATED",
                       "details": [
                           {
                               "name": "capacityProviderName",
                               "value": "CLItutorial-capacityprovider"
                           },
                           {
                               "name": "scalingPlanName",
                               "value": "ECSManagedAutoScalingPlan-dcd587aa-484f-45d0-8385-5653da381038"
                           }
                       ]
                   }
               ],
               "attachmentsStatus": "UPDATE_COMPLETE"
           }
       ],
       "failures": []
   }
   ```

1. Describe your Auto Scaling groups to verify that the scaling plans set the proper desired capacity values\.

   ```
   aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names CLItutorial-asg CLItutorial-asg-burst --region us-west-2
   ```

   The output returns a description of the Auto Scaling groups\. The following snippet confirms the desired capacity and container instance details of each Auto Scaling group\.

   ```
   {
       "AutoScalingGroups": [
           {
               "AutoScalingGroupName": "CLItutorial-asg-burst",
               "AutoScalingGroupARN": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:89e0ae05-4f1d-46b7-b862-331a3fef4488:autoScalingGroupName/CLItutorial-asg-burst",
               "LaunchConfigurationName": "CLItutorial-launchconfig",
               "MinSize": 0,
               "MaxSize": 10000,
               "DesiredCapacity": 1,
               ...
               "Instances": [
                   {
                       "InstanceId": "i-0880a00d7040f2b6a",
                       "AvailabilityZone": "us-west-2c",
                       "LifecycleState": "InService",
                       "HealthStatus": "Healthy",
                       "LaunchConfigurationName": "CLItutorial-launchconfig",
                       "ProtectedFromScaleIn": true
                   }
               ],
               ...
               "NewInstancesProtectedFromScaleIn": true,
               "ServiceLinkedRoleARN": "arn:aws:iam::111122223333:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling"
           },
           {
               "AutoScalingGroupName": "CLItutorial-asg",
               "AutoScalingGroupARN": "arn:aws:autoscaling:us-west-2:111122223333:autoScalingGroup:292c0be4-4218-4f65-9903-df914d398658:autoScalingGroupName/CLItutorial-asg",
               "LaunchConfigurationName": "CLItutorial-launchconfig",
               "MinSize": 0,
               "MaxSize": 10000,
               "DesiredCapacity": 2,
               ...
               "Instances": [
                   {
                       "InstanceId": "i-0d29b646b6f6b69c5",
                       "AvailabilityZone": "us-west-2c",
                       "LifecycleState": "InService",
                       "HealthStatus": "Healthy",
                       "LaunchConfigurationName": "CLItutorial-launchconfig",
                       "ProtectedFromScaleIn": true
                   },
                   {
                       "InstanceId": "i-0eb1b33b75fb12da5",
                       "AvailabilityZone": "us-west-2c",
                       "LifecycleState": "InService",
                       "HealthStatus": "Healthy",
                       "LaunchConfigurationName": "CLItutorial-launchconfig",
                       "ProtectedFromScaleIn": true
                   }
               ],
               ...
               "NewInstancesProtectedFromScaleIn": true,
               "ServiceLinkedRoleARN": "arn:aws:iam::111122223333:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling"
           }
       ]
   }
   ```

## Step 6: Clean up<a name="cli-tutorial-cleanup"></a>

When you have finished this tutorial, clean up the resources associated with it to avoid incurring charges for resources that you aren't using\. Deleting capacity providers and task definitions are not supported, but there is no cost associated with these resources\.

**To clean up the tutorial resources**

1. List the tasks in your cluster\.

   ```
   aws ecs list-tasks --cluster CLItutorial-cluster --region us-west-2
   ```

   The output returns a list of the tasks with the full ARNs\.

   ```
   {
       "taskArns": [
           "arn:aws:ecs:us-west-2:111122223333:task/3769f4fd-fe01-4629-9c9d-19b36bEXAMPLE",
           "arn:aws:ecs:us-west-2:111122223333:task/3a311591-de1a-4d6a-89dd-2be110EXAMPLE",
           "arn:aws:ecs:us-west-2:111122223333:task/5b46ed48-25c0-4eee-842d-8f89c6EXAMPLE",
           "arn:aws:ecs:us-west-2:111122223333:task/61b417b4-5a79-4cf7-9cef-18f5d4EXAMPLE",
           "arn:aws:ecs:us-west-2:111122223333:task/6272948e-09e9-4987-a26f-802de9EXAMPLE"
       ]
   }
   ```

1. Stop each of the tasks in your cluster using either the ID or full ARN of the task from the output of the previous step\. Repeat this step for each of the five running tasks\.

   ```
   aws ecs stop-task --cluster CLItutorial-cluster --task 3769f4fd-fe01-4629-9c9d-19b36bEXAMPLE --region us-west-2
   ```

   The output returns a description of the task, with an updated desired status of `STOPPED`\.

1. Delete the Auto Scaling groups using the following steps\. Specifying the `--force-delete` parameter will terminate the container instances as well\.

   1. Delete the first Auto Scaling group\.

      ```
      aws autoscaling delete-auto-scaling-group --auto-scaling-group-name CLItutorial-asg --force-delete --region us-west-2
      ```

   1. Delete the second Auto Scaling group\.

      ```
      aws autoscaling delete-auto-scaling-group --auto-scaling-group-name CLItutorial-asg-burst --force-delete --region us-west-2
      ```

1. Delete the Amazon ECS cluster\.

   ```
   aws ecs delete-cluster --cluster CLItutorial-cluster --region us-west-2
   ```