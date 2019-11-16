# Savings Plans and Amazon ECS<a name="savings-plans"></a>

Savings Plans are a pricing model that offer significant savings on AWS usage\. This pricing model offers lower prices on Amazon EC2 instances usage, regardless of instance family, size, operating system, tenancy or AWS Region, and also apply to AWS Fargate usage\. You commit to a consistent amount of usage, in USD per hour, for a term of 1 or 3 years, and receive a lower price for that usage\. For more information, see the [Savings Plans User Guide](https://docs.aws.amazon.com/savingsplans/latest/userguide/)\.

Each type of usage has an On\-Demand rate and a Savings Plans price\. For example, if you commit to $10/hour of compute usage, you'll receive Savings Plans prices on all usage up to $10\. Any usage beyond the commitment is charged at On\-Demand rates\.

Savings Plans are available for 1 year or 3 year terms\. You have the choice between **All Upfront**, **Partial upfront**, or **No upfront** payment options\.

The following Savings Plans types are available:
+ **Compute Savings Plans** provide the most flexibility and provide a discount of up to 66 percent\. These plans automatically apply to your Amazon EC2 instance usage, regardless of instance family \(for example, M5, C5\), instance sizes \(c5\.large, c5\.xlarge\), Region \(US\-East\-1, US\-East\-2\), operating system \(Windows, Linux\), or tenancy \(Dedicated, default, dedicated host\)\. They also apply to your Fargate usage\. You can move your Amazon ECS tasks from using Amazon EC2 to Fargate at any time and continue to receive the discounted rate provided by your Compute Savings Plan\.
+ **Amazon EC2 Instance Savings Plans** provide a discount of up to 72 percent, in exchange for a commitment to a specific instance type and Region\. You can change your instance size within the instance type \(for example, from `c5.xlarge` to `c5.2xlarge`\) or the operating system \(for example, from Windows to Linux\) and continue to receive the discounted rate provided by your Amazon EC2 Instance Savings Plan\.

To get started, see [Get Started with Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/get-started.html) in the *Savings Plans User Guide*\.