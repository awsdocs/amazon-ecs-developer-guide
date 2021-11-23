# Connect to your container Windows instance<a name="instance-windows-connect"></a>

To perform basic administrative tasks on your instance, such as updating or installing software or accessing diagnostic logs, connect to the instance using SSH\. To connect to your instance using SSH, your container instances must meet the following prerequisites:
+ Amazon EC2 instances created from most Windows AMIs allow you to connect using Remote Desktop\. Remote Desktop uses the Remote Desktop Protocol \(RDP\) and allowss you to connect to and use your instance in the same way you use a computer sitting in front of you\. It is available on most editions of Windows and available for Mac OS\.
+ Your container instances must have been launched with a valid Amazon EC2 key pair\. Amazon ECS container instances have no password, and you use a key pair to log in using SSH\. If you did not specify a key pair when you launched your instance, there is no way to connect to the instance\. For more information, see [Launching an Amazon ECS Windows container instance](launch_window-container_instance.md)

  \.
+ Ensure that the security group associated with your instance allows incoming RDP traffic \(port 3389\) from your IP address\. The default security group does not allow incoming RDP traffic by default\. For more information, see [Authorize inbound traffic for your Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

------
#### [ New AWS Management Console ]

1. Find the public IP or DNS address for your container instance\.

   1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

   1. In the navigation pane, choose **Clusters** and select the cluster that hosts the instance\.

   1. On the **Cluster : *name*** page, choose the **Infrastructure** tab\.

   1. Under **Container instances**, select the instance ID

   1. On the **Instances** page, record the **Public IP** or **Public DNS** for your instance\.

1. Find the default username for your container instance AMI\. 

1. You can connect to your instance by using RDP\. For more information, see [Connect to your Windows instance using RDP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

------
#### [ Classic AWS Management Console ]

1. Find the public IP or DNS address for your container instance\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the cluster that hosts your container instance\.

   1. On the **Cluster** page, choose **ECS Instances**\.

   1. On the **Container Instance** column, select the container instance to connect to\.

   1. On the **Container Instance** page, record the **Public IP** or **Public DNS** for your instance\.

1. Find the default username for your container instance AMI\. 

1. You can connect to your instance by using RDP\. For more information, see [Connect to your Windows instance using RDP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the *Amazon EC2 User Guide for Windows Instances*\.

------