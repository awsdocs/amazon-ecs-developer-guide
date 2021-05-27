# Amazon ECS Managed Policies and Trust Relationships<a name="ecs_managed_policies"></a>

Amazon ECS provides several managed policies and trust relationships that you can attach to IAM users, EC2 instances, or Amazon ECS tasks that allow differing levels of control over Amazon ECS resources and API operations\. You can apply these policies directly, or you can use them as starting points for creating your own policies\. For more information about each API operation mentioned in these policies, see [Actions](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_Operations.html) in the *Amazon Elastic Container Service API Reference*\.

**Topics**
+ [AmazonECS\_FullAccess](#AmazonECS_FullAccess)
+ [AmazonEC2ContainerServiceFullAccess](#AmazonEC2ContainerServiceFullAccess)
+ [AmazonEC2ContainerServiceforEC2Role](#AmazonEC2ContainerServiceforEC2Role)
+ [AmazonECSTaskExecutionRolePolicy](#AmazonECSTaskExecutionRolePolicy)
+ [AmazonEC2ContainerServiceRole](#AmazonEC2ContainerServiceRole)
+ [AmazonEC2ContainerServiceAutoscaleRole](#AmazonEC2ContainerServiceAutoscaleRole)
+ [AmazonEC2ContainerServiceTaskRole](#AmazonEC2ContainerServiceTaskRole)
+ [AmazonEC2ContainerServiceEventsRole](#AmazonEC2ContainerServiceEventsRole)

## AmazonECS\_FullAccess<a name="AmazonECS_FullAccess"></a>

This managed policy provides administrative access to Amazon ECS resources and enables ECS features through access to other AWS service resources, including VPCs, Auto Scaling groups, and AWS CloudFormation stacks\.

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

## AmazonEC2ContainerServiceFullAccess<a name="AmazonEC2ContainerServiceFullAccess"></a>

**Important**  
The `AmazonEC2ContainerServiceFullAccess` managed IAM policy is deprecated as of January 29, 2021 in response to a security finding with the `iam:passRole` permission which grants access to all resources including credentials to roles in the account\. Once the policy is deprecated, you won't be able to attach the policy to any new IAM users or roles\. Any existing users or roles that have the policy attached will be able to continue using it, however we recommend updating your IAM users or roles to use the `AmazonECS_FullAccess` managed policy instead\.

This managed policy allows full administrator access to Amazon ECS\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "autoscaling:Describe*",
                "autoscaling:UpdateAutoScalingGroup",
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStack*",
                "cloudformation:UpdateStack",
                "cloudwatch:GetMetricStatistics",
                "ec2:Describe*",
                "elasticloadbalancing:*",
                "ecs:*",
                "events:DescribeRule",
                "events:DeleteRule",
                "events:ListRuleNamesByTarget",
                "events:ListTargetsByRule",
                "events:PutRule",
                "events:PutTargets",
                "events:RemoveTargets",
                "iam:ListInstanceProfiles",
                "iam:ListRoles",
                "iam:PassRole"
            ],
            "Resource": "*"
        }
    ]
}
```

## AmazonEC2ContainerServiceforEC2Role<a name="AmazonEC2ContainerServiceforEC2Role"></a>

Amazon ECS container instances run the container agent and thus require an IAM policy and role for the service to know that the agent belongs to you\. Before you launch container instances and register them to a cluster, you must create an IAM role for those container instances to use\. This requirement applies to all container instances you register with an Amazon ECS cluster\.

The `AmazonEC2ContainerServiceforEC2Role` managed IAM policy was created with the least amount of permissions needed to use the full Amazon ECS feature set\. This managed policy can be attached to an IAM role and associated with your Amazon ECS container instances\. This policy provides permissions needed for the Amazon ECS container agent and Docker daemon to call AWS APIs on your behalf\.

### Considerations<a name="instance-iam-role-considerations"></a>

The following should be considered when using the `AmazonEC2ContainerServiceforEC2Role` managed IAM policy\.
+ Following the standard security advice of granting least privilege, the `AmazonEC2ContainerServiceforEC2Role` managed policy can be used as a guide\. If any of the permissions granted in the managed policy aren't needed for your use case, create a custom policy and add only the permissions you require\. For example, the `UpdateContainerInstancesState` permission is provided for Spot Instance draining\. If that permission is not needed for your use case, you can exclude it using a custom policy\. For more information, see [Permissions details](#instance-iam-role-permissions)\.
+ Containers that are running on your container instances have access to all of the permissions that are supplied to the container instance role through [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\. We recommend that you limit the permissions in your container instance role to the minimal list of permissions provided in the managed `AmazonEC2ContainerServiceforEC2Role` policy shown below\. If the containers in your tasks need extra permissions that are not listed here, we recommend providing those tasks with their own IAM roles\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

  You can prevent containers on the `docker0` bridge from accessing the permissions supplied to the container instance role \(while still allowing the permissions that are provided by [IAM Roles for Tasks](task-iam-roles.md)\) by running the following iptables command on your container instances; however, containers will not be able to query instance metadata with this rule in effect\. Note that this command assumes the default Docker bridge configuration and it will not work for containers that use the `host` network mode\. For more information, see [Network mode](task_definition_parameters.md#network_mode)\.

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

The `AmazonEC2ContainerServiceforEC2Role` managed IAM policy includes the following permissions\. Following the standard security advice of granting least privilege, the `AmazonEC2ContainerServiceforEC2Role` managed policy can be used as a guide\. If any of the permissions granted in the managed policy aren't needed for your use case, create a custom policy and add only the permissions you require\.
+ `ec2:DescribeTags` – Allows a principal to describe the tags associated with an Amazon EC2 instance\. This permission is used by the Amazon ECS container agent to support resource tag propagation\. For more information, see [Tagging your resources](ecs-using-tags.md#tag-resources)\.
+ `ecs:CreateCluster` – Allows a principal to create an Amazon ECS cluster\. This permission is used by the Amazon ECS container agent to create a `default` cluster, if one doesn't already exist\.
+ `ecs:DeregisterContainerInstance` – Allows a principal to deregister an Amazon ECS container instance from a cluster\. The Amazon ECS container agent doesn't call this API, but this permission remains to ensure backwards compatibility\.
+ `ecs:DiscoverPollEndpoint` – This action returns endpoints that the Amazon ECS container agent uses to poll for updates\.
+ `ecs:Poll` – Allows the Amazon ECS container agent to communicate with the Amazon ECS control plane to report task state changes\.
+ `ecs:RegisterContainerInstance` – Allows a principal to register a container instance with a cluster\. This permission is used by the Amazon ECS container agent to register the Amazon EC2 instance with a cluster as well as to support resource tag propagation\.
+ `ecs:StartTelemetrySession` – Allows the Amazon ECS container agent to communicate with the Amazon ECS control plane to report health information and metrics for each container and task\.
+ `ecs:UpdateContainerInstancesState` – Allows a principal to modify the status of an Amazon ECS container instance\. This permission is used by the Amazon ECS container agent for Spot Instance draining\. For more information, see [Spot Instance Draining](container-instance-spot.md#spot-instance-draining)\.
+ `ecs:Submit*` – This includes the `SubmitAttachmentStateChanges`, `SubmitContainerStateChange`, and `SubmitTaskStateChange` API actions which are used by the Amazon ECS container agent to report state changes for each resource to the Amazon ECS control plane\. The `SubmitContainerStateChange` permission is no longer used by the Amazon ECS container agent but remains to ensure backwards compatibility\.
+ `ecr:GetAuthorizationToken` – Allows a principal to retrieve an authorization token\. The authorization token represents your IAM authentication credentials and can be used to access any Amazon ECR registry that the IAM principal has access to\. The authorization token received is valid for 12 hours\.
+ `ecr:BatchCheckLayerAvailability` – When a container image is pushed to an Amazon ECR private repository, each image layer is checked to verify if it has already been pushed\. If it has, then the image layer is skipped\.
+ `ecr:GetDownloadUrlForLayer` – When a container image is pulled from an Amazon ECR private repository, this API is called once per image layer that is not already cached\.
+ `ecr:BatchGetImage` – When a container image is pulled from an Amazon ECR private repository, this API is called once to retrieve the image manifest\.
+ `logs:CreateLogStream` – Allows a principal to create a CloudWatch Logs log stream for a specified log group\.
+ `logs:PutLogEvents` – Allows a principal to upload a batch of log events to a specified log stream\.

The `AmazonEC2ContainerServiceforEC2Role` policy is shown below\.

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

### Amazon ECS updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon ECS since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon ECS document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  `AmazonEC2ContainerServiceforEC2Role` – Update to an existing policy  | Amazon ECS added the ec2:DescribeTags permission to the managed policy\. This permission is used by the Amazon ECS container agent to support tag propagation\. |  June 13, 2019  | 
|  `AmazonEC2ContainerServiceforEC2Role` – Update to an existing policy  | Amazon ECS added the ecs:UpdateContainerInstancesState permission to the managed policy\. This permission is used by the Amazon ECS container agent for Spot Instance draining\. |  May 17, 2017  | 
|  `AmazonEC2ContainerServiceforEC2Role` – Update to an existing policy  | Amazon ECS added the logs:CreateLogStream and logs:PutLogEvents permissions to the managed policy\. These permissions allow the Amazon ECS container agent to communicate with CloudWatch Logs to send container logs for tasks that are configured to use the awslogs log driver\. |  May 4, 2016  | 
|  `AmazonEC2ContainerServiceforEC2Role` – Update to an existing policy  | Amazon ECS added the ecr:GetAuthorizationToken, ecr:BatchCheckLayerAvailability, ecr:GetDownloadUrlForLayer, and ecr:BatchGetImages permissions to the managed policy\. This permissions allow the Amazon ECS container agent to communicate with Amazon ECR to pull container images from private repositories\. |  December 18, 2015  | 
|  `AmazonEC2ContainerServiceforEC2Role` – Update to an existing policy  |  Amazon ECS added the `ecs:StartTelemetrySession` permission to the managed policy\. This allows the Amazon ECS container agent to communicate with the Amazon ECS control plane to report health information and metrics for each container and task\.  |  August 17, 2015  | 
|  `AmazonEC2ContainerServiceforEC2Role` – Initial creation  | Amazon ECS created the AmazonEC2ContainerServiceforEC2Role managed policy\. |  March 19, 2015  | 

## AmazonECSTaskExecutionRolePolicy<a name="AmazonECSTaskExecutionRolePolicy"></a>

The `AmazonECSTaskExecutionRolePolicy` managed IAM policy grants the permissions needed by the Amazon ECS container agent and AWS Fargate container agents to make AWS API calls on your behalf\. This policy can be added to your task execution IAM role\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

### Permissions details<a name="taskexecution-iam-role-permissions"></a>

The `AmazonECSTaskExecutionRolePolicy` managed IAM policy includes the following permissions\. Following the standard security advice of granting least privilege, the `AmazonECSTaskExecutionRolePolicy` managed policy can be used as a guide\. If any of the permissions granted in the managed policy aren't needed for your use case, create a custom policy and add only the permissions you require\.
+ `ecr:GetAuthorizationToken` – Allows a principal to retrieve an authorization token\. The authorization token represents your IAM authentication credentials and can be used to access any Amazon ECR registry that the IAM principal has access to\. The authorization token received is valid for 12 hours\.
+ `ecr:BatchCheckLayerAvailability` – When a container image is pushed to an Amazon ECR private repository, each image layer is checked to verify if it has already been pushed\. If it has, then the image layer is skipped\.
+ `ecr:GetDownloadUrlForLayer` – When a container image is pulled from an Amazon ECR private repository, this API is called once per image layer that is not already cached\.
+ `ecr:BatchGetImage` – When a container image is pulled from an Amazon ECR private repository, this API is called once to retrieve the image manifest\.
+ `logs:CreateLogStream` – Allows a principal to create a CloudWatch Logs log stream for a specified log group\.
+ `logs:PutLogEvents` – Allows a principal to upload a batch of log events to a specified log stream\.

The `AmazonECSTaskExecutionRolePolicy` policy is shown below\.

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

### Amazon ECS updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon ECS since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon ECS document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  `AmazonECSTaskExecutionRolePolicy` – Initial creation  | Amazon ECS created the AmazonECSTaskExecutionRolePolicy managed policy\. |  November 16, 2017  | 

## AmazonEC2ContainerServiceRole<a name="AmazonEC2ContainerServiceRole"></a>

This managed policy allows Elastic Load Balancing load balancers to register and deregister Amazon ECS container instances on your behalf\. For more information, see [Service Scheduler IAM Role](ecs-legacy-iam-roles.md#service_IAM_role)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:Describe*",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:DeregisterTargets",
                "elasticloadbalancing:Describe*",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:RegisterTargets"
            ],
            "Resource": "*"
        }
    ]
}
```

## AmazonEC2ContainerServiceAutoscaleRole<a name="AmazonEC2ContainerServiceAutoscaleRole"></a>

This managed policy allows Application Auto Scaling to scale your Amazon ECS service's desired count up and down in response to CloudWatch alarms on your behalf\. For more information, see [Service Auto Scaling IAM Role](ecs-legacy-iam-roles.md#autoscale_IAM_role)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:DescribeServices",
                "ecs:UpdateService"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms",
                "cloudwatch:PutMetricAlarm"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

## AmazonEC2ContainerServiceTaskRole<a name="AmazonEC2ContainerServiceTaskRole"></a>

This IAM trust relationship policy allows containers in your Amazon ECS tasks to make calls to the AWS APIs on your behalf\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## AmazonEC2ContainerServiceEventsRole<a name="AmazonEC2ContainerServiceEventsRole"></a>

This policy allows CloudWatch Events to run tasks on your behalf\. For more information, see [Scheduled tasks \(`cron`\)](scheduled_tasks.md)\.

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