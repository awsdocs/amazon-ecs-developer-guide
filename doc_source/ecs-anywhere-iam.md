# IAM permissions for Amazon ECS Anywhere<a name="ecs-anywhere-iam"></a>

When using Amazon ECS Anywhere, there are required and conditional IAM permissions needed\. The following sections discuss the IAM permissions in more detail\.

## Required IAM permissions for external instances<a name="ecs-anywhere-iam-required"></a>

When registering an on\-premise server or virtual machine \(VM\) to your cluster, the server or VM requires an IAM role to communicate with AWS APIs\. You only need to create this IAM role once per AWS account, but it must be associated with each server or VM you register to your clusters\. This role is referred to as the `ECSAnywhereRole`\. The role can be created manually or Amazon ECS can create the role on your behalf when registering an external instance using the AWS Management Console\.

AWS provides two managed IAM policies that can be used when creating the ECS Anywhere IAM role\. One of the managed IAM policies, `AmazonEC2ContainerServiceforEC2Role`, likely provides more permissions than is needed for your use case\. Ensure that you follow the standard security advice of granting least privilege by reviewing the `AmazonEC2ContainerServiceforEC2Role` managed policy and if your use case doesn't require all of the permissions, create a custom policy and add only the permissions you require\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

**To create the ECS Anywhere IAM role \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Systems Manager**\.

1. Choose the **Systems Manager** use case and then **Next: Permissions**\.

1. In the **Attached permissions policy** section, search for and select the **AmazonSSMManagedInstanceCore** and **AmazonEC2ContainerServiceforEC2Role** policies and then choose **Next: Review**\.
**Important**  
The `AmazonEC2ContainerServiceforEC2Role` managed policy provides permissions needed for your on\-premise server or VM\. Following the standard security advice of granting least privilege, the `AmazonEC2ContainerServiceforEC2Role` managed policy may grant permissions that aren't needed for your use case\. Review the permissions granted by this policy and if your use case doesn't require all of the permissions, create a custom policy and add only the permissions you require\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy and then choose **Next: Review**\.

1. For **Role name**, type `ECSAnywhereRole` and optionally you can edit the description\.

1. Review your role information and then choose **Create role**\. 

1. Perform a search for the `ECSAnywhereRole` and then select it to view the role details\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. Search for the `AmazonSSMManagedInstanceCore` policy, select it, and then choose **Attach policy**\.

**To create the ECS Anywhere IAM role \(AWS CLI\)**

1. Create a file named `ssm-trust-policy.json` that contains the trust policy to use for the IAM role\. The file should contain the following:

   ```
   {
     "Version": "2012-10-17",
     "Statement": {
       "Effect": "Allow",
       "Principal": {"Service": [
         "ssm.amazonaws.com"
       ]},
       "Action": "sts:AssumeRole"
     }
   }
   ```

1. Create an IAM role named `ecsAnywhereRole` using the trust policy created in the previous step\.

   ```
   aws iam create-role \
         --role-name ecsAnywhereRole \
         --assume-role-policy-document file://ssm-trust-policy.json
   ```

1. Attach the AWS managed `AmazonSSMManagedInstanceCore` policy to the `ecsAnywhereRole` role\. This policy provides the Systems Manager API permissions needed for your on\-premise server or VM\.

   ```
   aws iam attach-role-policy \
         --role-name ecsAnywhereRole \
         --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

1. Attach the AWS managed `AmazonEC2ContainerServiceforEC2Role` policy to the `ecsAnywhereRole` role\.

   ```
   aws iam attach-role-policy \
        --role-name ecsAnywhereRole \
        --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceforEC2Role
   ```
**Important**  
The `AmazonEC2ContainerServiceforEC2Role` managed policy provides permissions needed for your on\-premise server or VM\. Following the standard security advice of granting least privilege, the `AmazonEC2ContainerServiceforEC2Role` managed policy may grant permissions that aren't needed for your use case\. Review the permissions granted by this policy and if your use case doesn't require all of the permissions, create a custom policy and add only the permissions you require\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

## Conditional IAM permissions<a name="ecs-anywhere-iam-conditional"></a>

The task execution IAM role grants the Amazon ECS container agent, which must be running on your external instance, permission to make AWS API calls on your behalf\. When a task execution IAM role is used, it must be specified in your task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

The task execution role is required if any of the following conditions exist:
+ You're sending container logs to CloudWatch Logs using the `awslogs` log driver\.
+ Your task definition specifies a container image hosted in an Amazon ECR private repository AND the `ECSAnywhereRole` IAM role associated with your external instance doesn't include the permissions necessary to pull images from Amazon ECR\.