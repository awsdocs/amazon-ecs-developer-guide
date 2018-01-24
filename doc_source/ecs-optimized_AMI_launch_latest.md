# How to Launch the Latest Amazon ECS\-Optimized AMI<a name="ecs-optimized_AMI_launch_latest"></a>

The following are several ways that you can launch the latest Amazon ECS\-optimized AMI into your cluster:

+ The Amazon ECS [console first\-run wizard](https://console.aws.amazon.com/ecs/home#/firstRun) launches your container instances with the latest Amazon ECS\-optimized AMI\. For more information, see [Getting Started with Amazon ECS using Fargate](ECS_GetStarted.md)\.

+ You can launch your container instances manually in the Amazon EC2 console by following the procedures in [Launching an Amazon ECS Container Instance](launch_container_instance.md)\. You could also choose the **EC2 console link** in the table below that corresponds to your cluster's region\.

+ Use an **AMI ID** from the table below that corresponds to your cluster's region with the AWS CLI, the AWS SDKs, or an AWS CloudFormation template to launch your instances\. 

The current Amazon ECS\-optimized Linux AMI IDs by region are listed below for reference\.


| Region | AMI Name | AMI ID | EC2 console link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-ce1c36ab | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-ce1c36ab) | 
| us\-east\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-28456852 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-28456852) | 
| us\-west\-2 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-decc7fa6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-decc7fa6) | 
| us\-west\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-74262414 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-74262414) | 
| eu\-west\-3 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-9aef59e7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-9aef59e7) | 
| eu\-west\-2 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-67cbd003 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-67cbd003) | 
| eu\-west\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-1d46df64 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-1d46df64) | 
| eu\-central\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-509a053f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-509a053f) | 
| ap\-northeast\-2 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-c212b2ac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-c212b2ac) | 
| ap\-northeast\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-872c4ae1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-872c4ae1) | 
| ap\-southeast\-2 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-58bb443a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-58bb443a) | 
| ap\-southeast\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-910d72ed | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-910d72ed) | 
| ca\-central\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-435bde27 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-435bde27) | 
| ap\-south\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-00491f6f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-00491f6f) | 
| sa\-east\-1 | amzn\-ami\-2017\.09\.g\-amazon\-ecs\-optimized | ami\-af521fc3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-af521fc3) | 

 For previous versions of the Amazon ECS\-optimized AMI and its corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.