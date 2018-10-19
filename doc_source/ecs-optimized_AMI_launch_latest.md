# How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI<a name="ecs-optimized_AMI_launch_latest"></a>

**Note**  
Amazon ECS vends Linux AMIs that are optimized for the service in two variants\. The latest and recommended version is based on Amazon Linux 2\. Amazon ECS also vends AMIs that are based on the Amazon Linux AMI, but we recommend that you migrate your workloads to the Amazon Linux 2 variant, as support for the Amazon Linux AMI will end no later than June 30, 2020\. For more information, see [Amazon ECS\-Optimized Amazon Linux 2 AMI](al2ami.md)\.

The following are several ways that you can launch the latest Amazon ECS\-optimized Amazon Linux AMI into your cluster:
+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized Amazon Linux AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.
+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.
+ You can retrieve the current Amazon ECS\-optimized Amazon Linux AMI programmatically using the SSM API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.
+ Use the current Amazon ECS\-optimized Amazon Linux AMI ID below to launch your instance using the AWS CLI, the AWS SDKs, or an AWS CloudFormation template\.

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

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.