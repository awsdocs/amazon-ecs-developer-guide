# Amazon ECS\-optimized AMI<a name="ecs-optimized_AMI"></a>

Amazon ECS provides the Amazon ECS\-optimized AMIs that are preconfigured with the requirements and recommendations to run your container workloads on Amazon ECS Linux instances\. We recommend that you use the Amazon ECS\-optimized Amazon Linux 2 AMI for your Amazon EC2 instances unless your application requires a specific operating system or a Docker version that is not yet available in that AMI\.

Although you can create your own Amazon EC2 instance AMI that meets the basic specifications needed to run your containerized workloads on Amazon ECS, the Amazon ECS\-optimized AMIs are preconfigured and tested on Amazon ECS by AWS engineers\. It is the simplest way for you to get started and to get your containers running on AWS quickly\.

The Amazon ECS\-optimized AMI metadata, including the AMI name, Amazon ECS container agent version, and Amazon ECS runtime version which includes the Docker version, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\.

The following variants of the Amazon ECS\-optimized AMI are available for your Amazon EC2 instances\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html)

## Amazon ECS\-optimized AMI changelog<a name="ecs-optimized-ami-linux-releasenotes"></a>

Amazon ECS provides a changelog for the Linux variant of the Amazon ECS\-optimized AMI on GitHub\. For more information, see [Changelog](https://github.com/aws/amazon-ecs-ami/blob/main/CHANGELOG.md)\.

The Linux variants of the Amazon ECS\-optimized AMI use the Amazon Linux 2 AMI as their base\. The Amazon Linux 2 source AMI name for each variant can be retrieved by querying the Systems Manager Parameter Store API\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_AMI.md)\. The Amazon Linux 2 AMI release notes are available as well\. For more information, see [Amazon Linux 2 release notes](http://aws.amazon.com/amazon-linux-2/release-notes/)\.