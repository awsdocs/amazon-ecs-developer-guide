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
| us\-east\-2 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0307f7ccf6ea35750 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0307f7ccf6ea35750) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-045f1b3f87ed83659 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-045f1b3f87ed83659) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-01b70aea4161476b7 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-01b70aea4161476b7) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0285183bbef6224bd | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0285183bbef6224bd) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0f4738fbeb53e6c3a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0f4738fbeb53e6c3a) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-01bee3897bba49d78 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-01bee3897bba49d78) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0627e141ce928067c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0627e141ce928067c) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0eaa3baf6969912ba | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0eaa3baf6969912ba) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-00294948a592fc052 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-00294948a592fc052) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-05b296a384694dfa4 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-05b296a384694dfa4) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-02c73ee1100ce3e7a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-02c73ee1100ce3e7a) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-050865a806e0dae53 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-050865a806e0dae53) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-0f552e0a86f08b660 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0f552e0a86f08b660) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-01ef9f6a829ae3956 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-01ef9f6a829ae3956) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-084b1eee100c102ee | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-084b1eee100c102ee) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.i\-amazon\-ecs\-optimized | ami\-1dafcb7c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-1dafcb7c) | 

 For more information about previous versions and the corresponding Docker and Amazon ECS container agent versions, see [Amazon ECS\-Optimized Amazon Linux AMI Container Agent Versions](container_agent_versions.md#ecs-optimized-ami-agent-versions)\.