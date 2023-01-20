# Amazon ECS\-optimized Linux AMI build script<a name="ecs-ami-build-scripts"></a>

Amazon ECS has open\-sourced the build scripts that are used to build the Linux variants of the Amazon ECS\-optimized AMI\. These build scripts are now available on GitHub\. For more information, see [amazon\-ecs\-ami](https://github.com/aws/amazon-ecs-ami) on GitHub\.

The build scripts repository includes a [HashiCorp packer](https://www.packer.io/) template and build scripts to generate each of the Linux variants of the Amazon ECS\-optimized AMI\. These scripts are the source of truth for Amazon ECS\-optimized AMI builds, so you can follow the GitHub repository to monitor changes to our AMIs\. For example, perhaps you want your own AMI to use the same version of Docker that the Amazon ECS team uses for the official AMI\.

For more information, see the Amazon ECS AMI repository at [aws/amazon\-ecs\-ami](https://github.com/aws/amazon-ecs-ami) on GitHub\.

**To build an Amazon ECS\-optimized Linux AMI**

1. Clone the `aws/amazon-ecs-ami` GitHub repo\.

   ```
   git clone https://github.com/aws/amazon-ecs-ami.git
   ```

1. Add an environment variable for the AWS Region to use when creating the AMI\. Replace the `us-west-2` value with the Region to use\.

   ```
   export REGION=us-west-2
   ```

1. A Makefile is provided to build the AMI\. From the root directory of the cloned repository, use one of the following commands, corresponding to the Linux variant of the Amazon ECS\-optimized AMI you want to build\.
   + Amazon ECS\-optimized Amazon Linux 2 AMI

     ```
     make al2
     ```
   + Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI

     ```
     make al2arm
     ```
   + Amazon ECS GPU\-optimized AMI

     ```
     make al2gpu
     ```
   + Amazon ECS optimized Amazon Linux 2 \(Inferentia\) AMI

     ```
     make al2inf
     ```
   + Amazon ECS\-optimized Amazon Linux AMI

     ```
     make al1
     ```
**Important**  
On April 15, 2021, the Amazon ECS\-optimized Amazon Linux AMI ended its standard support phase and entered a maintenance support phase\. In the maintenance support phase, Amazon ECS will continue providing critical and important security updates for a reduced list of packages\. During this period, Amazon ECS will no longer add support for new EC2 instance types, new services and features, and new packages\. Instead, Amazon ECS will provide updates only for critical and important security fixes that apply to a reduced set of packages\. Maintenance support period will end on June 30, 2023\.