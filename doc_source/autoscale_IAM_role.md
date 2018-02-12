# Amazon ECS Service Auto Scaling IAM Role<a name="autoscale_IAM_role"></a>

Before you can use Service Auto Scaling with Amazon ECS, the Application Auto Scaling service needs permission to describe your CloudWatch alarms and registered services, as well as permission to update your Amazon ECS service's desired count on your behalf\. These permissions are provided by the Service Auto Scaling IAM role \(`ecsAutoscaleRole`\)\.

**Note**  
IAM users also require permissions to use Service Auto Scaling; these permissions are described in [Service Auto Scaling Required IAM Permissions](service-autoscaling-stepscaling.md#auto-scaling-IAM)\. If an IAM user has the required permissions to use Service Auto Scaling in the Amazon ECS console, create IAM roles, and attach IAM role policies to them, then that user can create this role automatically as part of the Amazon ECS console create service or update service workflows, and then use the role for any other service later \(in the console or with the CLI/SDKs\)\.

You can use the following procedure to check and see if your account already has Service Auto Scaling\.

The `AmazonEC2ContainerServiceAutoscaleRole` policy is shown below\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1456535218000",
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
            "Sid": "Stmt1456535243000",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms"
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

1. In the **Managed Policies** section, ensure that the **AmazonEC2ContainerServiceAutoscaleRole** managed policy is attached to the role\. If the policy is attached, your Amazon ECS service role is properly configured\. If not, follow the substeps below to attach the policy\.

   1. Choose **Attach Policy**\.

   1. For **Filter**, type `AmazonEC2ContainerServiceAutoscaleRole` to narrow the available policies to attach\.

   1. Select the box to the left of the **AmazonEC2ContainerAutoscaleRole** policy and choose **Attach Policy**\.

1. Choose **Trust Relationships**, **Edit Trust Relationship**\.

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

1. In the navigation pane, choose **Roles** and then choose **Create New Role**\. 

1. In the **Select Role Type** section, scroll down and choose **Select** next to the **Amazon Elastic Container Service Autoscale Role** service role\.

1. In the **Attach Policy** section, select the **AmazonEC2ContainerServiceAutoscaleRole** policy and then choose **Next Step**\.

1. In the **Role Name** field, type `ecsAutoscaleRole` to name the role, and then choose **Next Step**\.

1. Review your role information and then choose **Create Role** to finish\. 