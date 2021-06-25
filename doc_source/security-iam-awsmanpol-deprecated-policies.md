# Phased out AWS managed policies for Amazon Elastic Container Service<a name="security-iam-awsmanpol-deprecated-policies"></a>

The following AWS managed IAM policies are phased out\. These policies are now replaced by the updated policies\. We recommend that you update your IAM users or roles to use the updated policies\.

## AmazonEC2ContainerServiceFullAccess<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceFullAccess"></a>

**Important**  
The `AmazonEC2ContainerServiceFullAccess` managed IAM policy was phased out as of January 29, 2021, in response to a security finding with the `iam:passRole` permission\. This permission grants access to all resources including credentials to roles in the account\. Now that the policy is phased out, you can't attach the policy to any new IAM users or roles\. Any users or roles that already have the policy attached can continue using it\. However, we recommend that you update your IAM users or roles to use the `AmazonECS_FullAccess` managed policy instead\. For more information, see [Migrating to the `AmazonECS_FullAccess` managed policy](security-iam-awsmanpol-amazonecs-full-access-migration.md)\.

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

## AmazonEC2ContainerServiceRole<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceRole"></a>

**Important**  
The `AmazonEC2ContainerServiceAutoscaleRole` managed IAM policy is phased out\. It's now replaced by the Amazon ECS service\-linked role\. For more information, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.

This policy grants administrative permissions that allow Elastic Load Balancing load balancers to register and deregister Amazon ECS container instances on your behalf\.

**Permissions details**

This policy includes the following permissions\.
+ `ec2` – Allows the principals to add ingress rules to security groups\.
+ `elasticloadbalancing` – Allows the principals to register and deregister targets to target groups and instances to load balancers\.

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

## AmazonEC2ContainerServiceAutoscaleRole<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceAutoscaleRole"></a>

**Important**  
The `AmazonEC2ContainerServiceAutoscaleRole` managed IAM policy is phased out\. It's now replaced by the Application Auto Scaling service\-linked role for Amazon ECS\. For more information, see [Service\-linked roles for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\.

This policy grants administrative permissions that allow Application Auto Scaling to scale up or down the desired count for your Amazon ECS service in response to CloudWatch alarms on your behalf\.

**Permissions details**

This policy includes the following permissions\.
+ `ecs` – Allows you to view and modify the services that are running in a cluster\.
+ `cloudwatch` – Allows you to view, create, and modify Amazon CloudWatch alarms\.

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