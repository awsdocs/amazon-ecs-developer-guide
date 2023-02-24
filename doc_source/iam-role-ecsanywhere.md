# ECS Anywhere IAM role<a name="iam-role-ecsanywhere"></a>

When registering an on\-premise server or virtual machine \(VM\) to your cluster, the server or VM requires an IAM role to communicate with AWS APIs\. You only need to create this IAM role once per AWS account\.

## Checking for the ECS Anywhere \(`ecsAnywhereRole`\) in the IAM console<a name="procedure-check-ecsanywhere-role"></a>

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. In the search box, enter `ecsAnywhereRole`\. If the role does exist, choose the role to view the attached policies\.

1. On the **Permissions** tab, verify that the **AmazonEC2ContainerServiceforEC2Role** and **AmazonSSMManagedInstanceCore** is attached to the role\.

   1. Choose **Add Permissions**, **Attach policies**\.

   1. To narrow the available policies to attach, for **Filter**, enter **AmazonEC2ContainerServiceforEC2Role** and **AmazonSSMManagedInstanceCore**\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceforEC2Role** and **AmazonSSMManagedInstanceCore** policy, and then choose **Attach policy**\.

1. Choose **Trust relationships**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, choose **Edit trust policy**, copy the policy into the **Policy Document** window and choose **Update policy**\.

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

## Creating the ECS Anywhere \(`ecsAnywhereRole`\) role<a name="ecs-anywhere-iam-role-create"></a>

**To create the `ecsAnywhereRole` \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Elastic Container Service**\.

1. Choose the **EC2 Role for Elastic Container Service** use case and then **Next: Permissions**\.

1. In the **Attached permissions policy** section, select **AmazonEC2ContainerServiceforEC2Role** and then choose **Next: Review**\.

1. For **Role name**, enter `ecsAnywhereRole` and optionally you can enter a description, for example **Allows on\-premises servers or virtual machine in an ECS cluster to access ECS\.**\.

1. Review your role information and then choose **Create role** to finish\.

1. Choose the **ecsAnywhereRole** role you just created\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. In the **Filter** box, enter **AmazonSSMManagedInstanceCore** to narrow the available policies to attach\.

1. Check the box to the left of the **AmazonSSMManagedInstanceCore** policy and choose **Attach policy**\.

1. On the **Trust relationships** tab, choose **Edit trust relationship**\.

1. Change the trust relationship so that it contains the following policy and then choose **Update Trust Policy**\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

**To create the `ecsAnywhereRole` role \(AWS CLI\)**

1. Create a local file named `ssm-trust-policy.json` with the following contents\.

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

1. Create the role\.

   ```
   aws iam create-role --role-name ecsAnywhereRole --assume-role-policy-document file://ssm-trust-policy.json
   ```

1. Attach the AWS managed policies\.

   ```
   aws iam attach-role-policy --role-name ecsAnywhereRole --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   aws iam attach-role-policy --role-name ecsAnywhereRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2ContainerServiceforEC2Role
   ```