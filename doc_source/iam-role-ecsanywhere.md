# ECS Anywhere IAM role<a name="iam-role-ecsanywhere"></a>

When registering an on\-premise server or virtual machine \(VM\) to your cluster, the server or VM requires an IAM role to communicate with AWS APIs\. You only need to create this IAM role once per AWS account\.<a name="procedure-check-ecsanywhere-role"></a>

**To check for the `ecsAnywhereRole` IAM role \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. 

1. Search the list of roles for `ecsAnywhereRole`\. If the role does not exist, use the procedure in the next section to create the role\. If the role does exist, select the role to view the attached policies\.

1. On the **Permissions** tab, ensure that the **AmazonEC2ContainerServiceforEC2Role** and **AmazonSSMManagedInstanceCore** managed policies are attached to the role\. If the policy is attached, your Amazon ECS Anywhere role is properly configured\. If not, follow the steps below to attach the policies\.

   1. Choose **Attach policies**\.

   1. In the **Filter** box, type **AmazonEC2ContainerServiceforEC2Role** to narrow the available policies to attach\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceforEC2Role** policy and choose **Attach policy**\.

   1. In the **Filter** box, type **AmazonSSMManagedInstanceCore** to narrow the available policies to attach\.

   1. Check the box to the left of the **AmazonSSMManagedInstanceCore** policy and choose **Attach policy**\.

1. Choose the **Trust relationships** tab, and **Edit trust relationship**\.

1. Verify that the trust relationship contains the following policy\. If the trust relationship matches the policy below, choose **Cancel**\. If the trust relationship does not match, copy the policy into the **Policy Document** window and choose **Update Trust Policy**\.

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

**To create the `ecsAnywhereRole` IAM role \(AWS Management Console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose **Elastic Container Service**\.

1. Choose the **EC2 Role for Elastic Container Service** use case and then **Next: Permissions**\.

1. In the **Attached permissions policy** section, select **AmazonEC2ContainerServiceforEC2Role** and then choose **Next: Review**\.

1. For **Role name**, type `ecsAnywhereRole` and optionally you can enter a description, for example **Allows on\-premises servers or virtual machine in an ECS cluster to access ECS\.**\.

1. Review your role information and then choose **Create role** to finish\.

1. Choose the **ecsAnywhereRole** role you just created\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. In the **Filter** box, type **AmazonSSMManagedInstanceCore** to narrow the available policies to attach\.

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

**To create the `ecsAnywhereRole` IAM role \(AWS CLI\)**

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
   aws iam attach-role-policy --role-name ecsAnywhereRole --policy-arn
   ```