# Amazon ECS\-optimized AMIs<a name="ecs-optimized_AMI"></a>

The basic Amazon Elastic Container Service \(Amazon ECS\) container instance specification consists of the following:

**Required**
+ A modern Linux distribution running at least version 3\.10 of the Linux kernel\.
+ The Amazon ECS container agent \(preferably the latest version\)\. For more information, see [Amazon ECS Container Agent](ECS_agent.md)\.
+ A Docker daemon running at least version 1\.9\.0, and any Docker runtime dependencies\. For more information, see [Check runtime dependencies](https://docs.docker.com/engine/installation/binaries/#check-runtime-dependencies) in the Docker documentation\.
**Note**  
For the best experience, we recommend the Docker version that ships with and is tested with the corresponding Amazon ECS agent version that you are using\. For more information, see [Amazon ECS Container Agent Versions](container_agent_versions.md)\.

**Recommended**
+ An initialization and nanny process to run and monitor the Amazon ECS agent\. The Amazon ECS\-optimized AMIs use the `ecs-init` RPM to manage the agent\. For more information, see the [`ecs-init` project](https://github.com/aws/amazon-ecs-init) on GitHub\.

The Amazon ECS\-optimized AMIs are preconfigured with these requirements and recommendations\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your container instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Amazon ECS vends AMIs that are optimized for the service in the following five variants\.
+ **Amazon ECS\-optimized Amazon Linux 2 AMI** – Recommended for launching your Amazon ECS container instances in most cases\.
+ **Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI** – Recommended for launching your Amazon ECS container instances when using the Amazon EC2 A1 instance type, which is powered by Arm\-based AWS Graviton Processors\. For more information, see [General Purpose Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/general-purpose-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ **Amazon ECS GPU\-optimized AMI** – Recommended for launching your Amazon ECS container instances when working with GPU workloads\. For more information, see [Working with GPUs on Amazon ECS](ecs-gpu.md)\.
+ **Amazon ECS\-optimized Amazon Linux AMI** – This AMI is based off of Amazon Linux 1\. We recommend that you migrate your workloads to the Amazon ECS\-optimized Amazon Linux 2 AMI\. Support for the Amazon ECS\-optimized Amazon Linux AMI ends no later than June 30, 2020\.
+ **Amazon ECS\-optimized Windows AMI** – Recommended for launching your Amazon ECS container instances on the Windows operating system\. For more information, see [Windows Containers](ECS_Windows.md)\.

Although you can create your own container instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI Metadata](retrieve-ecs-optimized_AMI.md)\.

View the AMI IDs on one of the following tabs, according to the variant you choose\.

------
#### [ Amazon Linux 2 ]

The current Amazon ECS\-optimized Amazon Linux 2 AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.27.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.27.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-00cffcd24cb08edf1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-00cffcd24cb08edf1) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0bc08634af113cccb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0bc08634af113cccb) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0054160a688deeb6a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0054160a688deeb6a) | 
| us\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-05cc68a00d392447a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-05cc68a00d392447a) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0b4b8274f0c0d3bac | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0b4b8274f0c0d3bac) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-016a20f0624bae8c5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-016a20f0624bae8c5) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-09cd8db92c6bf3a84 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-09cd8db92c6bf3a84) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0ab1db011871746ef | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0ab1db011871746ef) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-036cf93383aba5279 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-036cf93383aba5279) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0470f8828abe82a87 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0470f8828abe82a87) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-00f839709b07ffb58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-00f839709b07ffb58) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-011ce3fbe73731dfe | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-011ce3fbe73731dfe) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0c5b69a05af2f0e23 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0c5b69a05af2f0e23) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-039a05a64b90f63ee | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-039a05a64b90f63ee) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0d143ad35f29ad632 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0d143ad35f29ad632) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-04e333c875fae9d77 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-04e333c875fae9d77) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-04002561557e6b65d | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-04002561557e6b65d) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-c8c6b1a9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-c8c6b1a9) | 

------
#### [ Amazon Linux 2 \(arm64\) ]

