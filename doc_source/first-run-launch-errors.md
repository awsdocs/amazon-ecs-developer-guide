# Troubleshooting First\-Run Wizard Launch Issues<a name="first-run-launch-errors"></a>

The following error can prevent the Amazon ECS first\-run wizard from creating your cluster\.

VpcLimitExceeded  
If you get a `VpcLimitExceeded` error when attempting to complete the Amazon ECS first\-run wizard, you have reached the limit on the number of VPCs that you can create in a Region\. When you create your AWS account, there are default limits on the number of VPCs you can run in each Region\. For more information, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)\.  
To resolve this issue, you have the following options:  
+ Request a VPC service limit increase on a per\-Region basis\. For more information, see [Amazon VPC Limits](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html)\.
+ Delete any unused VPCs on your account\. For more information, see [Working with VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html)\.

**Important**  
Any Amazon ECS resources that were successfully created during the first\-run wizard before receiving this error can be deleted before running the wizard again\.