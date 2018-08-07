# Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI"></a>

The Amazon ECS\-optimized AMI is the recommended AMI for you to use to launch your Amazon ECS container instances\. Although you can create your own container instance AMI that meets the basic specifications outlined in [Container Instance AMIs](container_instance_AMIs.md), the Amazon ECS\-optimized AMI is preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest AMI for you to get started and to get your containers running on AWS quickly\.

The current Amazon ECS\-optimized AMI \(`amzn-ami-2018.03.d-amazon-ecs-optimized`\) consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.20.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.03.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.20.0-1`\)

The Amazon ECS\-optimized AMI metadata, including the AMI ID, can be retrieved programmatically\. For more information, see [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e65e665ff5f3fc5f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0e65e665ff5f3fc5f) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-112e366e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-112e366e) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-a1f8dfd9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-a1f8dfd9) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-dd0de2be | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-dd0de2be) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e4185127a627bbac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0e4185127a627bbac) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-c3ea1fa4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-c3ea1fa4) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0612d1ef7f8e72c06 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0612d1ef7f8e72c06) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-c7e9e72c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-c7e9e72c) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-02b0706448bd6fb5e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-02b0706448bd6fb5e) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-256c15c8 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-256c15c8) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-df49e9bd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-df49e9bd) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0e1566e9c8eb85002 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0e1566e9c8eb85002) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-cf60edab | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-cf60edab) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-0c814df738f3c9fd5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0c814df738f3c9fd5) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-085384d0e5fd5ae0a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-085384d0e5fd5ae0a) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.d\-amazon\-ecs\-optimized | ami\-8b8814ea | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-8b8814ea) | 

**Topics**
+ [Retrieving the Amazon ECS\-optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)
+ [How to Launch the Latest Amazon ECS\-Optimized AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)
+ [Storage Configuration](ecs-ami-storage-config.md)