The current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI consists of:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.27.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.27.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190403\-arm64\-ebs | ami\-0b7373d6d90afa646 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0b7373d6d90afa646) | 
| us\-east\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190403\-arm64\-ebs | ami\-0a399f59161729e22 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0a399f59161729e22) | 
| us\-west\-2 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190403\-arm64\-ebs | ami\-0db3030cf5c933348 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-0db3030cf5c933348) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-hvm\-2\.0\.20190403\-arm64\-ebs | ami\-0c812cd5f7b956092 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0c812cd5f7b956092) | 

------
#### [ Amazon Linux 2 \(GPU\) ]

The current Amazon ECS GPU\-optimized AMI consists of the following:
+ The latest minimal version of the Amazon Linux 2
+ The latest version of the Amazon ECS container agent \(`1.27.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.27.0-1`\)
+ The recommended NVIDIA driver version \(`"NVIDIA Driver Version": "410.104"`\)
+ The NVIDIA container runtime hook version \(`v1.4.0-1`\)
+ The recommended CUDA version \(`"CUDA Version": "9.2.88"`\)

The following table lists the current Amazon ECS GPU\-optimized AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-083c800fe4211192f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-083c800fe4211192f) | 
| us\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-03e13a3b6e7815f1f | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-03e13a3b6e7815f1f) | 
| us\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-05284b9aa3c01ab6b | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-05284b9aa3c01ab6b) | 
| us\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-03fc1e85125c026d1 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-03fc1e85125c026d1) | 
| eu\-west\-3 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-04a041a6e222beb5c | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-04a041a6e222beb5c) | 
| eu\-west\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0190d74b4f308b3fa | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0190d74b4f308b3fa) | 
| eu\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0638eba79fcfe776e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0638eba79fcfe776e) | 
| eu\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0a06003d7b855bfd3 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-0a06003d7b855bfd3) | 
| eu\-north\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0a1b9d94154abd70a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0a1b9d94154abd70a) | 
| ap\-northeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0b4cb7dc47ed8d637 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-0b4cb7dc47ed8d637) | 
| ap\-northeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0b23c122484f8bf90 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-0b23c122484f8bf90) | 
| ap\-southeast\-2 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-03cc98c2bbe75bc58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-03cc98c2bbe75bc58) | 
| ap\-southeast\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0962019d4f4ce50a9 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-0962019d4f4ce50a9) | 
| ca\-central\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-004fcd34a016ba693 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-004fcd34a016ba693) | 
| ap\-south\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-060e32be63fa1b286 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-060e32be63fa1b286) | 
| sa\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0027082516f6d0cda | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0027082516f6d0cda) | 
| us\-gov\-east\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-0a99d51364f87002a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0a99d51364f87002a) | 
| us\-gov\-west\-1 | amzn2\-ami\-ecs\-gpu\-hvm\-2\.0\.20190402\-x86\_64\-ebs | ami\-58c6b139 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-58c6b139) | 

------
#### [ Amazon Linux AMI ]

The current Amazon ECS\-optimized Amazon Linux AMI consists of:
+ The latest minimal version of the Amazon Linux AMI
+ The latest version of the Amazon ECS container agent \(`1.27.0`\)
+ The recommended version of Docker for the latest Amazon ECS container agent \(`18.06.1-ce`\)
+ The latest version of the `ecs-init` package to run and monitor the Amazon ECS agent \(`1.27.0-1`\)

The following table lists the current Amazon ECS\-optimized Amazon Linux AMI IDs by Region\.


