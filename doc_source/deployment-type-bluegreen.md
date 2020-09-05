# Blue/Green deployment with CodeDeploy<a name="deployment-type-bluegreen"></a>

The *blue/green* deployment type uses the blue/green deployment model controlled by CodeDeploy\. This deployment type enables you to verify a new deployment of a service before sending production traffic to it\. For more information, see [What Is CodeDeploy?](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) in the *AWS CodeDeploy User Guide*\.

There are three ways traﬃc can shift during a blue/green deployment:
+ **Canary** — Traﬃc is shifted in two increments\. You can choose from predeﬁned canary options that specify the percentage of traﬃc shifted to your updated task set in the ﬁrst increment and the interval, in minutes, before the remaining traﬃc is shifted in the second increment\.
+ **Linear** — Traﬃc is shifted in equal increments with an equal number of minutes between each increment\. You can choose from predeﬁned linear options that specify the percentage of traﬃc shifted in each increment and the number of minutes between each increment\.
+ **All\-at\-once** — All traﬃc is shifted from the original task set to the updated task set all at once\.

The following are components of CodeDeploy that Amazon ECS uses when a service uses the blue/green deployment type:

**CodeDeploy application**  
A collection of CodeDeploy resources\. This consists of one or more deployment groups\.

**CodeDeploy deployment group**  
The deployment settings\. This consists of the following:  
+ Amazon ECS cluster and service
+ Load balancer target group and listener information
+ Deployment roll back strategy
+ Traffic rerouting settings
+ Original revision termination settings
+ Deployment configuration
+ CloudWatch alarms configuration that can be set up to stop deployments
+ SNS or CloudWatch Events settings for notifications
For more information, see [Working with Deployment Groups](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups.html) in the *AWS CodeDeploy User Guide*\.

**CodeDeploy deployment configuration**  
Specifies how CodeDeploy routes production traffic to your replacement task set during a deployment\. The following pre\-defined linear and canary deployment configuration are available\. You can also create custom defined linear and canary deployments as well\. For more information, see [Working with Deployment Configurations](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html) in the *AWS CodeDeploy User Guide*\.      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-type-bluegreen.html)

**Revision**  
A revision is the CodeDeploy application specification file \(AppSpec file\)\. In the AppSpec file, you specify the full ARN of the task definition and the container and port of your replacement task set where traffic is to be routed when a new deployment is created\. The container name must be one of the container names referenced in your task definition\. If the network configuration or platform version has been updated in the service definition, you must also specify those details in the AppSpec file\. You can also specify the Lambda functions to run during the deployment lifecycle events\. The Lambda functions allow you to run tests and return metrics during the deployment\. For more information, see [AppSpec File Reference](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html) in the *AWS CodeDeploy User Guide*\.

## Blue/Green Deployment Considerations<a name="deployment-type-bluegreen-considerations"></a>

Consider the following when using the blue/green deployment type:
+ When an Amazon ECS service using the blue/green deployment type is initially created, an Amazon ECS task set is created\.
+ You must configure the service to use either an Application Load Balancer or Network Load Balancer\. Classic Load Balancers aren't supported\. The following are the load balancer requirements:
  + You must add a production listener to the load balancer, which is used to route production traffic\.
  + An optional test listener can be added to the load balancer, which is used to route test traffic\. If you specify a test listener, CodeDeploy routes your test traffic to the replacement task set during a deployment\.
  + Both the production and test listeners must belong to the same load balancer\.
  + You must define a target group for the load balancer\. The target group routes traffic to the original task set in a service through the production listener\.
+ Amazon ECS service auto scaling is supported when using the blue/green deployment type\. When a new deployment is started, if there are scaling activities occurring then CodeDeploy will wait 5 minutes before starting the deployment\. If the scaling activities complete within 5 minutes, the deployment will continue\. If the scaling activities do not complete within 5 minutes, the deployment will fail\.
+ Cluster capacity providers are not supported when using the blue/green deployment type\.
+ Tasks using the Fargate launch type or the `CODE_DEPLOY` deployment controller types don't support the `DAEMON` scheduling strategy\.
+ When you initially create a CodeDeploy application and deployment group, you must specify the following:
  + You must define two target groups for the load balancer\. One target group should be the initial target group defined for the load balancer when the Amazon ECS service was created\. The second target group's only requirement is that it can't be associated with a different load balancer than the one the service uses\.
+ When you create a CodeDeploy deployment for an Amazon ECS service, CodeDeploy creates a *replacement task set* \(or *green task set*\) in the deployment\. If you added a test listener to the load balancer, CodeDeploy routes your test traffic to the replacement task set\. This is when you can run any validation tests\. Then CodeDeploy reroutes the production traffic from the original task set to the replacement task set according to the traffic rerouting settings for the deployment group\.

## Amazon ECS Console Experience<a name="deployment-type-bluegreen-console"></a>

The service create and service update workflows in the Amazon ECS console supports blue/green deployments\.

To create an Amazon ECS service that uses the blue/green deployment type, see [Creating a service](create-service.md)\.

