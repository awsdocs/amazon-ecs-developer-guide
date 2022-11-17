# Set up to use Amazon ECS<a name="get-set-up-for-amazon-ecs"></a>

If you've already signed up for Amazon Web Services \(AWS\) and have been using Amazon Elastic Compute Cloud \(Amazon EC2\), you are close to being able to use Amazon ECS\. The set\-up process for the two services is similar\. The following guide prepares you for launching your first Amazon ECS cluster\.

Complete the following tasks to get set up for Amazon ECS\.

## Sign up for an AWS account<a name="sign-up-for-aws"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root)\.

AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

## Create an administrative user<a name="create-an-admin"></a>

After you sign up for an AWS account, create an administrative user so that you do not use the root user for everyday tasks\.

**Secure your AWS account root user**

1.  Sign in to the [AWS Management Console](https://console.aws.amazon.com/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.

   For help signing in using root user, see [Signing in as the root user](https://docs.aws.amazon.com/signin/latest/userguide/console-sign-in-tutorials.html#introduction-to-root-user-sign-in-tutorial) in the *AWS Sign\-In User Guide*\.

1. Turn on multi\-factor authentication \(MFA\) for your root user\.

   For instructions, see [Enable a virtual MFA device for your AWS account root user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-root) in the *IAM User Guide*\.

**Create an administrative user**
+ For your daily administrative tasks, assign administrative access to an administrative user in AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\.

  For instructions, see [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

**Sign in as the administrative user**
+ To sign in with your IAM Identity Center user, use the sign\-in URL that was sent to your email address when you created the IAM Identity Center user\.

  For help signing in using an IAM Identity Center user, see [Signing in to the AWS access portal](https://docs.aws.amazon.com/signin/latest/userguide/iam-id-center-sign-in-tutorial.html) in the *AWS Sign\-In User Guide*\.

## Create the credentials to connect to your EC2 instance<a name="create-a-key-pair"></a>

For Amazon ECS, a key pair is only needed if you intend on using the EC2 launch type\.

AWS uses public\-key cryptography to secure the login information for your instance\. A Linux instance, such as an Amazon ECS container instance, has no password to use for SSH access\. You use a key pair to log in to your instance securely\. You specify the name of the key pair when you launch your container instance, then provide the private key when you log in using SSH\.

If you haven't created a key pair already, you can create one using the Amazon EC2 console\. If you plan to launch instances in multiple regions, you'll need to create a key pair in each region\. For more information about regions, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To create a key pair**
+ Use the Amazon EC2 console to create a key pair\. For more information about creatiting a key pair, see [Create a key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To connect to your instance using your key pair**  
To connect to your Linux instance from a computer running macOS or Linux, specify the `.pem` file to your SSH client with the `-i` option and the path to your private key\. To connect to your Linux instance from a computer running Windows, you can use either MindTerm or PuTTY\. If you plan to use PuTTY, you need to install it and use the following procedure to convert the `.pem` file to a `.ppk` file\.<a name="prepare-for-putty"></a>

**To prepare to connect to a Linux instance from Windows using PuTTY**

1. Download and install PuTTY from [ http://www\.chiark\.greenend\.org\.uk/\~sgtatham/putty/](http://www.chiark.greenend.org.uk/~sgtatham/putty/)\. Be sure to install the entire suite\.

1. Start PuTTYgen \(for example, from the **Start** menu, choose **All Programs > PuTTY > PuTTYgen**\)\.

1. Under **Type of key to generate**, choose **RSA**\.  
![\[SSH-2 RSA key in PuTTYgen\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/puttygen-key-type.png)

1. Choose **Load**\. By default, PuTTYgen displays only files with the extension `.ppk`\. To locate your `.pem` file, select the option to display files of all types\.  
![\[Select all file types\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/puttygen-load-key.png)

1. Select the private key file that you created in the previous procedure and choose **Open**\. Choose **OK** to dismiss the confirmation dialog box\.

1. Choose **Save private key**\. PuTTYgen displays a warning about saving the key without a passphrase\. Choose **Yes**\.

1. Specify the same name for the key that you used for the key pair\. PuTTY automatically adds the `.ppk` file extension\.

## Create a virtual private cloud<a name="create-a-vpc"></a>

Amazon Virtual Private Cloud \(Amazon VPC\) enables you to launch AWS resources into a virtual network that you've defined\. We strongly suggest that you launch your container instances in a VPC\. 

**Note**  
The Amazon ECS console first\-run experience creates a VPC for your cluster, so if you intend to use the Amazon ECS console, you can skip to the next section\.

If you have a default VPC, you also can skip this section and move to the next task, [Create a security group](#create-a-base-security-group)\. To determine whether you have a default VPC, see [Supported Platforms in the Amazon EC2 Console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html#console-updates) in the *Amazon EC2 User Guide for Linux Instances*\. Otherwise, you can create a nondefault VPC in your account using the steps below\.

**Important**  
If your account supports Amazon EC2 Classic in a region, then you do not have a default VPC in that region\.

For information about how to create a VPC, see [Create a VPC only](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html) in the *Amazon VPC User Guide *and use the following table to determine what options to select\.


| Option | Value | 
| --- | --- | 
|  Resources to create  | VPC only | 
| Name |  Optionally provide a name for your VPC\.  | 
| IPv4 CIDR block |  IPv4 CIDR manual input The CIDR block size must have a size between /16 and /28\.  | 
|  IPv6 CIDR block  |  No IPv6 CIDR block  | 
|  Tenancy  |  Default  | 

For more information about Amazon VPC, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/) in the *Amazon VPC User Guide*\.

## Create a security group<a name="create-a-base-security-group"></a>

Security groups act as a firewall for associated container instances, controlling both inbound and outbound traffic at the container instance level\. You can add rules to a security group that enable you to connect to your container instance from your IP address using SSH\. You can also add rules that allow inbound and outbound HTTP and HTTPS access from anywhere\. Add any rules to open ports that are required by your tasks\. Container instances require external network access to communicate with the Amazon ECS service endpoint\. 

**Note**  
The Amazon ECS classic console first run experience creates a security group for your instances and load balancer based on the task definition you use, so if you intend to use the Amazon ECS console, you can move ahead to the next section\.

If you plan to launch container instances in multiple Regions, you need to create a security group in each Region\. For more information, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Tip**  
You need the public IP address of your local computer, which you can get using a service\. For example, we provide the following service: [http://checkip\.amazonaws\.com/](http://checkip.amazonaws.com/) or [https://checkip\.amazonaws\.com/](https://checkip.amazonaws.com/)\. To locate another service that provides your IP address, use the search phrase "what is my IP address\." If you are connecting through an internet service provider \(ISP\) or from behind a firewall without a static IP address, you must find out the range of IP addresses used by client computers\.

For information about how to create a security group, see [Create a security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#creating-security-group) in the *Amazon EC2 User Guide for Linux Instances* and use the following table to determine what options to select\.


| Option | Value | 
| --- | --- | 
|  Region  | The same Region in which you created your key pair\. | 
| Name | A name that is easy for you to remember, such as ecs\-instances\-default\-cluster\. | 
| VPC | The default VPC \(marked with "\(default\)" \. If your account supports Amazon EC2 Classic, select the VPC that you created in the previous task\.   | 

Amazon ECS container instances do not require any inbound ports to be open\. However, you might want to add an SSH rule so you can log into the container instance and examine the tasks with Docker commands\. You can also add rules for HTTP and HTTPS if you want your container instance to host a task that runs a web server\. Container instances do require external network access to communicate with the Amazon ECS service endpoint\. Complete the following steps to add these optional security group rules\.

Add the following three inbound rules to your security group\.For information about how to create a security group, see [Add rules to your security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-security-groups.html#adding-security-group-rule) in the *Amazon EC2 User Guide for Linux Instances*\.


| Option | Value | 
| --- | --- | 
|  HTTP rule  | Type: HTTP Source: Anywhere \(`0.0.0.0/0`\) This option automatically adds the 0\.0\.0\.0/0 IPv4 CIDR block as the source\. This is acceptable for a short time in a test environment, but it's unsafe in production environments\. In production, authorize only a specific IP address or range of addresses to access your instance\.   | 
|  HTTPS rule  | Type: HTTPS Source: Anywhere \(`0.0.0.0/0`\) This is acceptable for a short time in a test environment, but it's unsafe in production environments\. In production, authorize only a specific IP address or range of addresses to access your instance\.   | 
|  SSH rule  | Type: SSH Source: Custom, specify the public IP address of your computer or network in CIDR notation\. To specify an individual IP address in CIDR notation, add the routing prefix `/32`\. For example, if your IP address is `203.0.113.25`, specify `203.0.113.25/32`\. If your company allocates addresses from a range, specify the entire range, such as `203.0.113.0/24`\.  For security reasons, we don't recommend that you allow SSH access from all IP addresses \(`0.0.0.0/0`\) to your instance, except for testing purposes and only for a short time\.   | 

## Install the AWS CLI<a name="install_aws_cli"></a>

The AWS Management Console can be used to manage all operations manually with Amazon ECS\. However, installing the AWS CLI on your local desktop or a developer box enables you to build scripts that can automate common management tasks in Amazon ECS\.

To use the AWS CLI with Amazon ECS, install the latest AWS CLI version\. For information about installing the AWS CLI or upgrading it to the latest version, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.