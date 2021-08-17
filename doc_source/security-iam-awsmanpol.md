# AWS managed policies for Amazon Elastic Container Service<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

Amazon ECS and Amazon ECR provide several managed policies and trust relationships that you can attach to AWS Identity and Access Management \(IAM\) users, Amazon EC2 instances, and Amazon ECS tasks that allow differing levels of control over resources and API operations\. You can apply these policies directly, or you can use them as starting points for creating your own policies\. For more information about the Amazon ECR managed policies, see [Amazon ECR managed policies](AmazonECR/latest/userguide/ecr_managed_policies.html)\.

## AmazonECS\_FullAccess<a name="security-iam-awsmanpol-AmazonECS_FullAccess"></a>

You can attach the `AmazonECS_FullAccess` policy to your IAM identities\.

This policy grants administrative access to Amazon ECS resources and grants an IAM identity \(such as a user, group, or role\) access to the AWS services that Amazon ECS is integrated with to use all of Amazon ECS features\. Using this policy allows access to all of Amazon ECS features that are available in the AWS Management Console\.

### Permissions details<a name="security-iam-awsmanpol-AmazonECS_FullAccess-permissions"></a>

The `AmazonECS_FullAccess` managed IAM policy includes the following permissions\. Following the best practice of granting least privilege, you can use the `AmazonECS_FullAccess` managed policy as a template for creating you own custom policy\. That way, you can take away or add permissions to and from the managed policy based on your specific requirements\.
+ `ecs` – Allows principals full access to all Amazon ECS APIs\.
+ `application-autoscaling` – Allows principals to create, describe, and manage Application Auto Scaling resources\. This is required when enabling service auto scaling for your Amazon ECS services\.
+ `appmesh` – Allows principals to list App Mesh service meshes and virtual nodes and describe App Mesh virtual nodes\. This is required when integrating your Amazon ECS services with App Mesh\.
+ `autoscaling` – Allows principals to create, manage, and describe Amazon EC2 Auto Scaling resources\. This is required when managing Amazon EC2 auto scaling groups when using the cluster auto scaling feature\.
+ `cloudformation` – Allows principals to create and manage AWS CloudFormation stacks\. This is required when creating Amazon ECS clusters using the AWS Management Console and the subsequent managing of those clusters\.
+ `cloudwatch` – Allows principals to create, manage, and describe Amazon CloudWatch alarms\.
+ `codedeploy` – Allows principals to create and manage application deployments as well as view their configurations, revisions, and deployment targets\.
+ `sns` – Allows principals to view a list of Amazon SNS topics\.
+ `lambda` – Allows principals to view a list of AWS Lambda functions and their version specific configurations\.
+ `ec2` – Allows principals run Amazon EC2 instances as well as create and manage routes, route tables, internet gateways, launch groups, security groups, virtual private clouds, spot fleets, and subnets\.
+ `elasticloadbalancing` – Allows principals to create, describe, and delete Elastic Load Balancing load balancers\. Principals will also be able to fully manage the target groups, listeners, and listener rules for load balancers\.
+ `events` – Allows principals to create, manage, and delete Amazon EventBridge rules and their targets\.
+ `iam`– Allows principals to list IAM roles and their attached policies\. Principals can also list instance profiles available to your Amazon EC2 instances\.
+ `logs` – Allows principals to create and describe Amazon CloudWatch Logs log groups\. Principals can also list log events for these log groups\.
+ `route53` – Allows principals to create, manage, and delete Amazon Route 53 hosted zones\. Principals can also view Amazon Route 53 health check configuration and information\. For more information about hosted zones, see [Working with hosted zones](/Route53/latest/DeveloperGuide/hosted-zones-working-with.html)\.
+ `servicediscovery` – Allows principals to create, manage, and delete AWS Cloud Map services and create private DNS namespaces\.