To update an existing Amazon ECS service that is using the blue/green deployment type, see [Updating a service](update-service.md)\.

When you use the Amazon ECS console to create an Amazon ECS service using the blue/green deployment type, an Amazon ECS task set and the following CodeDeploy resources are created automatically with the following default settings\. 


| Resource | Default Setting | 
| --- | --- | 
|  Application name  |  `AppECS-<cluster[:47]>-<service[:47]>`  | 
|  Deployment group name  |  `DgpECS-<cluster[:47]>-<service[:47]>`  | 
|  Deployment group load balancer info  |  The load balancer production listener, optional test listener, and target groups specified are added to the deployment group configuration\.  | 
|  Traffic rerouting settings  |  Traffic rerouting – The default setting is **Reroute traffic immediately**\. You can change it on the CodeDeploy console or by updating the `TrafficRoutingConfig`\. For more information, see [CreateDeploymentConfig](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_CreateDeploymentConfig.html) in the *AWS CodeDeploy API Reference*\.  | 
|  Original revision termination settings  |  The original revision termination settings are configured to wait 1 hour after traffic has been rerouted before terminating the blue task set\.  | 
|  Deployment configuration  |  The deployment configuration is set to `CodeDeployDefault.ECSAllAtOnce` by default, which routes all traffic at one time from the blue task set to the green task set\. The deployment configuration can be changed using the AWS CodeDeploy console after the service is created\.  | 
|  Automatic rollback configuration  |  If a deployment fails, the automatic rollback settings are configured to roll it back\.  | 

To view details of an Amazon ECS service using the blue/green deployment type, use the **Deployments** tab on the Amazon ECS console\.

To view the details of a CodeDeploy deployment group in the CodeDeploy console, see [View Deployment Group Details with CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-view-details.html) in the *AWS CodeDeploy User Guide*\.

To modify the settings for a CodeDeploy deployment group in the CodeDeploy console, see [Change Deployment Group Settings with CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-edit.html) in the *AWS CodeDeploy User Guide*\.

A tutorial walking through the steps needed to create a service using the blue/green deployment type is provided\. For more information, see [Tutorial: Creating a service using a blue/green deployment](create-blue-green.md)\.

Support for performing a blue/green deployment has been added for AWS CloudFormation\. For more information, see [Perform Amazon ECS blue/green deployments through CodeDeploy using AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/blue-green.html) in the *AWS CloudFormation User Guide*\.

## Blue/Green Deployment Required IAM Permissions<a name="deployment-type-bluegreen-IAM"></a>

Amazon ECS blue/green deployments are made possible by a combination of the Amazon ECS and CodeDeploy APIs\. IAM users must have the appropriate permissions for these services before they can use Amazon ECS blue/green deployments in the AWS Management Console or with the AWS CLI or SDKs\. 

In addition to the standard IAM permissions for creating and updating services, Amazon ECS requires the following permissions\. These permissions have been added to the `AmazonECS_FullAccess` IAM policy\. For more information, see [AmazonECS\_FullAccess](ecs_managed_policies.md#AmazonECS_FullAccess)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codedeploy:CreateApplication",
        "codedeploy:CreateDeployment",
        "codedeploy:CreateDeploymentGroup",
        "codedeploy:GetApplication",
        "codedeploy:GetDeployment",
        "codedeploy:GetDeploymentGroup",
        "codedeploy:ListApplications",
        "codedeploy:ListDeploymentGroups",
        "codedeploy:ListDeployments",
        "codedeploy:StopDeployment",
        "codedeploy:GetDeploymentTarget",
        "codedeploy:ListDeploymentTargets",
        "codedeploy:GetDeploymentConfig",
        "codedeploy:GetApplicationRevision",
        "codedeploy:RegisterApplicationRevision",
        "codedeploy:BatchGetApplicationRevisions",
        "codedeploy:BatchGetDeploymentGroups",
        "codedeploy:BatchGetDeployments",
        "codedeploy:BatchGetApplications",
        "codedeploy:ListApplicationRevisions",
        "codedeploy:ListDeploymentConfigs",
        "codedeploy:ContinueDeployment",
        "sns:ListTopics",
        "cloudwatch:DescribeAlarms",
        "lambda:ListFunctions"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

**Note**  
In addition to the standard Amazon ECS permissions required to run tasks and services, IAM users also require `iam:PassRole` permissions to use IAM roles for tasks\.

CodeDeploy needs permissions to call Amazon ECS APIs, modify your Elastic Load Balancing, invoke Lambda functions, and describe CloudWatch alarms, as well as permissions to modify your service's desired count on your behalf\. Before creating an Amazon ECS service that uses the blue/green deployment type, you must create an IAM role \(`ecsCodeDeployRole`\)\. For more information, see [Amazon ECS CodeDeploy IAM Role](codedeploy_IAM_role.md)\.

The [Create Service Example](security_iam_id-based-policy-examples.md#IAM_create_service_policies) and [Update Service Example](security_iam_id-based-policy-examples.md#IAM_update_service_policies) IAM policy examples show the permissions that are required for IAM users to use Amazon ECS blue/green deployments on the AWS Management Console\.