# Retrieving Amazon ECS\-Optimized AMI metadata<a name="retrieve-ecs-optimized_windows_AMI"></a>

The AMI ID, image name, operating system, container agent version, and runtime version for each variant of the Amazon ECS\-optimized AMIs can be programmatically retrieved by querying the Systems Manager Parameter Store API\. For more information about the Systems Manager Parameter Store API, see [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) and [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)\.

**Note**  
Your administrative user must have the following IAM permissions to retrieve the Amazon ECS\-optimized AMI metadata\. These permissions have been added to the `AmazonECS_FullAccess` IAM policy\.  
ssm:GetParameters
ssm:GetParameter
ssm:GetParametersByPath

## Systems Manager Parameter Store parameter format<a name="ecs-optimized-ami-parameter-format"></a>

The following is the format of the parameter name for each Amazon ECS\-optimized AMI variant\.
+ Windows Server 2022 Full AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2022-English-Full-ECS_Optimized
  ```
+ Windows Server 2022 Core AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2022-English-Core-ECS_Optimized
  ```
+ Windows Server 2019 Full AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
  ```
+ Windows Server 2019 Core AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized
  ```
+ Windows Server 2016 Full AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
  ```

The following parameter name format retrieves the metadata of the latest stable Amazon ECS\-optimized Amazon Linux 2 AMI by using `recommended`\.

```
/aws/service/ecs/optimized-ami/amazon-linux-2/recommended
```

The following is an example of the JSON object that is returned for the parameter value\.

```
{
	"schema_version": 1,
	"image_name": "amzn2-ami-ecs-hvm-2.0.20181017-x86_64-ebs",
	"image_id": "ami-04a4fb062c609f55b",
	"os": "Amazon Linux 2",
	"ecs_runtime_version": "Docker version 18.06.1-ce",
	"ecs_agent_version": "1.21.0"
}
```

Each of the fields in the output above are available to be queried as sub\-parameters\. Construct the parameter path for a sub\-parameter by appending the sub\-parameter name to the path for the selected AMI\. The following sub\-parameters are available:
+ `schema_version`
+ `image_id`
+ `image_name`
+ `os`
+ `ecs_agent_version`
+ `ecs_runtime_version`

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

## Examples<a name="ecs-optimized-ami-windows-parameter-examples"></a>

The following examples show ways in which you can retrieve the metadata for each Amazon ECS\-optimized AMI variant\.

### Retrieving the metadata of the latest stable Amazon ECS\-optimized AMI<a name="ecs-optimized-ami-windows-parameter-examples-1"></a>

You can retrieve the latest stable Amazon ECS\-optimized AMI using the AWS CLI with the following AWS CLI commands\.
+ **For the Amazon ECS\-optimized Windows Server 2022 Full AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2022-English-Full-ECS_Optimized --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows Server 2022 Core AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2022-English-Core-ECS_Optimized --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows Server 2019 Full AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows Server 2019 Core AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows Server 2016 Full AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized --region us-east-1
  ```

### Using the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template<a name="ecs-optimized-ami-windows-parameter-examples-5"></a>

You can reference the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template by referencing the Systems Manager parameter store name\.

```
Parameters:
  LatestECSOptimizedAMI:
    Description: AMI ID
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized/image_id
```