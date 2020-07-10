# Connect to your container instance<a name="instance-connect"></a>

To perform basic administrative tasks on your instance, such as updating or installing software or accessing diagnostic logs, connect to the instance using SSH\. To connect to your instance using SSH, your container instances must meet the following prerequisites:
+ Your container instances need external network access to connect using SSH\. If your container instances are running in a private VPC, they need an SSH bastion instance to provide this access\. For more information, see the [Securely connect to Linux instances running in a private Amazon VPC](http://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/) blog post\.
+ Your container instances must have been launched with a valid Amazon EC2 key pair\. Amazon ECS container instances have no password, and you use a key pair to log in using SSH\. If you did not specify a key pair when you launched your instance, there is no way to connect to the instance\. For more information, see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.
+ SSH uses port 22 for communication\. Port 22 must be open in your container instance security group for you to connect to your instance using SSH\.
**Note**  
The Amazon ECS console first\-run experience creates a security group for your container instances without inbound access on port 22\. If your container instances were launched from the console first\-run experience, add inbound access to port 22 on the security group used for those instances\. For more information, see [Authorizing Network Access to Your Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**To connect to your container instance**

1. Find the public IP or DNS address for your container instance\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the cluster that hosts your container instance\.

   1. On the **Cluster** page, choose **ECS Instances**\.

   1. On the **Container Instance** column, select the container instance to connect to\.

   1. On the **Container Instance** page, record the **Public IP** or **Public DNS** for your instance\.

1. Find the default username for your container instance AMI\. The user name for instances launched with an Amazon ECS\-optimized AMI is `ec2-user`\. For Ubuntu AMIs, the default user name is `ubuntu`\. For CoreOS, the default user name is `core`\.

1. If you are using a macOS or Linux computer, connect to your instance with the following command, substituting the path to your private key and the public address for your instance:

   ```
   $ ssh -i /path/to/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
   ```

   For more information about using a Windows computer, see [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Important**  
For more information about any issues while connecting to your instance, see [Troubleshooting Connecting to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html) in the *Amazon EC2 User Guide for Linux Instances*\.