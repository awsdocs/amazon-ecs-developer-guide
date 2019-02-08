# Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The Amazon ECS\-optimized Amazon Linux AMIs are provided for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\.

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.25.1`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.25.1-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0f7f8edb4fe82cf70 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0f7f8edb4fe82cf70) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0bf2fb355727b7faf | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0bf2fb355727b7faf) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0d3bd9852d477ade8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0d3bd9852d477ade8) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-082091011e69ea8a8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-082091011e69ea8a8) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-067e52f6552e2dac1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-067e52f6552e2dac1) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0376206bb575c76dd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0376206bb575c76dd) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0728d926f3f65c089 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0728d926f3f65c089) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-08b3fd22c78a217d5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-08b3fd22c78a217d5) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0f462e20379b1cc66 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0f462e20379b1cc66) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0c0acda930d2bb85a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0c0acda930d2bb85a) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0e5c7bfb31b5e577e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0e5c7bfb31b5e577e) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-0a7936ea6d7d16c6d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0a7936ea6d7d16c6d) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-04770d89e9dedfa8b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-04770d89e9dedfa8b) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-093533dfa5b9a14ff | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-093533dfa5b9a14ff) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-074d7facdfdfd9ee2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-074d7facdfdfd9ee2) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-09870f8efd9314b59 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-09870f8efd9314b59) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.l\-amazon\-ecs\-optimized | ami\-00224e61 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-00224e61) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized Amazon Linux AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)