# Creating Amazon ECS resources with AWS CloudFormation<a name="creating-resources-with-cloudformation"></a>

Amazon ECS is integrated with AWS CloudFormation, a service that you can use to model and set up AWS resources with templates that you define\. This way, you can spend less time creating and managing your resources and infrastructure\. Using AWS CloudFormation, you can create a template that describes all the AWS resources that you want, such as specific Amazon ECS clusters\. Then, AWS CloudFormation takes care of provisioning and configuring those resources for you\. 

When you use AWS CloudFormation, you can reuse your template to set up your Amazon ECS resources in a consistent and repeatable manner\. You describe your resources one time, and then provision the same resources again across multiple AWS accounts and AWS Regions\. 

## Amazon ECS and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure resources for Amazon ECS and related services, make sure that you're familiar with [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. AWS CloudFormation templates are text files in the JSON or YAML format that describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with either the JSON or YAML format, or both, you can use AWS CloudFormation Designer to get started using AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

Amazon ECS supports creating clusters, task definitions, services, and task sets  in AWS CloudFormation\. The following examples demonstrate how to create resources with these templates using the AWS CLI\. You can also create these resources using the AWS CloudFormation console\. For more information about how to create resources using the AWS CloudFormation console, see the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.Walkthrough.html)\. 

## Example templates<a name="examples"></a>

### Creating Amazon ECS resources using separate stacks<a name="create-separate"></a>

The following examples show how to create Amazon ECS resources by using separate stacks for each resource\.

#### Amazon ECS task definitions<a name="cfn-task-definition"></a>

You can use the following template to create a Fargate Linux task\.

**JSON**

```
 {
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "taskdefinition": {
            "Type": "AWS::ECS::TaskDefinition",
            "Properties": {
                    "containerDefinitions": [ 
                       { 
                          "command": [
                             "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
                          ],
                          "entryPoint": [
                             "sh",
                             "-c"
                          ],
                          "essential": true,
                          "image": "httpd:2.4",
                          "logConfiguration": { 
                             "logDriver": "awslogs",
                             "options": { 
                                "awslogs-group" : "/ecs/fargate-task-definition",
                                "awslogs-region": "us-east-1",
                                "awslogs-stream-prefix": "ecs"
                             }
                          },
                          "name": "sample-fargate-app",
                          "portMappings": [ 
                             { 
                                "containerPort": 80,
                                "hostPort": 80,
                                "protocol": "tcp"
                             }
                          ]
                       }

                    ],
                    "cpu":"256",
                    "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole",
                    "family": "fargate-task-definition-cfn",
                    "memory":"512",
                    "networkMode": "awsvpc",
                    "requiresCompatibilities": [ 
                        "FARGATE" 
                     ],
                    "runtimePlatform": {
                        "OperatingSystemFamily": "LINUX"
                   }
                 }
                
            }
        }
    }
```

