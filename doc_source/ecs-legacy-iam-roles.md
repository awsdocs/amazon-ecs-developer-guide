# Legacy IAM Roles for Amazon ECS<a name="ecs-legacy-iam-roles"></a>

Prior to the introduction of the **AWSServiceRoleForECS** IAM role, you were required to create separate IAM roles to enable Amazon ECS permissions to call Elastic Load Balancing and Application Auto Scaling APIs on your behalf\.

The Amazon ECS service scheduler IAM role grants the Amazon ECS service scheduler permissions that it needs to register and deregister container instances with your load balancers\. You can optionally create the service scheduler IAM role and specify it when creating a service, or preferably you can allow Amazon ECS to use the service\-linked role\.

The Amazon ECS Service Auto Scaling IAM role grants Amazon ECS permission to describe your CloudWatch alarms and registered services, as well as permission to update your Amazon ECS service's desired count on your behalf\.

These legacy IAM roles are described in more detail below, but have effectively been replaced by the Amazon ECS service\-linked role\.

## Service Scheduler IAM Role<a name="service_IAM_role"></a>

Amazon ECS provides a managed IAM policy named `AmazonEC2ContainerServiceRole` to use for the service scheduler IAM role\. The `AmazonEC2ContainerServiceRole` policy is shown below\.

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

**Note**  
The `ec2:AuthorizeSecurityGroupIngress` rule is reserved for future use\. Amazon ECS does not automatically update the security groups associated with Elastic Load Balancing load balancers or Amazon ECS container instances\.

**To check for the `ecsServiceRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsServiceRole`\. If the role does not exist, use the procedure below to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose the **Permissions** tab\.

1. In the **Managed Policies** section, ensure that the **AmazonEC2ContainerServiceRole** managed policy is attached to the role\. If the policy is attached, your Amazon ECS service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach Policy**\.

   1. To narrow the available policies to attach, for **Filter**, type **AmazonEC2ContainerServiceRole**\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceRole** policy and choose **Attach Policy**\.

1. Choose **Trust Relationships**, **Edit Trust Relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2008-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ecs.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To create an IAM role for your service scheduler load balancers**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, **Create role**\. 

1. For **Select type of trusted entity** section, choose **AWS service**\.

1. For **Choose the service that will use this role**, choose **Elastic Container Service**\.

1. For **Select your use case**, choose **Elastic Container Service** and choose **Next: Permissions**\.

1. In the **Attached permissions policy** section, select the **AmazonEC2ContainerServiceRole** policy and choose **Next: Review**\.

1. For **Role Name**, type `ecsServiceRole`, enter a **Role description** and then choose **Create role**\.

## Service Auto Scaling IAM Role<a name="autoscale_IAM_role"></a>

Amazon ECS provides a managed IAM policy named `AmazonEC2ContainerServiceAutoscaleRole` to use for the Service Auto Scaling IAM role\. The `AmazonEC2ContainerServiceAutoscaleRole` policy is shown below\.

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

**To check for the Service Auto Scaling role in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Search the list of roles for `ecsAutoscaleRole`\. If the role does not exist, use the procedure below to create the role\. If the role does exist, select the role to view the attached policies\.

1. Choose the **Permissions** tab\.

1. In the **Permissions policies** section, ensure that the **AmazonEC2ContainerServiceAutoscaleRole** managed policy is attached to the role\. If the policy is attached, your Amazon ECS service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, type `AmazonEC2ContainerServiceAutoscaleRole`\.

   1. Select the box to the left of the **AmazonEC2ContainerAutoscaleRole** policy and choose **Attach policy**\.

1. Choose **Trust relationships**, **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "application-autoscaling.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To create an IAM role for Service Auto Scaling**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. In the **Choose the service that will use this role** section, choose **Elastic Container Service**\.

1. In the **Select your use case** section, choose **Elastic Container Service Autoscale**, **Next: Permissions**\.

1. For **Add tags \(optional\)**, enter any key value tags you wish to add to the IAM role\. Choose **Next: Review** when finished\.

1. In the **Role name** field, type `ecsAutoscaleRole` to name the role, and then choose **Create Role** to finish\.