| Region | AMI Name | AMI ID | EC2 Console Link | 
| --- | --- | --- | --- | 
| us\-east\-2 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0351a163d5f20e068 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-2#LaunchInstanceWizard:ami=ami-0351a163d5f20e068) | 
| us\-east\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0be9e1908fe51a590 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard:ami=ami-0be9e1908fe51a590) | 
| us\-west\-2 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-01a82c3fce2c3ba58 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-2#LaunchInstanceWizard:ami=ami-01a82c3fce2c3ba58) | 
| us\-west\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0860832102c806acb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-west-1#LaunchInstanceWizard:ami=ami-0860832102c806acb) | 
| eu\-west\-3 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0a6720df6a8239525 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-3#LaunchInstanceWizard:ami=ami-0a6720df6a8239525) | 
| eu\-west\-2 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0bf096cb3ddbdffe0 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-2#LaunchInstanceWizard:ami=ami-0bf096cb3ddbdffe0) | 
| eu\-west\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0f85d8192f90863dc | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-west-1#LaunchInstanceWizard:ami=ami-0f85d8192f90863dc) | 
| eu\-central\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-02c40c6d994943b85 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-central-1#LaunchInstanceWizard:ami=ami-02c40c6d994943b85) | 
| eu\-north\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0719a9312994ac2cb | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=eu-north-1#LaunchInstanceWizard:ami=ami-0719a9312994ac2cb) | 
| ap\-northeast\-2 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-07ef5505ecca20315 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-2#LaunchInstanceWizard:ami=ami-07ef5505ecca20315) | 
| ap\-northeast\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-047281402e27cf0c5 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard:ami=ami-047281402e27cf0c5) | 
| ap\-southeast\-2 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0f0ba4af21d1e1766 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:ami=ami-0f0ba4af21d1e1766) | 
| ap\-southeast\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-04e47a1e7ce1d448a | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#LaunchInstanceWizard:ami=ami-04e47a1e7ce1d448a) | 
| ca\-central\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0e73429acd3865bd2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ca-central-1#LaunchInstanceWizard:ami=ami-0e73429acd3865bd2) | 
| ap\-south\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0cf953afde70921a2 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=ap-south-1#LaunchInstanceWizard:ami=ami-0cf953afde70921a2) | 
| sa\-east\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0656fd0f926c55fc6 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=sa-east-1#LaunchInstanceWizard:ami=ami-0656fd0f926c55fc6) | 
| us\-gov\-east\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-0c6b0b15e70eb6f3e | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-east-1#LaunchInstanceWizard:ami=ami-0c6b0b15e70eb6f3e) | 
| us\-gov\-west\-1 | amzn\-ami\-2018\.03\.p\-amazon\-ecs\-optimized | ami\-22d3a443 | [Launch instance](https://console.aws.amazon.com/ec2/v2/home?region=us-gov-west-1#LaunchInstanceWizard:ami=ami-22d3a443) | 

------
#### [ Windows ]

The following table lists the current Amazon ECS\-optimized Windows AMI IDs by Region\.


| Region | AMI ID | 
| --- | --- | 
| us\-east\-2 | ami\-00c56e74f090d6f65 | 
| us\-east\-1 | ami\-0ed2f29599018e745 | 
| us\-west\-2 | ami\-07f6b98dc6c8067c3 | 
| us\-west\-1 | ami\-01b55f7fe967f727b | 
| eu\-west\-3 | ami\-06a5b6fc522511993 | 
| eu\-west\-2 | ami\-0fac4f3bdab9ccddc | 
| eu\-west\-1 | ami\-04a2fa8ce0fc20c61 | 
| eu\-central\-1 | ami\-0f7386282aa13a0d8 | 
| eu\-north\-1 | ami\-06e3cb4d2875b172e | 
| ap\-northeast\-2 | ami\-071b78467d9d35580 | 
| ap\-northeast\-1 | ami\-07839df9eec55ac8d | 
| ap\-southeast\-2 | ami\-06d33f81ca8384556 | 
| ap\-southeast\-1 | ami\-0a6c13d83c0fdbf2b | 
| ca\-central\-1 | ami\-098ad73a3005be676 | 
| ap\-south\-1 | ami\-0183732d8e0fd56c7 | 
| sa\-east\-1 | ami\-05889298c47e6d5c2 | 
| us\-gov\-east\-1 | ami\-0b6f703732ae49d69 | 
| us\-gov\-west\-1 | ami\-9d91fafc | 

------

**Topics**
+ [How to Launch the Latest Amazon ECS\-Optimized Amazon Linux AMI](ecs-optimized_AMI_launch_latest.md)
+ [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)
+ [AMI Storage Configuration](ecs-ami-storage-config.md)