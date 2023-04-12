# Amazon ECS container agent configuration<a name="ecs-agent-config"></a>

The Amazon ECS container agent supports a number of configuration options, most of which should be set through environment variables\. The following environment variables are available, and all of them are optional\.

If your container instance was launched with a Linux variant of the Amazon ECS\-optimized AMI, you can set these environment variables in the `/etc/ecs/ecs.config` file and then restart the agent\. You can also write these configuration variables to your container instances with Amazon EC2 user data at launch time\. For more information, see [Bootstrapping container instances with Amazon EC2 user data](bootstrap_container_instance.md)\.

If your container instance was launched with a Windows variant of the Amazon ECS\-optimized AMI, you can set these environment variables with the PowerShell SetEnvironmentVariable command and then restart the agent\. For more information, see [Run commands on your Windows instance at launch](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html) in the *Amazon EC2 User Guide for Windows Instances* and [Bootstrapping Windows container instances with Amazon EC2 user data](bootstrap_windows_container_instance.md)\.

If you are manually starting the Amazon ECS container agent \(for non Amazon ECS\-optimized AMIs\), you can use these environment variables in the docker run command that you use to start the agent\. Use these variables with the syntax `--env=VARIABLE_NAME=VARIABLE_VALUE`\. For sensitive information, such as authentication credentials for private repositories, you should store your agent environment variables in a file and pass them all at one time with the `--env-file path_to_env_file` option\.

For information about the available Amazon ECS container agent configuration parameters, see [Amazon ECS Container Agent](https://github.com/aws/amazon-ecs-agent/blob/master/README.md) on GitHub\.

## Storing container instance configuration in Amazon S3<a name="ecs-config-s3"></a>

Amazon ECS container agent configuration is controlled with the environment variables described in the previous section\. Linux variants of the Amazon ECS\-optimized AMI look for these variables in `/etc/ecs/ecs.config` when the container agent starts and configure the agent accordingly\. Certain innocuous environment variables, such as `ECS_CLUSTER`, can be passed to the container instance at launch through Amazon EC2 user data and written to this file without consequence\. However, other sensitive information, such as your AWS credentials or the `ECS_ENGINE_AUTH_DATA` variable, should never be passed to an instance in user data or written to `/etc/ecs/ecs.config` in a way that would allow them to show up in a `.bash_history` file\.

Storing configuration information in a private bucket in Amazon S3 and granting read\-only access to your container instance IAM role is a secure and convenient way to allow container instance configuration at launch\. You can store a copy of your `ecs.config` file in a private bucket\. You can then use Amazon EC2 user data to install the AWS CLI and copy your configuration information to `/etc/ecs/ecs.config` when the instance launches\.

**To allow Amazon S3 read\-only access for your container instance role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and select the IAM role to use for your container instances\. This role is likely titled `ecsInstanceRole`\. For more information, see [Amazon ECS container instance IAM role](instance_IAM_role.md)\. 

1. Under **Managed Policies**, choose **Attach Policy**\.

1. To narrow the policy results, on the **Attach Policy** page, for **Filter**, type `S3`\.

1. Select the box to the left of the **AmazonS3ReadOnlyAccess** policy and choose **Attach Policy**\.

**To store an `ecs.config` file in Amazon S3**

1. Create an `ecs.config` file with valid environment variables and values from [Amazon ECS container agent configuration](#ecs-agent-config) using the following format\. This example configures private registry authentication\. For more information, see [Private registry authentication for tasks](private-auth.md)\.

   ```
   ECS_ENGINE_AUTH_TYPE=dockercfg
   ECS_ENGINE_AUTH_DATA={"https://index.docker.io/v1/":{"auth":"zq212MzEXAMPLE7o6T25Dk0i","email":"email@example.com"}}
   ```

1. To store your configuration file, create a private bucket in Amazon S3\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/CreatingaBucket.html) in the *Amazon Simple Storage Service User Guide*\. 

1. Upload the `ecs.config` file to your S3 bucket\. For more information, see [Add an Object to a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/PuttingAnObjectInABucket.html) in the *Amazon Simple Storage Service User Guide*\.

**To load an `ecs.config` file from Amazon S3 at launch**

1. Complete the earlier procedures in this section to allow read\-only Amazon S3 access to your container instances and store an `ecs.config` file in a private S3 bucket\.

1. Launch new container instances by following the steps in [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\. In [Step 7](launch_container_instance.md#instance-launch-user-data-step), use the following example script that installs the AWS CLI and copies your configuration file to `/etc/ecs/ecs.config`\.

   ```
   #!/bin/bash
   yum install -y aws-cli
   aws s3 cp s3://your_bucket_name/ecs.config /etc/ecs/ecs.config
   ```