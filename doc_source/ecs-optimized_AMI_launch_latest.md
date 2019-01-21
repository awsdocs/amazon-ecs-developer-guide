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

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.