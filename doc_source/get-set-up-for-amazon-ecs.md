# Setting Up with Amazon ECS<a name="get-set-up-for-amazon-ecs"></a>

If you've already signed up for Amazon Web Services \(AWS\) and have been using Amazon Elastic Compute Cloud \(Amazon EC2\), you are close to being able to use Amazon ECS\. The set up process for the two services is very similar\. The following guide prepares you for launching your first cluster using either the Amazon ECS first\-run wizard or the Amazon ECS Command Line Interface \(CLI\)\.

**Note**  
Because Amazon ECS uses many components of Amazon EC2, you use the Amazon EC2 console for many of these steps\.

Complete the following tasks to get set up for Amazon ECS\. If you have already completed any of these steps, you may skip them and move on to installing the custom AWS CLI\.

1. [Sign Up for AWS](#sign-up-for-aws)

1. [Create an IAM User](#create-an-iam-user)

1. [Create an IAM Role for your Container Instances and Services](#create-an-iam-role)

1. [Create a Key Pair](#create-a-key-pair)

1. [Create a Virtual Private Cloud](#create-a-vpc)

1. [Create a Security Group](#create-a-base-security-group)

1. [Install the AWS CLI](#install_aws_cli)

## Sign Up for AWS<a name="sign-up-for-aws"></a>

When you sign up for AWS, your AWS account is automatically signed up for all services, including Amazon EC2 and Amazon ECS\. You are charged only for the services that you use\.

If you have an AWS account already, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

Note your AWS account number, because you'll need it for the next task\.

## Create an IAM User<a name="create-an-iam-user"></a>

Services in AWS, such as Amazon EC2 and Amazon ECS, require that you provide credentials when you access them, so that the service can determine whether you have permission to access its resources\. The console requires your password\. You can create access keys for your AWS account to access the command line interface or API\. However, we don't recommend that you access AWS using the credentials for your AWS account; we recommend that you use AWS Identity and Access Management \(IAM\) instead\. Create an IAM user, and then add the user to an IAM group with administrative permissions or and grant this user administrative permissions\. You can then access AWS using a special URL and the credentials for the IAM user\.

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\.

**To create an IAM user for yourself and add the user to an Administrators group**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](http://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane of the console, choose **Users**, and then choose **Add user**\.

1. For **User name**, type **Administrator**\.

1. Select the check box next to **AWS Management Console access**, select **Custom password**, and then type the new user's password in the text box\. You can optionally select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** type **Administrators**\.

1. For **Filter policies**, select the check box for **AWS managed \- job function**\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users, and to give your users access to your AWS account resources\. To learn about using policies to restrict users' permissions to specific AWS resources, go to [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

To sign in as this new IAM user, sign out of the AWS console, then use the following URL, where *your\_aws\_account\_id* is your AWS account number without the hyphens \(for example, if your AWS account number is `1234-5678-9012`, your AWS account ID is `123456789012`\):

```
https://your_aws_account_id.signin.aws.amazon.com/console/
```

Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays "*your\_user\_name* @ *your\_aws\_account\_id*"\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\. From the IAM dashboard, choose **Create Account Alias** and enter an alias, such as your company name\. To sign in after you create an account alias, use the following URL:

```
https://your_account_alias.signin.aws.amazon.com/console/
```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **IAM users sign\-in link** on the dashboard\.

For more information about IAM, see the [AWS Identity and Access Management User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## Create an IAM Role for your Container Instances and Services<a name="create-an-iam-role"></a>

Before the Amazon ECS container agent can register a container instance into a cluster, the agent must know which account credentials to use\. You can create an IAM role that allows the agent to know which account it should register the container instance with\. When you launch an instance with the Amazon ECS\-optimized AMI provided by Amazon using this role, the agent automatically registers the container instance into your default cluster\.

The Amazon ECS container agent also makes calls to the Amazon EC2 and Elastic Load Balancing APIs on your behalf, so container instances can be registered and deregistered with load balancers\. Before you can attach a load balancer to an Amazon ECS service, you must create an IAM role for your services to use before you start them\. This requirement applies to any Amazon ECS service that you plan to use with a load balancer\.

**Note**  
The Amazon ECS instance and service roles are automatically created for you in the console first run experience, so if you intend to use the Amazon ECS console, you can move ahead to the next section\. If you do not intend to use the Amazon ECS console, and instead plan to use the AWS CLI, complete the procedures in [Amazon ECS Container Instance IAM Role](instance_IAM_role.md) and [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md) before launching container instances or using Elastic Load Balancing load balancers with services\.

## Create a Key Pair<a name="create-a-key-pair"></a>

For Amazon ECS, a key pair is only needed if you intend on using the EC2 launch type\.

AWS uses public\-key cryptography to secure the login information for your instance\. A Linux instance, such as an Amazon ECS container instance, has no password to use for SSH access; you use a key pair to log in to your instance securely\. You specify the name of the key pair when you launch your container instance, then provide the private key when you log in using SSH\.

If you haven't created a key pair already, you can create one using the Amazon EC2 console\. Note that if you plan to launch instances in multiple regions, you'll need to create a key pair in each region\. For more information about regions, see [Regions and Availability Zones](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To create a key pair**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a region for the key pair\. You can select any region that's available to you, regardless of your location\. However, key pairs are specific to a region; for example, if you plan to launch a container instance in the US East \(Ohio\) Region, you must create a key pair for the instance in the US East \(Ohio\) Region\.  
![\[Select a region\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/EC2_select_region.png)

1. In the navigation pane, under **NETWORK & SECURITY**, choose **Key Pairs**\.
**Tip**  
The navigation pane is on the left side of the console\. If you do not see the pane, it might be minimized; choose the arrow to expand the pane\. You may have to scroll down to see the **Key Pairs** link\.  
![\[Open the key pairs page\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/key-pairs.png)

1. Choose **Create Key Pair**\.

1. Enter a name for the new key pair in the **Key pair name** field of the **Create Key Pair** dialog box, and then choose **Create**\. Use a name that is easy for you to remember, such as your IAM user name, followed by `-key-pair`, plus the region name\. For example, *me*\-key\-pair\-*useast2*\.

1. The private key file is automatically downloaded by your browser\. The base file name is the name you specified as the name of your key pair, and the file name extension is `.pem`\. Save the private key file in a safe place\.
**Important**  
This is the only chance for you to save the private key file\. You'll need to provide the name of your key pair when you launch an instance and the corresponding private key each time you connect to the instance\.

1. If you will use an SSH client on a Mac or Linux computer to connect to your Linux instance, use the following command to set the permissions of your private key file so that only you can read it\.

   ```
   chmod 400 your_user_name-key-pair-region_name.pem
   ```

For more information, see [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To connect to your instance using your key pair**  
To connect to your Linux instance from a computer running Mac or Linux, you'll specify the `.pem` file to your SSH client with the `-i` option and the path to your private key\. To connect to your Linux instance from a computer running Windows, you can use either MindTerm or PuTTY\. If you plan to use PuTTY, you'll need to install it and use the following procedure to convert the `.pem` file to a `.ppk` file\.<a name="prepare-for-putty"></a>

**\(Optional\) To prepare to connect to a Linux instance from Windows using PuTTY**

1. Download and install PuTTY from [ http://www\.chiark\.greenend\.org\.uk/\~sgtatham/putty/](http://www.chiark.greenend.org.uk/~sgtatham/putty/)\. Be sure to install the entire suite\.

1. Start PuTTYgen \(for example, from the **Start** menu, choose **All Programs > PuTTY > PuTTYgen**\)\.

1. Under **Type of key to generate**, choose **RSA**\.  
![\[SSH-2 RSA key in PuTTYgen\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/puttygen-key-type.png)

1. Choose **Load**\. By default, PuTTYgen displays only files with the extension `.ppk`\. To locate your `.pem` file, select the option to display files of all types\.  
![\[Select all file types\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/puttygen-load-key.png)

1. Select the private key file that you created in the previous procedure and choose **Open**\. Choose **OK** to dismiss the confirmation dialog box\.

1. Choose **Save private key**\. PuTTYgen displays a warning about saving the key without a passphrase\. Choose **Yes**\.

1. Specify the same name for the key that you used for the key pair\. PuTTY automatically adds the `.ppk` file extension\.

## Create a Virtual Private Cloud<a name="create-a-vpc"></a>

Amazon Virtual Private Cloud \(Amazon VPC\) enables you to launch AWS resources into a virtual network that you've defined\. We strongly suggest that you launch your container instances in a VPC\. 

**Note**  
The Amazon ECS console first run experience creates a VPC for your cluster, so if you intend to use the Amazon ECS console, you can skip to the next section\.

If you have a default VPC, you also can skip this section and move to the next task, [Create a Security Group](#create-a-base-security-group)\. To determine whether you have a default VPC, see [Supported Platforms in the Amazon EC2 Console](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html#console-updates) in the *Amazon EC2 User Guide for Linux Instances*\. Otherwise, you can create a nondefault VPC in your account using the steps below\.

**Important**  
If your account supports Amazon EC2 Classic in a region, then you do not have a default VPC in that region\.

**To create a nondefault VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. From the navigation bar, select a region for the VPC\. VPCs are specific to a region, so you should select the same region in which you created your key pair\.

1. On the VPC dashboard, choose **Launch VPC Wizard**\.

1. On the **Step 1: Select a VPC Configuration** page, ensure that **VPC with a Single Public Subnet** is selected, and choose **Select**\.

1. On the **Step 2: VPC with a Single Public Subnet** page, enter a friendly name for your VPC in the **VPC name** field\. Leave the other default configuration settings, and choose **Create VPC**\. On the confirmation page, choose **OK**\.

For more information about Amazon VPC, see [What is Amazon VPC?](http://docs.aws.amazon.com/vpc/latest/userguide/) in the *Amazon VPC User Guide*\.

## Create a Security Group<a name="create-a-base-security-group"></a>

Security groups act as a firewall for associated container instances, controlling both inbound and outbound traffic at the container instance level\. You can add rules to a security group that enable you to connect to your container instance from your IP address using SSH\. You can also add rules that allow inbound and outbound HTTP and HTTPS access from anywhere\. Add any rules to open ports that are required by your tasks\. Note that container instances require external network access to communicate with the Amazon ECS service endpoint\. 

**Note**  
The Amazon ECS console first run experience creates a security group for your instances and load balancer based on the task definition you use, so if you intend to use the Amazon ECS console, you can move ahead to the next section\.

Note that if you plan to launch container instances in multiple regions, you need to create a security group in each region\. For more information about regions, see [Regions and Availability Zones](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Tip**  
You need the public IP address of your local computer, which you can get using a service\. For example, we provide the following service: [http://checkip\.amazonaws\.com/](http://checkip.amazonaws.com/) or [https://checkip\.amazonaws\.com/](https://checkip.amazonaws.com/)\. To locate another service that provides your IP address, use the search phrase "what is my IP address\." If you are connecting through an Internet service provider \(ISP\) or from behind a firewall without a static IP address, you need to find out the range of IP addresses used by client computers\.

**To create a security group with least privilege**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select a region for the security group\. Security groups are specific to a region, so you should select the same region in which you created your key pair\.

1. Choose **Security Groups** in the navigation pane\.

1. Choose **Create Security Group**\.

1. Enter a name for the new security group and a description\. Choose a name that is easy for you to remember, such as *ecs\-instances\-default\-cluster*\.

1. In the **VPC** list, ensure that your default VPC is selected; it's marked with an asterisk \(\*\)\.
**Note**  
If your account supports Amazon EC2 Classic, select the VPC that you created in the previous task\.

1. Amazon ECS container instances do not require any inbound ports to be open\. However, you might want to add an SSH rule so you can log into the container instance and examine the tasks with Docker commands\. You can also add rules for HTTP and HTTPS if you want your container instance to host a task that runs a web server\. Container instances do require external network access to communicate with the Amazon ECS service endpoint\. Complete the following steps to add these optional security group rules\.

   On the **Inbound** tab, create the following rules \(choose **Add Rule** for each new rule\), and then choose **Create**:
   + Choose **HTTP** from the **Type** list, and make sure that **Source** is set to **Anywhere** \(`0.0.0.0/0`\)\.
   + Choose **HTTPS** from the **Type** list, and make sure that **Source** is set to **Anywhere** \(`0.0.0.0/0`\)\.
   + Choose **SSH** from the **Type** list\. In the **Source** field, ensure that **Custom IP** is selected, and specify the public IP address of your computer or network in CIDR notation\. To specify an individual IP address in CIDR notation, add the routing prefix `/32`\. For example, if your IP address is `203.0.113.25`, specify `203.0.113.25/32`\. If your company allocates addresses from a range, specify the entire range, such as `203.0.113.0/24`\.
**Important**  
For security reasons, we don't recommend that you allow SSH access from all IP addresses \(`0.0.0.0/0`\) to your instance, except for testing purposes and only for a short time\.

## Install the AWS CLI<a name="install_aws_cli"></a>

The AWS Management Console can be used to manage all operations manually with Amazon ECS\. However, installing the AWS CLI on your local desktop or a developer box enables you to build scripts that can automate common management tasks in Amazon ECS\.

To use the AWS CLI with Amazon ECS, install the latest AWS CLI, version\. For information about installing the AWS CLI or upgrading it to the latest version, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.