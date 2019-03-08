# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI_launch_latest"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The following are several ways that you can launch the latest Amazon ECS\-optimized Amazon Linux AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized Amazon Linux AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized Amazon Linux AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized Amazon Linux AMI ID below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

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

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.