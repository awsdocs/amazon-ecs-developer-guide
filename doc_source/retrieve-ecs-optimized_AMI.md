# Retrieving Amazon ECS\-Optimized AMI metadata<a name="retrieve-ecs-optimized_AMI"></a>

The AMI ID, image name, operating system, container agent version, source image name, and runtime version for each variant of the Amazon ECS\-optimized AMIs can be programmatically retrieved by querying the Systems Manager Parameter Store API\. For more information about the Systems Manager Parameter Store API, see [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) and [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)\.

**Note**  
Your user account must have the following IAM permissions to retrieve the Amazon ECS\-optimized AMI metadata\. These permissions have been added to the `AmazonECS_FullAccess` IAM policy\.  
ssm:GetParameters
ssm:GetParameter
ssm:GetParametersByPath

## Systems Manager Parameter Store parameter format<a name="ecs-optimized-ami-parameter-format"></a>

The following is the format of the parameter name for each Amazon ECS\-optimized AMI variant\.

**Linux Amazon ECS\-optimized AMIs**
+ Amazon Linux 2 AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/<version>
  ```
+ Amazon Linux 2 \(arm64\) AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/<version>
  ```
+ Amazon Linux 2 \(GPU\) AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/<version>
  ```
+ Amazon Linux 2 \(Inferentia\) AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/inf/<version>
  ```
+ Amazon Linux 2022 AMI metadata:
**Important**  
The Amazon ECS\-Optimized Amazon Linux 2022 AMI is in preview and subject to change\.

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2022/<version>
  ```
+ Amazon Linux AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux/<version>
  ```
**Important**  
The **Amazon ECS\-optimized Amazon Linux AMI** is deprecated as of April 15, 2021\. After that date, Amazon ECS will continue providing critical and important security updates for the AMI but will not add support for new features\.

The following parameter name format retrieves the image ID of the latest stable Amazon ECS\-optimized Amazon Linux 2 AMI by using the sub\-parameter `image_id`\.

```
/aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id
```

The following parameter name format retrieves the metadata of a specific Amazon ECS\-optimized AMI version by specifying the AMI name\.
+ Amazon ECS\-optimized Amazon Linux 2 AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/amzn2-ami-ecs-hvm-2.0.20181112-x86_64-ebs
  ```

**Note**  
All versions of the Amazon ECS\-optimized Amazon Linux 2 AMI are available for retrieval\. Only Amazon ECS\-optimized AMI versions `amzn-ami-2017.09.l-amazon-ecs-optimized` \(Linux\) and later can be retrieved\. For more information, see [Amazon ECS\-optimized AMI versions](ecs-ami-versions.md)\.

## Examples<a name="ecs-optimized-ami-parameter-examples"></a>

The following examples show ways in which you can retrieve the metadata for each Amazon ECS\-optimized AMI variant\.

### Retrieving the metadata of the latest stable Amazon ECS\-optimized AMI<a name="ecs-optimized-ami-parameter-examples-1"></a>

You can retrieve the latest stable Amazon ECS\-optimized AMI using the AWS CLI with the following AWS CLI commands\.

**Linux Amazon ECS\-optimized AMIs**
+ **For the Amazon ECS\-optimized Amazon Linux 2 AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/recommended --region us-east-1
  ```
+ **For the Amazon ECS GPU\-optimized AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/recommended --region us-east-1
  ```
+ **For the Amazon ECS optimized Amazon Linux 2 \(Inferentia\) AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended --region us-east-1
  ```
+ The Amazon ECS\-Optimized Amazon Linux 2022 AMI is in preview and subject to change\.

  **For the Amazon ECS\-optimized Amazon Linux 2022 AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2022/recommended --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Amazon Linux AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended --region us-east-1
  ```
**Important**  
The **Amazon ECS\-optimized Amazon Linux AMI** is deprecated as of April 15, 2021\. After that date, Amazon ECS will continue providing critical and important security updates for the AMI but will not add support for new features\.

### Retrieving the metadata of a specific Amazon ECS\-optimized Amazon Linux 2 AMI version<a name="ecs-optimized-ami-parameter-examples-2"></a>

Retrieve the metadata of a specific Amazon ECS\-optimized Amazon Linux AMI version using the AWS CLI with the following AWS CLI command\. Replace the AMI name with the name of the Amazon ECS\-optimized Amazon Linux AMI to retrieve\. For more information about the available versions, see [Amazon ECS\-optimized AMI versions](ecs-ami-versions.md)\.

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/amzn2-ami-ecs-hvm-2.0.20200928-x86_64-ebs --region us-east-1
```

### Retrieving the Amazon ECS\-optimized Amazon Linux 2 AMI metadata using the Systems Manager GetParametersByPath API<a name="ecs-optimized-ami-parameter-examples-3"></a>

Retrieve the Amazon ECS\-optimized Amazon Linux 2 AMI metadata with the Systems Manager GetParametersByPath API using the AWS CLI with the following command\.

```
aws ssm get-parameters-by-path --path /aws/service/ecs/optimized-ami/amazon-linux-2/ --region us-east-1
```

### Retrieving the image ID of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI<a name="ecs-optimized-ami-parameter-examples-4"></a>

You can retrieve the image ID of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI ID by using the sub\-parameter `image_id`\.

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id --region us-east-1
```

To retrieve the `image_id` value only, you can query the specific parameter value; for example:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id --region us-east-1 --query "Parameters[0].Value"
```

### Using the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template<a name="ecs-optimized-ami-parameter-examples-5"></a>

You can reference the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template by referencing the Systems Manager parameter store name\.

**Linux example**

```
Parameters:
  LatestECSOptimizedAMI:
    Description: AMI ID
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id
```