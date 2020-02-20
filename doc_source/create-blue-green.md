# Tutorial: Creating a Service Using a Blue/Green Deployment<a name="create-blue-green"></a>

Amazon ECS has integrated blue/green deployments into the Create Service wizard on the Amazon ECS console\. For more information, see [Creating a Service](create-service.md)\.

The following tutorial shows how to create an Amazon ECS service containing a Fargate task that uses the blue/green deployment type with the AWS CLI\.

## Prerequisites<a name="create-blue-green-prereqs"></a>

This tutorial assumes that you have completed the following prerequisites:
+ The latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading the AWS CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ The steps in [Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md) have been completed\.
+ Your AWS user has the required permissions specified in the [Amazon ECS First Run Wizard Permissions](security_iam_id-based-policy-examples.md#first-run-permissions) IAM policy example\.
+ You have a VPC and security group created to use\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-public-private-vpc.html)\.
+ The Amazon ECS CodeDeploy IAM role is created\. For more information, see [Amazon ECS CodeDeploy IAM Role](codedeploy_IAM_role.md)\.

## Step 1: Create an Application Load Balancer<a name="create-blue-green-loadbalancer"></a>

Amazon ECS services using the blue/green deployment type require the use of either an Application Load Balancer or a Network Load Balancer\. This tutorial uses an Application Load Balancer\.

**To create an Application Load Balancer**

1. Use the [create\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-load-balancer.html) command to create an Application Load Balancer\. Specify two subnets that aren't from the same Availability Zone as well as a security group\.

   ```
   aws elbv2 create-load-balancer \
        --name bluegreen-alb \
        --subnets subnet-abcd1234 subnet-abcd5678 \
        --security-groups sg-abcd1234 \
        --region us-east-1
   ```

   The output includes the Amazon Resource Name \(ARN\) of the load balancer, with the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create a target group\. This target group will route traffic to the original task set in your service\.

   ```
   aws elbv2 create-target-group \
        --name bluegreentarget1 \
        --protocol HTTP \
        --port 80 \
        --target-type ip \
        --vpc-id vpc-abcd1234 \
        --region us-east-1
   ```

   The output includes the ARN of the target group, with the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4
   ```

1. Use the [create\-listener](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-listener.html) command to create a load balancer listener with a default rule that forwards requests to the target group\.

   ```
   aws elbv2 create-listener \
        --load-balancer-arn arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642 \
        --protocol HTTP \
        --port 80 \
        --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4 \
        --region us-east-1
   ```

   The output includes the ARN of the listener, with the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:listener/app/bluegreen-alb/e5ba62739c16e642/665750bec1b03bd4
   ```

## Step 2: Create an Amazon ECS Cluster<a name="create-blue-green-cluster"></a>

Use the [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) command to create a cluster named `tutorial-bluegreen-cluster` to use\.

```
aws ecs create-cluster \
     --cluster-name tutorial-bluegreen-cluster \
     --region us-east-1
```

The output includes the ARN of the cluster, with the following format:

```
arn:aws:ecs:region:aws_account_id:cluster/tutorial-bluegreen-cluster
```

## Step 3: Register a Task Definition<a name="create-blue-green-taskdef"></a>

