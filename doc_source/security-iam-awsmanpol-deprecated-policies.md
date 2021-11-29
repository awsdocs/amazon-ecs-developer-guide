# Phased out AWS managed policies for Amazon Elastic Container Service<a name="security-iam-awsmanpol-deprecated-policies"></a>

The following AWS managed IAM policies are phased out\. These policies are now replaced by the updated policies\. We recommend that you update your IAM users or roles to use the updated policies\.

## AmazonEC2ContainerServiceFullAccess<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceFullAccess"></a>

**Important**  
The `AmazonEC2ContainerServiceFullAccess` managed IAM policy was phased out as of January 29, 2021, in response to a security finding with the `iam:passRole` permission\. This permission grants access to all resources including credentials to roles in the account\. Now that the policy is phased out, you can't attach the policy to any new IAM users or roles\. Any users or roles that already have the policy attached can continue using it\. However, we recommend that you update your IAM users or roles to use the `AmazonECS_FullAccess` managed policy instead\. For more information, see [Migrating to the `AmazonECS_FullAccess` managed policy](security-iam-awsmanpol-amazonecs-full-access-migration.md)\.

## AmazonEC2ContainerServiceRole<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceRole"></a>

**Important**  
The `AmazonEC2ContainerServiceRole` managed IAM policy is phased out\. It's now replaced by the Amazon ECS service\-linked role\. For more information, see [Service\-linked role for Amazon ECS](using-service-linked-roles.md)\.

## AmazonEC2ContainerServiceAutoscaleRole<a name="security-iam-awsmanpol-AmazonEC2ContainerServiceAutoscaleRole"></a>

**Important**  
The `AmazonEC2ContainerServiceAutoscaleRole` managed IAM policy is phased out\. It's now replaced by the Application Auto Scaling service\-linked role for Amazon ECS\. For more information, see [Service\-linked roles for Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the *Application Auto Scaling User Guide*\.