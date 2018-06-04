# Retrieving the Amazon ECS\-optimized AMI Metadata<a name="retrieve-ecs-optimized_AMI"></a>

The AMI ID, image name, operating system, container agent version, and runtime version for an Amazon ECS\-optimized AMI can be programmatically retrieved by querying the SSM Parameter Store API\. For more information about the SSM Parameter Store API, see [GetParameters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) and [GetParametersByPath](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)\.

**Note**  
Your user account must have the following IAM permissions to retrieve the Amazon ECS\-optimized AMI metadata\. These permissions have been added to the `AmazonECS_FullAccess` IAM policy\.  
ssm:GetParameters
ssm:GetParameter
ssm:GetParametersByPath

The following is the format of the parameter name\.
+ Linux Amazon ECS\-optimized AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux/<version>
  ```
+ Windows Amazon ECS\-optimized AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/<os family>/<os version>/<os locale>/<os sku>/<version>
  ```

The following parameter name format retrieves the metadata of the latest stable Amazon ECS\-optimized AMI by using `recommended`\.

```
/aws/service/ecs/optimized-ami/amazon-linux/recommended
```

The following is an example of the JSON object that is returned for the parameter value\.

```
{
    "schema_version": 1,
    "image_id": "ami-aff65ad2",
    "image_name": "amzn-ami-2017.09.l-amazon-ecs-optimized",
    "os": "Amazon Linux",
    "ecs_agent_version": "1.17.3",
    "ecs_runtime_version": "Docker version 17.12.1-ce"
}
```

Each of the fields in the output above are available to be queried as sub\-parameters\. Construct the parameter path for a sub\-parameter by appending the sub\-parameter name to the path for the selected AMI\. The following sub\-parameters are available:
+ `schema_version`
+ `image_id`
+ `image_name`
+ `os`
+ `ecs_agent_version`
+ `ecs_runtime_version`

The following parameter name format retrieves the image ID of the latest stable Linux Amazon ECS\-optimized AMI by using the sub\-parameter `image_id`\.

```
/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id
```

The following parameter name format retrieves the metadata of a specific Amazon ECS\-optimized AMI version by specifying the AMI name\.
+ Linux Amazon ECS\-optimized AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/amazon-linux/amzn-ami-2017.09.l-amazon-ecs-optimized
  ```
+ Windows Amazon ECS\-optimized AMI metadata:

  ```
  /aws/service/ecs/optimized-ami/windows_server/2016/english/full/2018.03.26
  ```

**Note**  
Only Amazon ECS\-optimized AMI versions `amzn-ami-2017.09.l-amazon-ecs-optimized` \(Linux\) and `Windows_Server-2016-English-Full-ECS_Optimized-2018.03.26` \(Windows\) and later can be retrieved\. For more information, see [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)\.

**Example retrieving the metadata of the latest stable Amazon ECS\-optimized AMI**  
You can retrieve the latest stable Amazon ECS\-optimized AMI using the AWS CLI with the following AWS CLI command\.  
+ **For the Linux Amazon ECS\-optimized AMIs:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended --region us-east-1
  ```

  Output:

  ```
  {
      "Parameters": [
          {
              "Name": "/aws/service/ecs/optimized-ami/amazon-linux/recommended",
              "Type": "String",
              "Value": "{\"schema_version\":1,\"image_name\":\"amzn-ami-2017.09.l-amazon-ecs-optimized\",\"image_id\":\"ami-aff65ad2\",\"os\":\"Amazon Linux\",\"ecs_runtime_version\":\"Docker version 17.12.1-ce\",\"ecs_agent_version\":\"1.17.3\"}",
              "Version": 21
          }
      ],
      "InvalidParameters": []
  }
  ```
+ **For the Windows Amazon ECS\-optimized AMI:**

  ```
  aws ssm get-parameters --names /aws/service/ecs/optimized-ami/windows_server/2016/english/full/recommended --region us-east-1
  ```

  Output:

  ```
  {
      "Parameters": [
          {
              "Name": "/aws/service/ecs/optimized-ami/windows_server/2016/english/full/recommended",
              "Type": "String",
              "Value": "{\"schema_version\":1,\"os\":\"Windows_Server-2016-English-Full\",\"image_id\":\"ami-c014cbbd\",\"ecs_agent_version\":\"1.17.2\",\"ecs_runtime_version\":\"Docker version 17.06.2-ee-6, build e75fdb8\",\"image_name\":\"Windows_Server-2016-English-Full-ECS_Optimized-2018.03.26\"}",
              "Version": 1
          }
      ],
      "InvalidParameters": []
  }
  ```
+ **For the Windows Amazon ECS\-optimized AMI using AWS PowerShell:**

  ```
   Get-SSMParameter -Name /aws/service/ecs/optimized-ami/windows_server/2016/english/full/recommended/image_id -region us-east-1
  ```
  
  Output:
  
  ```
  Name                                                                                 Type   Value        Version
  ----                                                                                 ----   -----        -------
  /aws/service/ecs/optimized-ami/windows_server/2016/english/full/recommended/image_id String ami-4734a738 3
  ```

**Example retrieving the metadata of a specific Amazon ECS\-optimized AMI version**  
Retrieve the metadata of a specific Amazon ECS\-optimized AMI version using the AWS CLI with the following AWS CLI command\. Replace the AMI name with the name of the Amazon ECS\-optimized AMI to retrieve\. For more information about the available versions, see [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)\.  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/amzn-ami-2017.09.l-amazon-ecs-optimized --region us-east-1
```

**Example retrieving the Amazon ECS\-optimized AMI metadata using the SSM GetParametersByPath API**  
Retrieve the Amazon ECS\-optimized AMI metadata with the SSM GetParametersByPath API using the AWS CLI with the following command\.  

```
aws ssm get-parameters-by-path --path /aws/service/ecs/optimized-ami/amazon-linux/ --region us-east-1
```

**Example retrieving the image ID of the latest recommended Amazon ECS\-optimized AMI**  
You can retrieve the image ID of the latest recommended Amazon ECS\-optimized AMI ID by using the sub\-parameter `image_id`\.  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id --region us-east-1
```
Output:  

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id",
            "Type": "String",
            "Value": "ami-f9ac2f86",
            "Version": 1
        }
    ],
    "InvalidParameters": []
}
```
To retrieve the `image_id` value only, you can query the specific parameter value; for example:  

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id --region us-east-1 --query "Parameters[0].Value"
```
Output:  

```
"ami-f9ac2f86"
```

**Example using the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template**  
You can retrieve the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template by referencing the SSM parameter store name; for example:  
Linux:  

```
Parameters:
  ECSAMI:
    Description: AMI ID
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ecs/optimized-ami/amazon-linux/recommended/image_id
```
Windows:  

```
Parameters:
  ECSAMI:
    Description: AMI ID
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ecs/optimized-ami/windows_server/2016/english/full/recommended/image_id
```