Use the [register\-task\-definition](https://docs.aws.amazon.com/cli/latest/reference/ecs/register-task-definition.html) command to register a task definition that is compatible with Fargate\. It requires the use of the `awsvpc` network mode\. The following is the example task definition used for this tutorial\.

First, create a file named `fargate-task.json` with the following contents\. Ensure that you use the ARN for your task execution role\. For more information, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

```
{
    "family": "tutorial-task-def",
        "networkMode": "awsvpc",
        "containerDefinitions": [
            {
                "name": "sample-app",
                "image": "httpd:2.4",
                "portMappings": [
                    {
                        "containerPort": 80,
                        "hostPort": 80,
                        "protocol": "tcp"
                    }
                ],
                "essential": true,
                "entryPoint": [
                    "sh",
                    "-c"
                ],
                "command": [
                    "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
                ]
            }
        ],
        "requiresCompatibilities": [
            "FARGATE"
        ],
        "cpu": "256",
        "memory": "512",
        "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole"
}
```

Then register the task definition using the `fargate-task.json` file that you created\.

```
aws ecs register-task-definition \
     --cli-input-json file://fargate-task.json \
     --region us-east-1
```

## Step 4: Create an Amazon ECS Service<a name="create-blue-green-service"></a>

Use the [create\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-service.html) command to create a service\. 

First, create a file named `service-bluegreen.json` with the following contents\.

```
{
    "cluster": "tutorial-bluegreen-cluster",
    "serviceName": "service-bluegreen",
    "taskDefinition": "tutorial-task-def",
    "loadBalancers": [
        {
            "targetGroupArn": "arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4",
            "containerName": "sample-app",
            "containerPort": 80
        }
    ],
    "launchType": "FARGATE",
    "schedulingStrategy": "REPLICA",
    "deploymentController": {
        "type": "CODE_DEPLOY"
    },
    "platformVersion": "LATEST",
    "networkConfiguration": {
       "awsvpcConfiguration": {
          "assignPublicIp": "ENABLED",
          "securityGroups": [ "sg-abcd1234" ],
          "subnets": [ "subnet-abcd1234", "subnet-abcd5678" ]
       }
    },
    "desiredCount": 1
}
```

Then create your service using the `service-bluegreen.json` file that you created\.

```
aws ecs create-service \
     --cli-input-json file://service-bluegreen.json \
     --region us-east-1
```

The output includes the ARN of the service, with the following format:

```
arn:aws:ecs:region:aws_account_id:service/service-bluegreen
```

## Step 5: Create the AWS CodeDeploy Resources<a name="create-blue-green-codedeploy"></a>

Use the following steps to create your CodeDeploy application, the Application Load Balancer target group for the CodeDeploy deployment group, and the CodeDeploy deployment group\.

**To create CodeDeploy resources**

1. Use the [create\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-application.html) command to create an CodeDeploy application\. Specify the `ECS` compute platform\.

   ```
   aws deploy create-application \
        --application-name tutorial-bluegreen-app \
        --compute-platform ECS \
        --region us-east-1
   ```

   The output includes the application ID, with the following format:

   ```
   {
       "applicationId": "b8e9c1ef-3048-424e-9174-885d7dc9dc11"
   }
   ```

1. Use the [create\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/create-target-group.html) command to create a second Application Load Balancer target group, which will be used when creating your CodeDeploy deployment group\.

   ```
   aws elbv2 create-target-group \
        --name bluegreentarget2 \
        --protocol HTTP \
        --port 80 \
        --target-type ip \
        --vpc-id "vpc-0b6dd82c67d8012a1" \
        --region us-east-1
   ```

   The output includes the ARN for the target group, with the following format:

   ```
   arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget2/708d384187a3cfdc
   ```

1. Use the [create\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-group.html) command to create an CodeDeploy deployment group\.

   First, create a file named `tutorial-deployment-group.json` with the following contents\. This example uses the resource that you created\. For the `serviceRoleArn`, specify the ARN of your Amazon ECS CodeDeploy IAM role\. For more information, see [Amazon ECS CodeDeploy IAM Role](codedeploy_IAM_role.md)\.

   ```
   {
      "applicationName": "tutorial-bluegreen=app",
      "autoRollbackConfiguration": {
         "enabled": true,
         "events": [ "DEPLOYMENT_FAILURE" ]
      },
      "blueGreenDeploymentConfiguration": {
         "deploymentReadyOption": {
            "actionOnTimeout": "CONTINUE_DEPLOYMENT",
            "waitTimeInMinutes": 0
         },
         "terminateBlueInstancesOnDeploymentSuccess": {
            "action": "TERMINATE",
            "terminationWaitTimeInMinutes": 5
         }
      },
      "deploymentGroupName": "tutorial-bluegreen-dg",
      "deploymentStyle": {
         "deploymentOption": "WITH_TRAFFIC_CONTROL",
         "deploymentType": "BLUE_GREEN"
      },
      "loadBalancerInfo": {
         "targetGroupPairInfoList": [
           {
             "targetGroups": [
                {
                    "name": "bluegreentarget1"
                },
                {
                    "name": "bluegreentarget2"
                }
             ],
             "prodTrafficRoute": {
                 "listenerArns": [
                     "arn:aws:elasticloadbalancing:region:aws_account_id:listener/app/bluegreen-alb/e5ba62739c16e642/665750bec1b03bd4"
                 ]
             }
           }
         ]
      },
      "serviceRoleArn": "arn:aws:iam::aws_account_id:role/ecsCodeDeployRole",
      "ecsServices": [
          {
              "serviceName": "service-bluegreen",
              "clusterName": "tutorial-bluegreen-cluster"
          }
      ]
   }
   ```

   Then create the CodeDeploy deployment group\.

   ```
   aws deploy create-deployment-group \
        --cli-input-json file://tutorial-deployment-group.json \
        --region us-east-1
   ```

   The output includes the deployment group ID, with the following format:

   ```
   {
       "deploymentGroupId": "6fd9bdc6-dc51-4af5-ba5a-0a4a72431c88"
   }
   ```

## Step 6: Create and Monitor an CodeDeploy Deployment<a name="create-blue-green-verify"></a>

Use the following steps to create and upload an application specification file \(AppSpec file\) and an CodeDeploy deployment\.

**To create and monitor an CodeDeploy deployment**

1. Create and upload an AppSpec file using the following steps\.

   1. Create a file named `appspec.yaml` with the contents of the CodeDeploy deployment group\. This example uses the resources that you created earlier in the tutorial\.

      ```
      version: 0.0
      Resources:
        - TargetService:
            Type: AWS::ECS::Service
            Properties:
              TaskDefinition: "arn:aws:ecs:region:aws_account_id:task-definition/first-run-task-definition:7"
              LoadBalancerInfo:
                ContainerName: "sample-app"
                ContainerPort: 80
              PlatformVersion: "LATEST"
      ```

   1. Use the [s3 mb](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html) command to create an Amazon S3 bucket for the AppSpec file\.

      ```
      aws s3 mb s3://tutorial-bluegreen-bucket
      ```

   1. Use the [s3 cp](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command to upload the AppSpec file to the Amazon S3 bucket\.

      ```
      aws s3 cp ./appspec.yaml s3://tutorial-bluegreen-bucket/appspec.yaml
      ```

1. Create the CodeDeploy deployment using the following steps\.

   1. Create a file named `create-deployment.json` with the contents of the CodeDeploy deployment\. This example uses the resources that you created earlier in the tutorial\.

      ```
      {
          "applicationName": "tutorial-bluegreen-app",
          "deploymentGroupName": "tutorial-bluegreen-dg",
          "revision": {
              "revisionType": "S3",
              "s3Location": {
                  "bucket": "tutorial-bluegreen-bucket",
                  "key": "appspec.yaml",
                  "bundleType": "YAML"
              }
          }
      }
      ```

   1. Use the [create\-deployment](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment.html) command to create the deployment\.

      ```
      aws deploy create-deployment \
           --cli-input-json file://create-deployment.json \
           --region us-east-1
      ```

      The output includes the deployment ID, with the following format:

      ```
      {
          "deploymentId": "d-RPCR1U3TW"
      }
      ```

   1. Use the [get\-deployment\-target](https://docs.aws.amazon.com/cli/latest/reference/deploy/get-deployment-target.html) command to get the details of the deployment, specifying the `deploymentId` from the previous output\.

      ```
      aws deploy get-deployment-target \
           --deployment-id "d-IMJU3A8TW" \
           --target-id tutorial-bluegreen-cluster:service-bluegreen \
           --region us-east-1
      ```

      Continue to retrieve the deployment details until the status is `Succeeded`, as shown in the following output\.

      ```
      {
          "deploymentTarget": {
              "deploymentTargetType": "ECSTarget",
              "ecsTarget": {
                  "deploymentId": "d-RPCR1U3TW",
                  "targetId": "tutorial-bluegreen-app:service-bluegreen",
                  "targetArn": "arn:aws:ecs:region:aws_account_id:service/service-bluegreen",
                  "lastUpdatedAt": 1543431490.226,
                  "lifecycleEvents": [
                      {
                          "lifecycleEventName": "BeforeInstall",
                          "startTime": 1543431361.022,
                          "endTime": 1543431361.433,
                          "status": "Succeeded"
                      },
                      {
                          "lifecycleEventName": "Install",
                          "startTime": 1543431361.678,
                          "endTime": 1543431485.275,
                          "status": "Succeeded"
                      },
                      {
                          "lifecycleEventName": "AfterInstall",
                          "startTime": 1543431485.52,
                          "endTime": 1543431486.033,
                          "status": "Succeeded"
                      },
                      {
                          "lifecycleEventName": "BeforeAllowTraffic",
                          "startTime": 1543431486.838,
                          "endTime": 1543431487.483,
                          "status": "Succeeded"
                      },
                      {
                          "lifecycleEventName": "AllowTraffic",
                          "startTime": 1543431487.748,
                          "endTime": 1543431488.488,
                          "status": "Succeeded"
                      },
                      {
                          "lifecycleEventName": "AfterAllowTraffic",
                          "startTime": 1543431489.152,
                          "endTime": 1543431489.885,
                          "status": "Succeeded"
                      }
                  ],
                  "status": "Succeeded",
                  "taskSetsInfo": [
                      {
                          "identifer": "ecs-svc/9223370493425779968",
                          "desiredCount": 1,
                          "pendingCount": 0,
                          "runningCount": 1,
                          "status": "ACTIVE",
                          "trafficWeight": 0.0,
                          "targetGroup": {
                              "name": "bluegreentarget1"
                          }
                      },
                      {
                          "identifer": "ecs-svc/9223370493423413672",
                          "desiredCount": 1,
                          "pendingCount": 0,
                          "runningCount": 1,
                          "status": "PRIMARY",
                          "trafficWeight": 100.0,
                          "targetGroup": {
                              "name": "bluegreentarget2"
                          }
                      }
                  ]
              }
          }
      }
      ```

## Step 7: Clean Up<a name="create-blue-green-cleanup"></a>

When you have finished this tutorial, clean up the resources associated with it to avoid incurring charges for resources that you aren't using\.

**Cleaning up the tutorial resources**

1. Use the [delete\-deployment\-group](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-deployment-group.html) command to delete the CodeDeploy deployment group\.

   ```
   aws deploy delete-deployment-group \
        --application-name tutorial-bluegreen-app \
        --deployment-group-name tutorial-bluegreen-dg \
        --region us-east-1
   ```

1. Use the [delete\-application](https://docs.aws.amazon.com/cli/latest/reference/deploy/delete-application.html) command to delete the CodeDeploy application\.

   ```
   aws deploy delete-application \
        --application-name tutorial-bluegreen-app \
        --region us-east-1
   ```

1. Use the [delete\-service](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-service.html) command to delete the Amazon ECS service\. Using the `--force` flag allows you to delete a service even if it has not been scaled down to zero tasks\.

   ```
   aws ecs delete-service \
        --service arn:aws:ecs:region:aws_account_id:service/service-bluegreen \
        --force \
        --region us-east-1
   ```

1. Use the [delete\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/delete-cluster.html) command to delete the Amazon ECS cluster\.

   ```
   aws ecs delete-cluster \
        --cluster tutorial-bluegreen-cluster \
        --region us-east-1
   ```

1. Use the [s3 rm](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) command to delete the AppSpec file from the Amazon S3 bucket\.

   ```
   aws s3 rm s3://tutorial-bluegreen-bucket/appspec.yaml
   ```

1. Use the [s3 rb](https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html) command to delete the Amazon S3 bucket\.

   ```
   aws s3 rb s3://tutorial-bluegreen-bucket
   ```

1. Use the [delete\-load\-balancer](https://docs.aws.amazon.com/cli/latest/reference/elbv2/delete-load-balancer.html) command to delete the Application Load Balancer\.

   ```
   aws elbv2 delete-load-balancer \
        --load-balancer-arn arn:aws:elasticloadbalancing:region:aws_account_id:loadbalancer/app/bluegreen-alb/e5ba62739c16e642 \
        --region us-east-1
   ```

1. Use the [delete\-target\-group](https://docs.aws.amazon.com/cli/latest/reference/elbv2/delete-target-group.html) command to delete the two Application Load Balancer target groups\.

   ```
   aws elbv2 delete-target-group \
        --target-group-arn arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget1/209a844cd01825a4 \
        --region us-east-1
   ```

   ```
   aws elbv2 delete-target-group \
        --target-group-arn arn:aws:elasticloadbalancing:region:aws_account_id:targetgroup/bluegreentarget2/708d384187a3cfdc \
        --region us-east-1
   ```
