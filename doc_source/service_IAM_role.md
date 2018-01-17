# Amazon ECS Service Scheduler IAM Role<a name="service_IAM_role"></a>

The Amazon ECS service scheduler makes calls to the Amazon EC2 and Elastic Load Balancing APIs on your behalf to register and deregister container instances with your load balancers\. Before you can attach a load balancer to an Amazon ECS service, you must create an IAM role for your services to use before you start them\. This requirement applies to any Amazon ECS service that you plan to use with a load balancer\.

In most cases, the Amazon ECS service role is created for you automatically in the console first\-run experience\. You can use the following procedure to check if your account already has the Amazon ECS service role\.

The `AmazonEC2ContainerServiceRole` policy is shown below\.

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

   1. In the **Filter** box, type **AmazonEC2ContainerServiceRole** to narrow the available policies to attach\.

   1. Check the box to the left of the **AmazonEC2ContainerServiceRole** policy and choose **Attach Policy**\.

1. Choose the **Trust Relationships** tab, and **Edit Trust Relationship**\.

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

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. In the **Select type of trusted entity** section, choose **EC2 Container Service**\.

1. In the **Select your use case** section, choose **EC2 Container Service** and then choose **Next: Permissions**\.

1. In the **Attached permissions policy** section, select the **AmazonEC2ContainerServiceRole** policy and then choose **Next: Review**\.

1. For **Role Name**, type `ecsServiceRole`, enter a **Role description**, and then choose **Create role**\.