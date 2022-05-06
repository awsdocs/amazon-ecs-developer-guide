# Launching an Amazon ECS Windows container instance<a name="launch_window-container_instance"></a>

Your Amazon ECS container instances are created using the Amazon EC2 console\. Before you begin, be sure that you've completed the steps in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md)\.

**To launch a container instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the Region to use\.

1. From the **EC2 Dashboard**, choose **Launch instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, complete the following steps:

   1. Choose **AWS Marketplace**\.

   1. Choose an AMI for your container instance\. You can search for one of the Amazon ECS\-optimized AMIs, for example the **Windows\_2019\_Full\_ECS\_Optimized**\. If you do not choose an Amazon ECS\-optimized AMI, you must follow the procedures in [Installing the Amazon ECS container agent](ecs-agent-install.md)\.

      For more information about the latest Amazon ECS\-optimized AMIs, see 

1. On the **Choose an Instance Type** page, you can select the hardware configuration of your instance\. The `t2.micro` instance type is selected by default\. The instance type that you select determines the resources available for your tasks to run on\.

   Choose **Next: Configure Instance Details** when you are done\.

1. On the **Configure Instance Details** page, complete the following steps:

   1. Set the **Number of instances** field depending on how many container instances you want to add to your cluster\.

   1. \(Optional\) To use Spot Instances, for **Purchasing option**, select the check box next to **Request Spot Instances**\. You also need to set the other fields related to Spot Instances\. For more information, see [Spot Instance Requests](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-requests.html)\.
**Note**  
If you are using Spot Instances and see a `Not available` message, you may need to choose a different instance type\.

   1. For **Network**, choose the VPC into which to launch your container instance\.

   1. For **Subnet**, choose a subnet to use, or keep the default option to choose the default subnet in any Availability Zone\.

   1. Set the **Auto\-assign Public IP** field depending on whether you want your instance to be accessible from the public internet\. If your instance should be accessible from the internet, verify that the **Auto\-assign Public IP** field is set to **Enable**\. If not, set this field to **Disable**\.
**Note**  
Container instances need access to communicate with the Amazon ECS service endpoint\. This can be through an interface VPC endpoint or through your container instances having public IP addresses\.  
For more information about interface VPC endpoints, see [Amazon ECS interface VPC endpoints \(AWS PrivateLink\)](vpc-endpoints.md)\.  
If you do not have an interface VPC endpoint configured and your container instances do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide* and [HTTP proxy configuration](http_proxy_config.md) in this guide\. For more information, see [Create a virtual private cloud](get-set-up-for-amazon-ecs.md#create-a-vpc)\.\.

   1. Select your container instance IAM role\. This is usually named `ecsInstanceRole`\.
**Important**  
If you do not launch your container instance with the proper IAM permissions, your Amazon ECS agent cannot connect to your cluster\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\.

   1. <a name="instance-windows-launch-user-data-step"></a>Configure your Amazon ECS container instance with user data, such as the agent environment variables from [Amazon ECS container agent configuration](ecs-agent-config.md)\. Amazon EC2 user data scripts are executed only one time, when the instance is first launched\. The following are common examples of what user data is used for:
      + By default, your container instance launches into your default cluster\. To launch into a non\-default cluster, choose the **Advanced Details** list\. Then, paste the following script into the **User data** field, replacing *your\_cluster\_name* with the name of your cluster\.

        The `EnableTaskIAMRole` turns on the Task IAM roles feature for the tasks\.

        In addition, the following options are available when you use the `awsvpc` network mode\.
        + `EnableTaskENI`: This flag turns on task networking and is required when you use the `awsvpc` network mode\.
        + `AwsvpcBlockIMDS`: This optional flag blocks IMDS access for the task containers running in the `awsvpc` network mode\.
        + `AwsvpcAdditionalLocalRoutes`: This optional flag allows you to have additional routes in the task namespace\.

          Replace `ip-address` with the IP Address for the additional routes, for example 172\.31\.42\.23/32\.

        ```
        <powershell>
        Import-Module ECSTools
        Initialize-ECSAgent -Cluster your_cluster_name -EnableTaskIAMRole -EnableTaskENI -AwsvpcBlockIMDS -AwsvpcAdditionalLocalRoutes
        '["ip-address"]'
        </powershell>
        ```

   1. Choose **Next: Add Storage**\.

1. On the **Add Storage** page, configure the storage for your container instance\.

   You can optionally increase or decrease the volume sizes for your instance to meet your application needs\.

   When done configuring your volumes, choose **Next: Add Tags**\.

1. On the **Add Tags** page, specify tags by providing key and value combinations for the container instance\. Choose **Add another tag** to add more than one tag to your container instance\. For more information resource tags, see [Resources and tags](ecs-resource-tagging.md)\.

   Choose **Next: Configure Security Group** when you are done\.

1. On the **Configure Security Group** page, use a security group to define firewall rules for your container instance\. These rules specify which incoming network traffic is delivered to your container instance\. All other traffic is ignored\. Select or create a security group as follows, and then choose **Review and Launch**\.

1. On the **Review Instance Launch** page, under **Security Groups**, you see that the wizard created and selected a security group for you\. Instead, select the security group that you created in [Set up to use Amazon ECS](get-set-up-for-amazon-ecs.md) using the following steps:

   1. Choose **Edit security groups**\.

   1. On the **Configure Security Group** page, select the **Select an existing security group** option\.

   1. Select the security group you created for your container instance from the list of existing security groups, and choose **Review and Launch**\.

1. On the **Review Instance Launch** page, choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** dialog box, choose **Choose an existing key pair**, then select the key pair that you created when getting set up\. 

   When you are ready, select the acknowledgment field, and then choose **Launch Instances**\. 

1. A confirmation page lets you know that your instance is launching\. Choose **View Instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of your instance\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running`, and it receives a public DNS name\. If the **Public DNS** column is hidden, choose **Show/Hide**, **Public DNS**\.