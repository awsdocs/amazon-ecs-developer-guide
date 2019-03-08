# Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The Amazon ECS\-optimized Amazon Linux AMIs are provided for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\.

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.26.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.26.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0cca5d0eeadc8c3c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0cca5d0eeadc8c3c4) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0a6a36557ea3b9859 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0a6a36557ea3b9859) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d2f82a622136a696 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0d2f82a622136a696) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-066a6b3ae13abc046 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-066a6b3ae13abc046) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-020cc3695affa4b6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-020cc3695affa4b6b) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0e1065cc4f7231034 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0e1065cc4f7231034) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-00921cd1ce43d567a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-00921cd1ce43d567a) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-042ae7188819e7e9b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-042ae7188819e7e9b) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0a92075786c5779b9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0a92075786c5779b9) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0e0d82e1272b5ae8a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0e0d82e1272b5ae8a) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-084cb340923dc7101 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-084cb340923dc7101) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-051b682e0d63cc816 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-051b682e0d63cc816) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0eb4239fe0f64fe58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0eb4239fe0f64fe58) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d9198a587e83919b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0d9198a587e83919b) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0d7805fed18723d71 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0d7805fed18723d71) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-0c4cd93b06ee26c34 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0c4cd93b06ee26c34) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-04a689185b06da6db | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-04a689185b06da6db) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.o\-amazon\-ecs\-optimized | ami\-7a5d361b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-7a5d361b) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized Amazon Linux AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)