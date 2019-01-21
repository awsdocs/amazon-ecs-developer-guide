# Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The Amazon ECS\-optimized Amazon Linux AMIs are provided for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\.

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.24.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.24.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0b31574e5d83d5c42 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0b31574e5d83d5c42) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-06bec82fb46167b4f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-06bec82fb46167b4f) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0b2cc421c0d3015b4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0b2cc421c0d3015b4) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-03a86880c9c6880ac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-03a86880c9c6880ac) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0ca148151641c602a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0ca148151641c602a) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0e4266b1932fa97c8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e4266b1932fa97c8) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0de29b072b458b107 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0de29b072b458b107) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-08f05e21d1b86879f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-08f05e21d1b86879f) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-01821d990572e458a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-01821d990572e458a) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-08733cca39f256fc0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-08733cca39f256fc0) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-04b084b13eedc8061 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-04b084b13eedc8061) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-00f815702af6b8889 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-00f815702af6b8889) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0b62a2301e9954559 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0b62a2301e9954559) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-03c73af2712f26819 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-03c73af2712f26819) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0c6c683094db433fe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0c6c683094db433fe) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-0d32ccfc47c154080 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0d32ccfc47c154080) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.j\-amazon\-ecs\-optimized | ami\-cd3654ac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-cd3654ac) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized Amazon Linux AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)