The following is an example `AmazonECS_FullAccess` policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:DeleteScalingPolicy",
                "application-autoscaling:DeregisterScalableTarget",
                "application-autoscaling:DescribeScalableTargets",
                "application-autoscaling:DescribeScalingActivities",
                "application-autoscaling:DescribeScalingPolicies",
                "application-autoscaling:PutScalingPolicy",
                "application-autoscaling:RegisterScalableTarget",
                "appmesh:ListMeshes",
                "appmesh:ListVirtualNodes",
                "appmesh:DescribeVirtualNode",
                "autoscaling:UpdateAutoScalingGroup",
                "autoscaling:CreateAutoScalingGroup",
                "autoscaling:CreateLaunchConfiguration",
                "autoscaling:DeleteAutoScalingGroup",
                "autoscaling:DeleteLaunchConfiguration",
                "autoscaling:Describe*",
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStack*",
                "cloudformation:UpdateStack",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:PutMetricAlarm",
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
                "lambda:ListFunctions",
                "ec2:AssociateRouteTable",
                "ec2:AttachInternetGateway",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CancelSpotFleetRequests",
                "ec2:CreateInternetGateway",
                "ec2:CreateLaunchTemplate",
                "ec2:CreateRoute",
                "ec2:CreateRouteTable",
                "ec2:CreateSecurityGroup",
                "ec2:CreateSubnet",
                "ec2:CreateVpc",
                "ec2:DeleteLaunchTemplate",
                "ec2:DeleteSubnet",
                "ec2:DeleteVpc",
                "ec2:Describe*",
                "ec2:DetachInternetGateway",
                "ec2:DisassociateRouteTable",
                "ec2:ModifySubnetAttribute",
                "ec2:ModifyVpcAttribute",
                "ec2:RunInstances",
                "ec2:RequestSpotFleet",
                "elasticloadbalancing:CreateListener",
                "elasticloadbalancing:CreateLoadBalancer",
                "elasticloadbalancing:CreateRule",
                "elasticloadbalancing:CreateTargetGroup",
                "elasticloadbalancing:DeleteListener",
                "elasticloadbalancing:DeleteLoadBalancer",
                "elasticloadbalancing:DeleteRule",
                "elasticloadbalancing:DeleteTargetGroup",
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:DescribeTargetGroups",
                "ecs:*",
                "events:DescribeRule",
                "events:DeleteRule",
                "events:ListRuleNamesByTarget",
                "events:ListTargetsByRule",
                "events:PutRule",
                "events:PutTargets",
                "events:RemoveTargets",
                "iam:ListAttachedRolePolicies",
                "iam:ListInstanceProfiles",
                "iam:ListRoles",
                "logs:CreateLogGroup",
                "logs:DescribeLogGroups",
                "logs:FilterLogEvents",
                "route53:GetHostedZone",
                "route53:ListHostedZonesByName",
                "route53:CreateHostedZone",
                "route53:DeleteHostedZone",
                "route53:GetHealthCheck",
                "servicediscovery:CreatePrivateDnsNamespace",
                "servicediscovery:CreateService",
                "servicediscovery:GetNamespace",
                "servicediscovery:GetOperation",
                "servicediscovery:GetService",
                "servicediscovery:ListNamespaces",
                "servicediscovery:ListServices",
                "servicediscovery:UpdateService",
                "servicediscovery:DeleteService"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParametersByPath",
                "ssm:GetParameters",
                "ssm:GetParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/aws/service/ecs*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DeleteInternetGateway",
                "ec2:DeleteRoute",
                "ec2:DeleteRouteTable",
                "ec2:DeleteSecurityGroup"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "ec2:ResourceTag/aws:cloudformation:stack-name": "EC2ContainerService-*"
                }
            }
        },
        {
            "Action": "iam:PassRole",
            "Effect": "Allow",
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "ecs-tasks.amazonaws.com"
                }
            }
        },
        {
            "Action": "iam:PassRole",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iam::*:role/ecsInstanceRole*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": [
                        "ec2.amazonaws.com",
                        "ec2.amazonaws.com.cn"
                    ]
                }
            }
        },
        {
            "Action": "iam:PassRole",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:iam::*:role/ecsAutoscaleRole*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": [
                        "application-autoscaling.amazonaws.com",
                        "application-autoscaling.amazonaws.com.cn"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": [
                        "ecs.amazonaws.com",
                        "spot.amazonaws.com",
                        "spotfleet.amazonaws.com",
                        "ecs.application-autoscaling.amazonaws.com",
                        "autoscaling.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

## AmazonEC2ContainerServiceforEC2Role<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceforEC2Role"></a>

Amazon ECS attaches this policy to a service role that allows Amazon ECS to perform actions on your behalf against Amazon EC2 instances or external instances\.

This policy grants administrative permissions that allow Amazon ECS container instances to make calls to AWS on your behalf\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

### Considerations<a name="instance-iam-role-considerations"></a>

You should consider the following recommendations and considerations when using the `AmazonEC2ContainerServiceforEC2Role` managed IAM policy\.
+ Following the standard security advice of granting least privilege, you can modify the `AmazonEC2ContainerServiceforEC2Role` managed policy to fit your specific needs\. If any of the permissions granted in the managed policy aren't needed for your use case, create a custom policy and add only the permissions that you require\. For example, the `UpdateContainerInstancesState` permission is provided for Spot Instance draining\. If that permission isn't needed for your use case, exclude it using a custom policy\. For more information, see [Permissions details](#instance-iam-role-permissions)\.
+ Containers that are running on your container instances have access to all of the permissions that are supplied to the container instance role through [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\. We recommend that you limit the permissions in your container instance role to the minimal list of permissions that are provided in the managed `AmazonEC2ContainerServiceforEC2Role` policy\. If the containers in your tasks need extra permissions that aren't listed, we recommend providing those tasks with their own IAM roles\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

  You can prevent containers on the `docker0` bridge from accessing the permissions supplied to the container instance role\. You can do this while still allowing the permissions that are provided by [IAM Roles for Tasks](task-iam-roles.md) by running the following iptables command on your container instances\. Containers can't query instance metadata with this rule in effect\. This command assumes the default Docker bridge configuration and it doesn't work with containers that use the `host` network mode\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.

  ```
  sudo yum install -y iptables-services; sudo iptables --insert FORWARD 1 --in-interface docker+ --destination 169.254.169.254/32 --jump DROP
  ```

  You must save this iptables rule on your container instance for it to survive a reboot\. For the Amazon ECS\-optimized AMI, use the following command\. For other operating systems, consult the documentation for that OS\.
  + For the Amazon ECS\-optimized Amazon Linux 2 AMI:

    ```
    sudo iptables-save | sudo tee /etc/sysconfig/iptables && sudo systemctl enable --now iptables
    ```
  + For the Amazon ECS\-optimized Amazon Linux AMI:

    ```
    sudo service iptables save
    ```

### Permissions details<a name="instance-iam-role-permissions"></a>

The `AmazonEC2ContainerServiceforEC2Role` managed IAM policy includes the following permissions\. Following the standard security advice of granting least privilege, the `AmazonEC2ContainerServiceforEC2Role` managed policy can be used as a guide\. If you don't need any of the permissions that are granted in the managed policy for your use case, create a custom policy and add only the permissions that you need\.
+ `ec2:DescribeTags` – Allows a principal to describe the tags that are associated with an Amazon EC2 instance\. This permission is used by the Amazon ECS container agent to support resource tag propagation\. For more information, see [Tagging your resources](ecs-using-tags.md#tag-resources)\.
+ `ecs:CreateCluster` – Allows a principal to create an Amazon ECS cluster\. This permission is used by the Amazon ECS container agent to create a `default` cluster, if one doesn't already exist\.
+ `ecs:DeregisterContainerInstance` – Allows a principal to deregister an Amazon ECS container instance from a cluster\. The Amazon ECS container agent doesn't call this API, but this permission remains to ensure backwards compatibility\.
+ `ecs:DiscoverPollEndpoint` – This action returns endpoints that the Amazon ECS container agent uses to poll for updates\.
+ `ecs:Poll` – Allows the Amazon ECS container agent to communicate with the Amazon ECS control plane to report task state changes\.
+ `ecs:RegisterContainerInstance` – Allows a principal to register a container instance with a cluster\. This permission is used by the Amazon ECS container agent to register the Amazon EC2 instance with a cluster as well as to support resource tag propagation\.
+ `ecs:StartTelemetrySession` – Allows the Amazon ECS container agent to communicate with the Amazon ECS control plane to report health information and metrics for each container and task\.
+ `ecs:UpdateContainerInstancesState` – Allows a principal to modify the status of an Amazon ECS container instance\. This permission is used by the Amazon ECS container agent for Spot Instance draining\. For more information, see [Spot Instance Draining](container-instance-spot.md#spot-instance-draining)\.
+ `ecs:Submit*` – This includes the `SubmitAttachmentStateChanges`, `SubmitContainerStateChange`, and `SubmitTaskStateChange` API actions\. They're used by the Amazon ECS container agent to report state changes for each resource to the Amazon ECS control plane\. The `SubmitContainerStateChange` permission is no longer used by the Amazon ECS container agent but remains to ensure backwards compatibility\.
+ `ecr:GetAuthorizationToken` – Allows a principal to retrieve an authorization token\. The authorization token represents your IAM authentication credentials and can be used to access any Amazon ECR registry that the IAM principal has access to\. The authorization token received is valid for 12 hours\.
+ `ecr:BatchCheckLayerAvailability` – When a container image is pushed to an Amazon ECR private repository, each image layer is checked to verify if it's already pushed\. If it is, then the image layer is skipped\.
+ `ecr:GetDownloadUrlForLayer` – When a container image is pulled from an Amazon ECR private repository, this API is called once for each image layer that's not already cached\.
+ `ecr:BatchGetImage` – When a container image is pulled from an Amazon ECR private repository, this API is called once to retrieve the image manifest\.
+ `logs:CreateLogStream` – Allows a principal to create a CloudWatch Logs log stream for a specified log group\.
+ `logs:PutLogEvents` – Allows a principal to upload a batch of log events to a specified log stream\.

The following is an example `AmazonEC2ContainerServiceforEC2Role` policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeTags",
                "ecs:CreateCluster",
                "ecs:DeregisterContainerInstance",
                "ecs:DiscoverPollEndpoint",
                "ecs:Poll",
                "ecs:RegisterContainerInstance",
                "ecs:StartTelemetrySession",
                "ecs:UpdateContainerInstancesState",
                "ecs:Submit*",
                "ecr:GetAuthorizationToken",
                "ecr:BatchCheckLayerAvailability",
                "ecr:GetDownloadUrlForLayer",
                "ecr:BatchGetImage",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

## AmazonEC2ContainerServiceEventsRole<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceEventsRole"></a>

This policy grants permissions that allow Amazon EventBridge \(formerly CloudWatch Events\) to run tasks on your behalf\. This policy can be attached to the IAM role that's specified when you create scheduled tasks\. For more information, see [Amazon ECS CloudWatch Events IAM Role](CWE_IAM_role.md)\.

**Permissions details**

This policy includes the following permissions\.
+ `ecs` – Allows a principal in a service to call the Amazon ECS RunTask API\.
+ `iam` – Allows passing any IAM service role to any Amazon ECS tasks\.

The following is an example `AmazonEC2ContainerServiceEventsRole` policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:RunTask"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringLike": {
                    "iam:PassedToService": "ecs-tasks.amazonaws.com"
                }
            }
        }
    ]
}
```

## AmazonECSTaskExecutionRolePolicy<a name="security-iam-awsmanpol-AmazonECSTaskExecutionRolePolicy"></a>

The `AmazonECSTaskExecutionRolePolicy` managed IAM policy grants the permissions that are needed by the Amazon ECS container agent and AWS Fargate container agents to make AWS API calls on your behalf\. This policy can be added to your task execution IAM role\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

### Permissions details<a name="taskexecution-iam-role-permissions"></a>

The `AmazonECSTaskExecutionRolePolicy` managed IAM policy includes the following permissions\. Following the standard security advice of granting least privilege, the `AmazonECSTaskExecutionRolePolicy` managed policy can be used as a guide\. If any of the permissions that are granted in the managed policy aren't needed for your use case, create a custom policy and add only the permissions that you require\.
+ `ecr:GetAuthorizationToken` – Allows a principal to retrieve an authorization token\. The authorization token represents your IAM authentication credentials and can be used to access any Amazon ECR registry that the IAM principal has access to\. The authorization token received is valid for 12 hours\.
+ `ecr:BatchCheckLayerAvailability` – When a container image is pushed to an Amazon ECR private repository, each image layer is checked to verify if it's already pushed\. If it's pushed, then the image layer is skipped\.
+ `ecr:GetDownloadUrlForLayer` – When a container image is pulled from an Amazon ECR private repository, this API is called once for each image layer that's not already cached\.
+ `ecr:BatchGetImage` – When a container image is pulled from an Amazon ECR private repository, this API is called once to retrieve the image manifest\.
+ `logs:CreateLogStream` – Allows a principal to create a CloudWatch Logs log stream for a specified log group\.
+ `logs:PutLogEvents` – Allows a principal to upload a batch of log events to a specified log stream\.

The following is an example `AmazonECSTaskExecutionRolePolicy` policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ecr:GetAuthorizationToken",
        "ecr:BatchCheckLayerAvailability",
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "*"
    }
  ]
}
```

## `AWSApplicationAutoscalingECSServicePolicy`<a name="security-iam-awsmanpol-AWSApplicationAutoscalingECSServicePolicy"></a>

You can't attach `AWSApplicationAutoscalingECSServicePolicy` to your IAM entities\. This policy is attached to a service\-linked role that allows Application Auto Scaling to perform actions on your behalf\. For more information, see [Service\-linked roles for Application Auto Scaling](/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html)\.

## `AWSCodeDeployRoleForECS`<a name="security-iam-awsmanpol-AWSCodeDeployRoleForECS"></a>

You can't attach `AWSCodeDeployRoleForECS` to your IAM entities\. This policy is attached to a service\-linked role that allows CodeDeploy to perform actions on your behalf\. For more information, see [Create a service role for CodeDeploy](/codedeploy/latest/userguide/getting-started-create-service-role.html)\.

## `AWSCodeDeployRoleForECSLimited`<a name="security-iam-awsmanpol-AWSCodeDeployRoleForECSLimited"></a>

You can't attach `AWSCodeDeployRoleForECSLimited` to your IAM entities\. This policy is attached to a service\-linked role that allows CodeDeploy to perform actions on your behalf\. For more information, see [Create a service role for CodeDeploy](/codedeploy/latest/userguide/getting-started-create-service-role.html)\.

## Amazon ECS updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for Amazon ECS since this service started tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon ECS Document history page\.

 


| Change | Description | Date | 
| --- | --- | --- | 
|  Amazon ECS started tracking changes  |  Amazon ECS started tracking changes for its AWS managed policies\.  | June 8, 2021 | 