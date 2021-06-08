# IAM permissions for Amazon ECS Anywhere<a name="ecs-anywhere-iam"></a>

There are several required and conditional IAM permissions that apply to Amazon ECS Anywhere\. The following sections describe the IAM permissions in more detail\.

## Required IAM permissions for external instances<a name="ecs-anywhere-iam-required"></a>

When registering an on\-premises server or virtual machine \(VM\) to your cluster, the server or VM requires an IAM role to communicate with AWS APIs\. You only need to create this IAM role once for each AWS account\. However, this IAM role must be associated with each server or VM that you register to a cluster\. This role is the `ECSAnywhereRole`\. You can create this role manually\. Alternatively, Amazon ECS can create the role on your behalf when you register an external instance in the AWS Management Console\.

AWS provides two managed IAM policies that can be used when creating the ECS Anywhere IAM role, the `AmazonSSMManagedInstanceCore` and `AmazonEC2ContainerServiceforEC2Role` policies\. The `AmazonEC2ContainerServiceforEC2Role` policy includes permissions that likely provide more access than you need\. Therefore, depending on your specific use case, we recommend that you create a custom policy adding only the permissions from that policy that you require in it\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

**To create the ECS Anywhere IAM role \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Systems Manager**\.

1. Choose the **Systems Manager** use case and then **Next: Permissions**\.

1. In the **Attached permissions policy** section, search for and select the **AmazonSSMManagedInstanceCore** and **AmazonEC2ContainerServiceforEC2Role** policies and then choose **Next: Review**\.
**Important**  
The `AmazonEC2ContainerServiceforEC2Role` managed policy provides permissions that are needed for your on\-premises server or VM\. However, the `AmazonEC2ContainerServiceforEC2Role` managed policy might grant permissions that aren't needed for your use case\. Review the permissions granted by this policy and see if your use case doesn't require all of the permissions\. Then, depending on your situation, optionally create a custom policy and add only the permissions you require\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy and then choose **Next: Review**\.

1. For **Role name**, enter `ECSAnywhereRole` and optionally you can edit the description\.

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

1. Create an IAM role named `ecsAnywhereRole` using the trust policy that's created in the previous step\.

   ```
   aws iam create-role \
         --role-name ecsAnywhereRole \
         --assume-role-policy-document file://ssm-trust-policy.json
   ```

1. Attach the AWS managed `AmazonSSMManagedInstanceCore` policy to the `ecsAnywhereRole` role\. This policy provides the Systems Manager API permissions that are needed for your on\-premises server or VM\.

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
The `AmazonEC2ContainerServiceforEC2Role` managed policy provides permissions that are needed for your on\-premises server or VM\. However, the `AmazonEC2ContainerServiceforEC2Role` managed policy might grant permissions that aren't needed for your use case\. Review the permissions granted by this policy and see if your use case doesn't require all of the permissions\. Then, depending on your situation, optionally create a custom policy and add only the permissions you require\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

## Conditional IAM permissions<a name="ecs-anywhere-iam-conditional"></a>

The task execution IAM role grants the Amazon ECS container agent permission to make AWS API calls on your behalf\. When a task execution IAM role is used, it must be specified in your task definition\. For more information, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

The task execution role is required if any of the following conditions apply:
+ You're sending container logs to CloudWatch Logs using the `awslogs` log driver\.
+ Your task definition specifies a container image that's hosted in an Amazon ECR private repository\. However, if the `ECSAnywhereRole` IAM role that's associated with your external instance also includes the permissions necessary to pull images from Amazon ECR then your task execution role doesn't need to include them\.