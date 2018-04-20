# Retrieving the Amazon ECS\-optimized AMI Metadata<a name="retrieve-ecs-optimized_AMI"></a>

The AMI ID, image name, container agent version, and runtime version for an Amazon ECS\-optimized AMI can be programmatically retrieved by querying the SSM Parameter Store API\. For more information about the SSM Parameter Store API, see [GetParameters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) and [GetParametersByPath](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)\.

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

**To retrieve the metadata of the latest stable Amazon ECS\-optimized AMI**

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

**To retrieve the metadata of a specific Amazon ECS\-optimized AMI version**

Retrieve the metadata of a specific Amazon ECS\-optimized AMI version using the AWS CLI with the following AWS CLI command\. Replace the AMI name with the name of the Amazon ECS\-optimized AMI to retrieve\. For more information about the available versions, see [Amazon ECS\-Optimized AMI Versions](ecs-ami-versions.md)\.

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/amzn-ami-2017.09.l-amazon-ecs-optimized --region us-east-1
```

**To retrieve the Amazon ECS\-optimized AMI metadata using the SSM GetParametersByPath API**

Retrieve the Amazon ECS\-optimized AMI metadata with the SSM GetParametersByPath API using the AWS CLI with the following command\.

```
aws ssm get-parameters-by-path --path /aws/service/ecs/optimized-ami/amazon-linux/ --region us-east-1
```

**To retrieve the Amazon ECS\-optimized AMI ID by using the jq tool**

You can retrieve the Amazon ECS\-optimized AMI ID by parsing the output of the parameter store\. The following method uses the jq tool\.

1. Download and install the jq tool from [github](https://stedolan.github.io/jq/)\.

1. Using the jq tool, the following AWS CLI command will parse the JSON output to provide the Amazon ECS\-optimized AMI ID\.

   ```
   aws ssm get-parameter --name /aws/service/ecs/optimized-ami/amazon-linux/recommended --region us-east-1 --query 'Parameter.Value' | jq 'fromjson.image_id'
   ```

**To retrieve the Amazon ECS\-optimized AMI ID by using the AWS SDK for Python**

You can retrieve the Amazon ECS\-optimized AMI ID by parsing the output of the parameter store\. The following method uses the AWS SDK for Python\.

1. Create a file named `ecsami.py` with the contents of the following python script\. Replace the `parameter_name` and `aws_region` parameters as necessary\.

   ```
   #!/usr/bin/env python
   import json
   import botocore.session
   
   parameter_name = '/aws/service/ecs/optimized-ami/amazon-linux/recommended'
   aws_region = 'us-east-1'
   
   session = botocore.session.Session()
   client = session.create_client(service_name='ssm', region_name=aws_region)
   ami_id = json.loads(client.get_parameter(Name=parameter_name)['Parameter']['Value'])['image_id']
   print(ami_id)
   ```

1. Run the python script\.

   ```
   python ecsami.py
   ```