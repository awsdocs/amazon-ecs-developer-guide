# Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The Amazon ECS\-optimized Amazon Linux AMIs are provided for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\.

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.25.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.25.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-06b0b1a4f330921e7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-06b0b1a4f330921e7) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-031507b307be48f22 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-031507b307be48f22) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0956ec5e79bcaa784 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0956ec5e79bcaa784) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-089408c670f3e10c0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-089408c670f3e10c0) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0ef426807a3b85415 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0ef426807a3b85415) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-042dad9866b3cb091 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-042dad9866b3cb091) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-001085c9389955bb6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-001085c9389955bb6) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0e4ccc6bc8b6243c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0e4ccc6bc8b6243c4) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-04f5a7cba4ff05434 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-04f5a7cba4ff05434) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0e1789b579cf905bf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0e1789b579cf905bf) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0b37663be8bbe0d3a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0b37663be8bbe0d3a) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-04d525f01efc24268 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-04d525f01efc24268) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0868ea3484cc0cd39 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0868ea3484cc0cd39) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0d797597b39c09784 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0d797597b39c09784) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-00925cf783e2e8fa0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-00925cf783e2e8fa0) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-0e67d868ae66a74be | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0e67d868ae66a74be) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.k\-amazon\-ecs\-optimized | ami\-cb84e9aa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-cb84e9aa) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized Amazon Linux AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)