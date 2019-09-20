# Retrieving Amazon ECS\-Optimized AMI Metadata<a name="retrieve-ecs-optimized_AMI"></a>

You can programmatically retrieve the Amazon Machine Image \(AMI\) ID, image name, operating system, container agent version, and runtime version for the Amazon ECS\-optimized AMIs by querying the SSM Parameter Store API\. These parameters eliminate the need for you to manually look up Amazon ECS\-optimized AMI IDs\. For more information about the Systems Manager Parameter Store API, see [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) and [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)\.

The following is the format of the parameter name\.
+ Amazon ECS\-optimized Amazon Linux 2 AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/<version>
  ```
+ Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/<version>
  ```
+ Amazon ECS GPU\-optimized AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/gpu/<version>
  ```
+ Amazon ECS\-optimized Amazon Linux AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux/<version>
  ```
+  Amazon ECS\-optimized Windows 2019 AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
  ```
+  Amazon ECS\-optimized Windows 2016 AMI metadata:

  ```
  /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
  ```

## Required IAM Permissions<a name="ami-retrieve-iam-permissions"></a>

To retrieve the Amazon ECS\-optimized AMI metadata, you must have the following IAM permissions\. These permissions have been added to the `AmazonECS_FullAccess` IAM policy\.
+ ssm:GetParameters
+ ssm:GetParameter
+ ssm:GetParametersByPath

## Working With The SSM Parameter Format<a name="ami-retrieve-parameter-format"></a>

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
+ Amazon ECS\-optimized Amazon Linux 2 \(arm64\) AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux-2/arm64/amzn2-ami-ecs-hvm-2.0.20181120-arm64-ebs
  ```
+ Amazon ECS\-optimized Amazon Linux AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux/amzn-ami-2017.09.l-amazon-ecs-optimized
  ```

**Note**  
All versions of the Amazon ECS\-optimized Amazon Linux 2 AMI are available for retrieval\. Only Amazon ECS\-optimized AMI versions `amzn-ami-2017.09.l-amazon-ecs-optimized` \(Linux\) and later can be retrieved\. For more information, see [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)\.

## Examples<a name="ami-retrieve-ssm-examples"></a>

The following are examples showing how to retrieve the AMI ID and other details about the Amazon ECS\-optimized AMIs\.

**Example Retrieve the full metadata of the latest recommended Amazon ECS\-optimized AMI**  
You can retrieve the latest recommended Amazon ECS\-optimized AMI using the AWS CLI with the following command\.  
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
+ **For the Amazon ECS\-optimized Amazon Linux AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows 2019 AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized --region us-east-1
  ```
+ **For the Amazon ECS\-optimized Windows 2016 AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized --region us-east-1
  ```

**Example Retrieve the metadata of a specific Amazon ECS\-optimized Amazon Linux 2 AMI version**  
You can retrieve the metadata of a specific Amazon ECS\-optimized Amazon Linux 2 AMI using the AWS CLI with the following command\. Replace the AMI name with the name of the Amazon ECS\-optimized Amazon Linux 2 AMI to retrieve\. For more information about the available versions, see [Amazon ECS\-optimized AMI Versions](ecs-ami-versions.md)\.  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/amzn2-ami-ecs-hvm-2.0.20190913-x86_64-ebs --region us-east-1
```

**Example Retrieve the Amazon ECS\-optimized Amazon Linux 2 AMI metadata using the Systems Manager GetParametersByPath API**  
You can retrieve the Amazon ECS\-optimized Amazon Linux 2 AMI metadata with the Systems Manager GetParametersByPath API using the AWS CLI with the following command\.  

```
aws ssm get-parameters-by-path --path /aws/service/ecs/optimized-ami/amazon-linux-2/ --region us-east-1
```

**Example Retrieve the image ID of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI**  
You can retrieve the image ID of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI ID by using the sub\-parameter `image_id`\.  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id --region us-east-1
```
To retrieve the `image_id` value only, you can query the specific parameter value; for example:  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id --region us-east-1 --query "Parameters[0].Value"
```

**Example Retrieve the ECS agent version of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI**  
You can retrieve the ECS agent version of the latest recommended Amazon ECS\-optimized Amazon Linux 2 AMI ID by using the sub\-parameter `image_id`\.  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/ecs_agent_version --region us-east-1
```
To retrieve the `ecs_agent_version` value only, you can query the specific parameter value; for example:  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended/ecs_agent_version --region us-east-1 --query "Parameters[0].Value"
```

**Example Use the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template**  
You can retrieve the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template by referencing the Systems Manager parameter store name; for example:  
Amazon Linux 2:  

```
Parameters:
  ECSAMI:
    Description: AMI ID
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ecs/optimized-ami/amazon-linux-2/recommended/image_id
```
Referencing the Amazon ECS\-optimized Windows AMIs in a AWS CloudFormation template is not supported\.