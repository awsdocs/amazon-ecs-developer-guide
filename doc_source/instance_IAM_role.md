# Amazon ECS container instance IAM role<a name="instance_IAM_role"></a>

Amazon ECS container instances, including both Amazon EC2 and external instances, run the Amazon ECS container agent and require an IAM role for the service to know that the agent belongs to you\. Before you launch container instances and register them to a cluster, you must create an IAM role for your container instances to use\. The role is created in the account that you use to log into the console or run the AWS CLI commands\.

**Important**  
If you are registering external instances to your cluster, the IAM role you use requires Systems Manager permissions as well\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

Amazon ECS provides the `AmazonEC2ContainerServiceforEC2Role` managed IAM policy which contains the permissions needed to use the full Amazon ECS feature set\. This managed policy can be attached to an IAM role and associated with your container instances\. Alternatively, you can use the managed policy as a guide when creating a custom policy to use\. The container instance role provides permissions needed for the Amazon ECS container agent and Docker daemon to call AWS APIs on your behalf\. For more information on the managed policy, see [AmazonEC2ContainerServiceforEC2Role](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonEC2ContainerServiceforEC2Role)\.

## Verifying the Amazon ECS container instance IAM role<a name="instance-iam-role-verify"></a>

The Amazon ECS instance role is automatically created for you when completing the Amazon ECS console first\-run experience\. However, you can manually create the role and attach the managed IAM policy for container instances to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. Use the following procedure to check and see if your account already has the Amazon ECS container instance IAM role and to attach the managed IAM policy if needed\.<a name="procedure_check_instance_role"></a>

**To check for the `ecsInstanceRole` in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. In the **Filter roles** search box, enter **ecsInstanceRole**\. If the role does not exist, create the role\. For more information, see [Creating the Amazon ECS container instance IAM role](#instance-iam-role-create)\.

    If the role does exist, select the role to view the attached policies\.

1. Choose the **Permissions** tab\.

1. In the **Permissions policies** section, verify that the **AmazonEC2ContainerServiceforEC2Role** managed policy, or an equivalent custom policy, is attached to the role\. If the policy is attached, your container instance role is properly configured\. If not, follow the substeps below to attach the policy\.
**Important**  
The **AmazonEC2ContainerServiceforEC2Role** managed policy should be attached to the container instance IAM role, otherwise you will receive an error using the AWS Management Console to create clusters\.

   1. In the navigation pane, choose **Policies**\. 

   1. In the **Filter policies** search box, enter **AmazonEC2ContainerServiceforEC2Role**, and then choose the policy\.

   1. Choose **Attach**\.

   1. In the **Filter roles** search box, enter **ecsInstanceRole**\.

   1. Check the box to the left of the **ecsInstanceRole** role, and then choose **Attach policy**\.

   1. In the navigation pane, choose **Roles**\. 

   1. In the **Filter roles** search box, enter **ecsInstanceRole**\.

1. Choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window, and then choose **Update Trust Policy**\.

   ```
   {
     "Version": "2008-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ec2.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

## Creating the Amazon ECS container instance IAM role<a name="instance-iam-role-create"></a>

**Important**  
If you are registering external instances to your cluster, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.

The Amazon ECS instance role is automatically created for you when completing the Amazon ECS console first\-run experience\. However, you can manually create the role and attach the managed IAM policy for container instances to allow Amazon ECS to add permissions for future features and enhancements as they are introduced\. Use the following procedure to check and see if your account already has the Amazon ECS container instance IAM role and to attach the managed IAM policy if needed\.

**To create the `ecsInstanceRole` IAM role for your container instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Elastic Container Service**\.

1. Choose the **EC2 Role for Elastic Container Service** use case, and then choose **Next: Permissions**\.

1. In the **Permissions policies** section, verify the **AmazonEC2ContainerServiceforEC2Role** policy is selected and then choose **Next**\.
**Important**  
The **AmazonEC2ContainerServiceforEC2Role** managed policy should be attached to the container instance IAM role, otherwise you will receive an error using the AWS Management Console to create clusters\.

1. For **Role name**, enter **ecsInstanceRole** and optionally you can enter a description\.

1. For **Add tags \(optional\)**, specify any custom tags to associate with the policy, and then choose **Next: Review**\.

1. Review your role information and then choose **Create role** to finish\.

**To create the `ecsInstanceRole` IAM role for your container instances using the AWS CLI**

1. Create an instance profile named `ecsInstanceRole-profile` using the [create\-instance\-profile](https://docs.aws.amazon.com/cli/latest/reference/iam/create-instance-profile.html) command\. 

   ```
   aws iam create-instance-profile --instance-profile-name ecsInstanceRole-profile
   ```

   Example response

   ```
   {
       "InstanceProfile": {
           "InstanceProfileId": "AIPAJTLBPJLEGREXAMPLE",
           "Roles": [],
           "CreateDate": "2022-04-12T23:53:34.093Z",
           "InstanceProfileName": "ecsInstanceRole-profile",
           "Path": "/",
           "Arn": "arn:aws:iam::123456789012:instance-profile/ecsInstanceRole-profile"
       }
   }
   ```

1. Add the `ecsInstanceRole` role to the `ecsInstanceRole-profile` instance profile\.

   ```
   aws iam add-role-to-instance-profile \
       --instance-profile-name ecsInstanceRole-profile \
       --role-name ecsInstanceRole
   ```

## Adding Amazon S3 read\-only access to your container instance IAM role<a name="container-instance-role-s3"></a>

Storing configuration information in a private bucket in Amazon S3 and granting read\-only access to your container instance IAM role is a secure and convenient way to allow container instance configuration at launch time\. You can store a copy of your `ecs.config` file in a private bucket, use Amazon EC2 user data to install the AWS CLI and then copy your configuration information to `/etc/ecs/ecs.config` when the instance launches\.

For more information about creating an `ecs.config` file, storing it in Amazon S3, and launching instances with this configuration, see [Storing container instance configuration in Amazon S3](ecs-agent-config.md#ecs-config-s3)\.

**To allow Amazon S3 read\-only access for your container instance role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\. 

1. In the **Filter policies** search box, enter **AmazonS3ReadOnlyAccess**, and then choose the policy\.
**Note**  
This policy allows read\-only access to all Amazon S3 resources\. For more restrictive bucket policy examples, see [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html) in the Amazon Simple Storage Service User Guide\.

1. Choose **Attach**\.

1. In the **Filter roles** search box, enter **ecsInstanceRole**\.

1. Check the box to the left of the **ecsInstanceRole** role, and then choose **Attach policy**\.