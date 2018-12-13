# Amazon ECS Deployment Types<a name="deployment-types"></a>

An Amazon ECS deployment type determines the deployment strategy that your service uses\. There are two deployment types:

**Topics**
+ [Rolling Update](#deployment-type-ecs)
+ [Blue/Green Deployment with AWS CodeDeploy](#deployment-type-bluegreen)

## Rolling Update<a name="deployment-type-ecs"></a>

The **rolling update** deployment type is controlled by Amazon ECS\. This involves the service scheduler replacing the current running version of the container with the latest version\. The number of tasks Amazon ECS adds or removes from the service during a rolling update is controlled by the deployment configuration\. A deployment configuration consists of the minimum and maximum number of tasks allowed during a service deployment\.

To create a new Amazon ECS service that uses the rolling update deployment type, see [Creating a Service](create-service.md)\.

## Blue/Green Deployment with AWS CodeDeploy<a name="deployment-type-bluegreen"></a>

The **blue/green** deployment type uses the blue/green deployment model controlled by AWS CodeDeploy\. This deployment type allows you to verify a new deployment of a service before sending production traffic to it\. For more information, see [What Is AWS CodeDeploy?](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) in the *AWS CodeDeploy User Guide*\.

The following are components of AWS CodeDeploy that Amazon ECS uses when a service uses the blue/green deployment type:

**AWS CodeDeploy application**  
A collection of AWS CodeDeploy resources\. This consists of one or more deployment groups\.

**AWS CodeDeploy deployment group**  
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

**AWS CodeDeploy deployment configuration**  
Specifies how AWS CodeDeploy routes production traffic to your replacement task set during a deployment\. The only supported value at this time is `CodeDeployDefault.AllAtOnce`, which means all traffic is routed from the original task set to the replacement task set at the same time\. For more information, see [Working with Deployment Configurations](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html) in the *AWS CodeDeploy User Guide*\.

**Revision**  
A revision is the AWS CodeDeploy application specification file \(AppSpec file\)\. In the AppSpec file, you specify the full ARN of the task definition and the container and port of your replacement task set where traffic is to be routed when a new deployment is created\. The container name must be one of the container names referenced in your task definition\. If the network configuration or platform version has been updated in the service definition, you must also specify those details in the AppSec file\. You can also specify the Lambda functions to run during the deployment lifecycle events\. The Lambda functions allow you to run tests and return metrics during the deployment\. For more information, see [AppSpec File Reference](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html) in the *AWS CodeDeploy User Guide*\.

### Blue/Green Deployment Considerations<a name="deployment-type-bluegreen-considerations"></a>

There are several things to consider when using the blue/green deployment type:
+ When an Amazon ECS service using the blue/green deployment type is initially created, an Amazon ECS task set is created\.
+ The service must be configured to use either an Application Load Balancer or Network Load Balancer\. Classic Load Balancers are not supported\. The following are the load balancer requirements:
  + A production listener must be added to the load balancer, which is used to route production traffic\.
  + An optional test listener can be added to the load balancer, which is used to route test traffic\. If you specify a test listener, AWS CodeDeploy routes your test traffic to the replacement task set during a deployment\.
  + Both the production and test listeners must belong to the same load balancer\.
  + A target group must be defined for the load balancer\. The target group routes traffic to the original task set in a service through the production listener\.
+ When an AWS CodeDeploy application and deployment group are initially created, the following must be specified:
  + Two target groups must be defined for the load balancer\. One target group should be the initial target group defined for the load balancer when the Amazon ECS service was created\. The second target group's only requirement is that it cannot be associated with a different load balancer than the one the service uses\.
+ When an AWS CodeDeploy deployment has been created for an Amazon ECS service, AWS CodeDeploy creates a *replacement task set* \(or *green task set*\) in the deployment\. If a test listener was added to the load balancer, AWS CodeDeploy routes your test traffic to the replacement task set\. This is when any validation tests can be run\. Then, AWS CodeDeploy reroutes the production traffic from the original task set to the replacement task set according to the traffic rerouting settings for the deployment group\.

### Amazon ECS Console Experience<a name="deployment-type-bluegreen-console"></a>

The service create and service update workflows in the Amazon ECS console supports blue/green deployments\.

To create a new Amazon ECS service that uses the blue/green deployment type, see [Creating a Service](create-service.md)\.

To update an existing Amazon ECS service that is using the blue/green deployment type, see [Updating a Service](update-service.md)\.

When an Amazon ECS service using the blue/green deployment type is initially created using the Amazon ECS console, an Amazon ECS task set and the following AWS CodeDeploy resources are created automatically with the following default settings\. 


| Resource | Default setting | 
| --- | --- | 
|  Application name  |  `AppECS-<cluster[:47]>-<service[:47]>`  | 
|  Deployment group name  |  `DgpECS-<cluster[:47]>-<service[:47]>`  | 
|  Deployment group load balancer info  |  The load balancer production listener, optional test listener, and target groups specified are added to the deployment group configuration\.  | 
|  Traffic rerouting settings  |  Traffic rerouting – The default is set to **Reroute traffic immediately**\. It can be changed in the AWS CodeDeploy console or by updating the `TrafficRoutingConfig`\. For more information, see [CreateDeploymentConfig](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_CreateDeploymentConfig.html) in the *AWS CodeDeploy API Reference*\.  | 
|  Original revision termination settings  |  The original revision termination settings are configured to wait 1 hour after traffic has been rerouted before terminating the blue task set\.  | 
| Deployment configuration | The deployment configuration is set to CodeDeployDefault\.AllAtOnce, which routes all traffic at one time from the blue task set to the green task set\. This setting cannot be changed\. | 
| Automatic rollback configuration | If a deployment fails, the automatic rollback settings are configured to roll it back\. | 

To view details of an Amazon ECS service using the blue/green deployment type, use the **Deployments** tab in the Amazon ECS console\.

To view the details of an AWS CodeDeploy deployment group in the AWS CodeDeploy console, see [View Deployment Group Details with AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-view-details.html) in the *AWS CodeDeploy User Guide*\.

To modify the settings for an AWS CodeDeploy deployment group in the AWS CodeDeploy console, see [Change Deployment Group Settings with AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-edit.html) in the *AWS CodeDeploy User Guide*\.

### Blue/Green Deployment Required IAM Permissions<a name="deployment-type-bluegreen-IAM"></a>

Amazon ECS blue/green deployments are made possible by a combination of the Amazon ECS and AWS CodeDeploy APIs\. IAM users must have the appropriate permissions for these services before they can use Amazon ECS blue/green deployments in the AWS Management Console or with the AWS CLI or SDKs\. 

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
        "codedpeloy:ContinueDeployment",
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

AWS CodeDeploy needs permission to call Amazon ECS APIs, modify your Elastic Load Balancing, invoke Lambda functions, and describe CloudWatch alarms, as well as permissions to modify your service's desired count on your behalf\. Before creating an Amazon ECS service that uses the blue/green deployment type, you must create an IAM role \(`ecsCodeDeployRole`\)\. For more information, see [Amazon ECS AWS CodeDeploy IAM Role](codedeploy_IAM_role.md)\.

The [Create Services](IAMPolicyExamples.md#IAM_create_service_policies) and [Update Services](IAMPolicyExamples.md#IAM_update_service_policies) IAM policy examples show the permissions that are required for IAM users to use Amazon ECS blue/green deployments in the AWS Management Console\.