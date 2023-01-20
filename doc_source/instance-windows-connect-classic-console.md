# Connect to your container Windows instance using the classic console<a name="instance-windows-connect-classic-console"></a>

You can connect to your Windows instances to perform basic administrative tasks, such as installing or updating software or accessing diagnostic logs\. To connect to your instance using Remote Desktop Protocol \(RDP\), your Windows instance must meet the following prerequisites\.
+ Amazon EC2 instances created from most Windows AMIs allow you to connect using Remote Desktop Protocol \(RDP\)\. RDP allows you to connect to and use your instance in the same way you use a computer sitting in front of you\. It is available on most editions of Windows and available for Mac OS\.
+ Your Windows instance must have been launched with a valid Amazon EC2 key pair\. Amazon EC2 instances have no password, you use a key pair for access over RDP\. If you did not specify a key pair when you launched your instance, there is no way to connect to the instance\.
+ Ensure that the security group associated with your instance allows incoming RDP traffic \(port 3389\) from your IP address\. The default security group doesn't allow incoming RDP traffic by default\. For more information, see [Authorize inbound traffic for your Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Find the public IP or DNS address for your container instance\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the cluster that hosts your container instance\.

   1. On the **Cluster** page, choose **ECS Instances**\.

   1. On the **Container Instance** column, select the container instance to connect to\.

   1. On the **Container Instance** page, record the **Public IP** or **Public DNS** for your instance\.

1. Find the default username for your container instance AMI\. 

1. You can connect to your instance by using RDP\. For more information, see [Connect to your Windows instance using RDP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.