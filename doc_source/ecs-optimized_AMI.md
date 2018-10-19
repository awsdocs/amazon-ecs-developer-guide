# Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The Amazon ECS\-optimized Amazon Linux AMIs are provided for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\.

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.21.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.21.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Amazon Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0a0c6574ce16ce87a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0a0c6574ce16ce87a) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-07eb698ce660402d2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-07eb698ce660402d2) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-09568291a9d6c804c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-09568291a9d6c804c) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-04c22ba97a0c063c4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-04c22ba97a0c063c4) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0a0948de946510ec0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0a0948de946510ec0) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0cb31bf24b130a0f9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0cb31bf24b130a0f9) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-066826c6a40879d75 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-066826c6a40879d75) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0b9fee3a2d0596ed1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0b9fee3a2d0596ed1) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0b52e57bed048ca48 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0b52e57bed048ca48) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0edf19001c48838c7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0edf19001c48838c7) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-08c26730c8ee004fa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-08c26730c8ee004fa) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-08d4fe232c67b81b8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-08d4fe232c67b81b8) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-055750f063052ec55 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-055750f063052ec55) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-05f009513cd58ac90 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-05f009513cd58ac90) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-0ada25501ac1375b3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0ada25501ac1375b3) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.h\-amazon\-ecs\-optimized | ami\-3c39a25d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-3c39a25d) | 

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized Amazon Linux AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)