**YAML**

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  taskdefinition:
    Type: 'AWS::ECS::TaskDefinition'
    Properties:
      containerDefinitions:
        - command:
            - >-
              /bin/sh -c "echo '<html> <head> <title>Amazon ECS Sample
              App</title> <style>body {margin-top: 40px; background-color:
              #333;} </style> </head><body> <div
              style=color:white;text-align:center> <h1>Amazon ECS Sample
              App</h1> <h2>Congratulations!</h2> <p>Your application is now
              running on a container in Amazon ECS.</p> </div></body></html>' > 
              /usr/local/apache2/htdocs/index.html && httpd-foreground"
          entryPoint:
            - sh
            - '-c'
          essential: true
          image: 'httpd:2.4'
          logConfiguration:
            logDriver: awslogs
            options:
              awslogs-group: /ecs/fargate-task-definition
              awslogs-region: us-east-1
              awslogs-stream-prefix: ecs
          name: sample-fargate-app
          portMappings:
            - containerPort: 80
              hostPort: 80
              protocol: tcp
      cpu: '256'
      executionRoleArn: 'arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole'
      family: fargate-task-definition-cfn
      memory: '512'
      networkMode: awsvpc
      requiresCompatibilities:
        - FARGATE
      runtimePlatform:
        OperatingSystemFamily: LINUX
```

#### Amazon ECS clusters<a name="create-clusters"></a>

You can use the following template to create an empty cluster\.

**JSON**

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "ECSCluster": {
            "Type": "AWS::ECS::Cluster",
            "Properties": {
                "ClusterName": "MyEmptyCluster"
            }
        }
    }
}
```

**YAML**

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  ECSCluster:
    Type: 'AWS::ECS::Cluster'
    Properties:
      ClusterName: MyEmptyCluster
```

### Creating multiple Amazon ECS resources in one stack<a name="multiple-resources"></a>

You can use the following example template to create multiple Amazon ECS resources in one stack\. The template creates an Amazon ECS cluster that's named `CFNCluster`\. The cluster contains a Linux Fargate task definition\. The template also creates a service that's named `cfn-service` that launches and maintains the Fargate task definition\. Before you use this command, make sure that the subnet and security group IDs in the service's `NetworkConfiguration` all belong to the same VPC\. 

**JSON**

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "ECSCluster": {
            "Type": "AWS::ECS::Cluster",
            "Properties": {
                "ClusterName": "CFNCluster"
            }
        },
        "taskdefinition": {
            "Type": "AWS::ECS::TaskDefinition",
            "Properties": {
                "containerDefinitions": [
                    {
                        "command": [
                            "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
                        ],
                        "entryPoint": [
                            "sh",
                            "-c"
                        ],
                        "essential": true,
                        "image": "httpd:2.4",
                        "logConfiguration": {
                            "logDriver": "awslogs",
                            "options": {
                                "awslogs-group": "/ecs/fargate-task-definition",
                                "awslogs-region": "us-east-1",
                                "awslogs-stream-prefix": "ecs"
                            }
                        },
                        "name": "sample-fargate-app",
                        "portMappings": [
                            {
                                "containerPort": 80,
                                "hostPort": 80,
                                "protocol": "tcp"
                            }
                        ]
                    }
                ],
                "cpu": "256",
                "executionRoleArn": "arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole",
                "family": "task-definition-cfn",
                "memory": "512",
                "networkMode": "awsvpc",
                "requiresCompatibilities": [
                    "FARGATE"
                ],
                "runtimePlatform": {
                    "OperatingSystemFamily": "LINUX"
                }
            }
        },
        "ECSService": {
            "Type": "AWS::ECS::Service",
            "Properties": {
                "ServiceName": "cfn-service",
                "Cluster": {
                    "Ref": "ECSCluster"
                },
                "DesiredCount": 1,
                "LaunchType": "FARGATE",
                "NetworkConfiguration": {
                    "AwsvpcConfiguration": {
                        "AssignPublicIp": "ENABLED",
                        "SecurityGroups": [
                            "sg-abcdef01234567890"
                        ],
                        "Subnets": [
                            "subnet-abcdef01234567890"
                        ]
                    }
                },
                "TaskDefinition": {
                    "Ref": "taskdefinition"
                }
            }
        }
    }
}
```

**YAML**

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  ECSCluster:
    Type: 'AWS::ECS::Cluster'
    Properties:
      ClusterName: CFNCluster
  taskdefinition:
    Type: 'AWS::ECS::TaskDefinition'
    Properties:
      containerDefinitions:
        - command:
            - >-
              /bin/sh -c "echo '<html> <head> <title>Amazon ECS Sample
              App</title> <style>body {margin-top: 40px; background-color:
              #333;} </style> </head><body> <div
              style=color:white;text-align:center> <h1>Amazon ECS Sample
              App</h1> <h2>Congratulations!</h2> <p>Your application is now
              running on a container in Amazon ECS.</p> </div></body></html>' > 
              /usr/local/apache2/htdocs/index.html && httpd-foreground"
          entryPoint:
            - sh
            - '-c'
          essential: true
          image: 'httpd:2.4'
          logConfiguration:
            logDriver: awslogs
            options:
              awslogs-group: /ecs/fargate-task-definition
              awslogs-region: us-east-1
              awslogs-stream-prefix: ecs
          name: sample-fargate-app
          portMappings:
            - containerPort: 80
              hostPort: 80
              protocol: tcp
      cpu: '256'
      executionRoleArn: 'arn:aws:iam::aws_account_id:role/ecsTaskExecutionRole'
      family: task-definition-cfn
      memory: '512'
      networkMode: awsvpc
      requiresCompatibilities:
        - FARGATE
      runtimePlatform:
        OperatingSystemFamily: LINUX
  ECSService:
    Type: 'AWS::ECS::Service'
    Properties:
      ServiceName: cfn-service
      Cluster: !Ref ECSCluster
      DesiredCount: 1
      LaunchType: FARGATE
      NetworkConfiguration:
        AwsvpcConfiguration:
          AssignPublicIp: ENABLED
          SecurityGroups:
            - sg-abcdef01234567890
          Subnets:
            - subnet-abcdef01234567890
      TaskDefinition: !Ref taskdefinition
```

## Using the AWS CLI to create resources from templates<a name="using-cli"></a>

The following command creates a stack that's named `ecs-stack` using a template body file that's named `ecs-template-body.json`\. Ensure that the template body file is in the JSON or YAML format\. The location of the file is specified in the `--template-body` parameter\. In this case, the template body file is located in the current directory\.

```
aws cloudformation create-stack \
      --stack-name ecs-stack \
      --template-body file://ecs-template-body.json
```

To ensure that resources are created correctly, check the Amazon ECS console or alternatively use the following commands:
+ The following command lists all task definitions\.

  ```
  aws ecs list-task-definitions
  ```
+ The following command lists all clusters\. 

  ```
  aws ecs list-clusters
  ```
+ The following command lists all services defined in the cluster `CFNCluster`\. Replace `CFNCluster` with the name of the cluster that you wanted to create the service in\. 

  ```
  aws ecs list-services \
        --cluster CFNCluster
  ```

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](http://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation Command Line Interface User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)