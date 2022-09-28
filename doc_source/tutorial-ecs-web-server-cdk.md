# Getting started with Amazon ECS using the AWS CDK<a name="tutorial-ecs-web-server-cdk"></a>

The AWS Cloud Development Kit \(AWS CDK\) is an Infrastructure\-as\-Code \(IAC\) framework that you can use to define AWS cloud infrastructure by using a programming language of your choosing\. To define your own cloud infrastructure, you first write an app \(in one of the CDK's supported languages\) that contains one or more stacks\. Then, you synthesize it to an AWS CloudFormation template and deploy your resources to your AWS account\. Follow the steps in this topic to deploy a containerized web server with Amazon Elastic Container Service \(Amazon ECS\) and the AWS CDK on Fargate\. 

The AWS Construct Library, included with the CDK, provides modules that you can use to model the resources that AWS services provide\. For popular services, the library provides curated constructs with smart defaults and best practices\. One of these modules, specifically `[aws\-ecs\-patterns](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs_patterns-readme.html)`, provides high\-level abstractions that you can use to define your containerized service and all the necessary supporting resources in a few lines of code\.

This topic uses the [https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs_patterns.ApplicationLoadBalancedFargateService.html](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs_patterns.ApplicationLoadBalancedFargateService.html) construct\. This construct deploys an Amazon ECS service on Fargate behind an application load balancer\. The `aws-ecs-patterns` module also includes constructs that use a network load balancer and run on Amazon EC2\.

Before starting this task, set up your AWS CDK development environment, and install the AWS CDK by running the following command\. For instructions on how to set up your AWS CDK development environment, see [Getting Started With the AWS CDK \- Prerequisites](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html#getting_started_prerequisites)\.

```
npm install -g aws-cdk
```

**Note**  
These instructions assume you are using AWS CDK v2\. 

**Topics**
+ [Step 1: Set up your AWS CDK project](#ecs-web-server-cdk-step-1)
+ [Step 2: Use the AWS CDK to define a containerized web server on Fargate](#ecs-web-server-cdk-step-2)
+ [Step 3: Test the web server](#ecs-web-server-cdk-step-3)
+ [Step 4: Clean up](#ecs-web-server-cdk-step-4)
+ [Next steps](#ecs-web-server-cdk-next-steps)

## Step 1: Set up your AWS CDK project<a name="ecs-web-server-cdk-step-1"></a>

Create a directory for your new AWS CDK app and initialize the project\.

------
#### [ TypeScript ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language typescript
```

------
#### [ JavaScript ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language javascript
```

------
#### [ Python ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language python
```

After the project is started, activate the project's virtual environment and install the AWS CDK's baseline dependencies\.

```
source .venv/bin/activate
python -m pip install -r requirements.txt
```

------
#### [ Java ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language java
```

Import this Maven project to your Java IDE\. For example, in Eclipse, use **File** > **Import** > **Maven** > **Existing Maven Projects**\.

------
#### [ C\# ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language csharp
```

------

**Note**  
The AWS CDK application template uses the name of the project directory to generate names for source files and classes\. In this example, the directory is named `hello-ecs`\. If you use a different project directory name, your app won't match these instructions\.

AWS CDK v2 includes stable constructs for all AWS services in a single package that's called `aws-cdk-lib`\. This package is installed as a dependency when you initialize the project\. When working with certain programming languages, the package is installed when you build the project for the first time\. This topic covers how to use an Amazon ECS Patterns construct, which provides high\-level abstractions for working with Amazon ECS\. This module relies on Amazon ECS constructs and other constructs to provision the resources that your Amazon ECS application needs\.

The names that you use to import these libraries into your CDK application might differ slightly depending on which programming language you use\. For reference, the following are the names that are used in each supported CDK programming language\.

------
#### [ TypeScript ]

```
aws-cdk-lib/aws-ecs
aws-cdk-lib/aws-ecs-patterns
```

------
#### [ JavaScript ]

```
aws-cdk-lib/aws-ecs
aws-cdk-lib/aws-ecs-patterns
```

------
#### [ Python ]

```
aws_cdk.aws_ecs
aws_cdk.aws_ecs_patterns
```

------
#### [ Java ]

```
software.amazon.awscdk.services.ecs
software.amazon.awscdk.services.ecs.patterns
```

------
#### [ C\# ]

```
Amazon.CDK.AWS.ECS
Amazon.CDK.AWS.ECS.Patterns
```

------

## Step 2: Use the AWS CDK to define a containerized web server on Fargate<a name="ecs-web-server-cdk-step-2"></a>

Use the container image [https://hub.docker.com/r/amazon/amazon-ecs-sample](https://hub.docker.com/r/amazon/amazon-ecs-sample) from DockerHub\. This image contains a PHP web app that runs on Amazon Linux 2\.

In the AWS CDK project that you created, edit the file that contains the stack definition to resemble one of the following examples\.

**Note**  
A stack is a unit of deployment\. All resources must be in a stack, and all the resources that are in a stack are deployed at the same time\. If a resource fails to deploy, any other resources that were already deployed are rolled back\. An AWS CDK app can contain multiple stacks, and resources in one stack can refer to resources in another stack\.

------
#### [ TypeScript ]

Update `lib/hello-ecs-stack.ts` so that it resembles the following\.

```
import * as cdk from 'aws-cdk-lib';
import { Construct } from 'constructs';
import * as ecs from 'aws-cdk-lib/aws-ecs';
import * as ecsp from 'aws-cdk-lib/aws-ecs-patterns';

export class HelloEcsStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    new ecsp.ApplicationLoadBalancedFargateService(this, 'MyWebServer', {
      taskImageOptions: {
        image: ecs.ContainerImage.fromRegistry('amazon/amazon-ecs-sample'),
      },
      publicLoadBalancer: true
    });
  }
}
```

------
#### [ JavaScript ]

Update `lib/hello-ecs-stack.js` so that it resembles the following\.

```
const cdk = require('aws-cdk-lib');
const { Construct } = require('constructs');
const ecs = require('aws-cdk-lib/aws-ecs');
const ecsp = require('aws-cdk-lib/aws-ecs-patterns');

class HelloEcsStack extends cdk.Stack {
  constructor(scope = Construct, id = string, props = cdk.StackProps) {
    super(scope, id, props);

    new ecsp.ApplicationLoadBalancedFargateService(this, 'MyWebServer', {
      taskImageOptions: {
        image: ecs.ContainerImage.fromRegistry('amazon/amazon-ecs-sample'),
      },
      publicLoadBalancer: true
    });
  }
}

module.exports = { HelloEcsStack }
```

------
#### [ Python ]

Update `hello-ecs/hello_ecs_stack.py` so that it resembles the following\.

```
import aws_cdk as cdk
from constructs import Construct

import aws_cdk.aws_ecs as ecs
import aws_cdk.aws_ecs_patterns as ecsp

class HelloEcsStack(cdk.Stack):

    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        ecsp.ApplicationLoadBalancedFargateService(self, "MyWebServer",
            task_image_options=ecsp.ApplicationLoadBalancedTaskImageOptions(
                image=ecs.ContainerImage.from_registry("amazon/amazon-ecs-sample")),
            public_load_balancer=True
        )
```

------
#### [ Java ]

Update `src/main/java/com.myorg/HelloEcsStack.java` so that it resembles the following\.

```
package com.myorg;

import software.constructs.Construct;
import software.amazon.awscdk.Stack;
import software.amazon.awscdk.StackProps;

import software.amazon.awscdk.services.ecs.ContainerImage;
import software.amazon.awscdk.services.ecs.patterns.ApplicationLoadBalancedFargateService;
import software.amazon.awscdk.services.ecs.patterns.ApplicationLoadBalancedTaskImageOptions;

public class HelloEcsStack extends Stack {
    public HelloEcsStack(final Construct scope, final String id) {
        this(scope, id, null);
    }

    public HelloEcsStack(final Construct scope, final String id, final StackProps props) {
        super(scope, id, props);

        ApplicationLoadBalancedFargateService.Builder.create(this, "MyWebServer")
        	.taskImageOptions(ApplicationLoadBalancedTaskImageOptions.builder()
        			.image(ContainerImage.fromRegistry("amazon/amazon-ecs-sample"))
        			.build())
        	.publicLoadBalancer(true)
        	.build();        
    }
}
```

------
#### [ C\# ]

Update `src/HelloEcs/HelloEcsStack.cs` so that it resembles the following\.

```
using Amazon.CDK;
using Constructs;
using Amazon.CDK.AWS.ECS;
using Amazon.CDK.AWS.ECS.Patterns;
namespace HelloEcs
{
    public class HelloEcsStack : Stack
    {
        internal HelloEcsStack(Construct scope, string id, IStackProps props = null) : base(scope, id, props)
        {
            new ApplicationLoadBalancedFargateService(this, "MyWebServer",
                new ApplicationLoadBalancedFargateServiceProps
                {
                    TaskImageOptions = new ApplicationLoadBalancedTaskImageOptions
                    {
                        Image = ContainerImage.FromRegistry("amazon/amazon-ecs-sample")
                    },
                    PublicLoadBalancer = true
                });
        }
    }
}
```

------

The preceding short snippet includes the following:
+ The service's logical name: `MyWebServer`\.
+ The container image that was obtained from DockerHub: `amazon/amazon-ecs-sample`\.
+ Other relevant information, such as the fact that the load balancer has a public address and is accessible from the Internet\.

 The AWS CDK will create all the resources that are required to deploy the web server including the following resources\. These resources were omitted in this example\.
+ Amazon ECS cluster 
+ Amazon VPC and Amazon EC2 instances 
+  Auto Scaling group
+  Application Load Balancer 
+  IAM roles and policies 

 Some automatically provisioned resources are shared by all Amazon ECS services defined in the stack\.

Save the source file, then run the `cdk synth` command in your application's main directory\. The AWS CDK runs the app and synthesizes an AWS CloudFormation template from it, and then displays the template\. The template is an approximately 600\-line YAML file\. The beginning of the file is shown here\. Your template might differ from this example\.

```
Resources:
  MyWebServerLB3B5FD3AB:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties:
      LoadBalancerAttributes:
        - Key: deletion_protection.enabled
          Value: "false"
      Scheme: internet-facing
      SecurityGroups:
        - Fn::GetAtt:
            - MyWebServerLBSecurityGroup01B285AA
            - GroupId
      Subnets:
        - Ref: EcsDefaultClusterMnL3mNNYNVpcPublicSubnet1Subnet3C273B99
        - Ref: EcsDefaultClusterMnL3mNNYNVpcPublicSubnet2Subnet95FF715A
      Type: application
    DependsOn:
      - EcsDefaultClusterMnL3mNNYNVpcPublicSubnet1DefaultRouteFF4E2178
      - EcsDefaultClusterMnL3mNNYNVpcPublicSubnet2DefaultRouteB1375520
    Metadata:
      aws:cdk:path: HelloEcsStack/MyWebServer/LB/Resource
  MyWebServerLBSecurityGroup01B285AA:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Automatically created Security Group for ELB HelloEcsStackMyWebServerLB06757F57
      SecurityGroupIngress:
        - CidrIp: 0.0.0.0/0
          Description: Allow from anyone on port 80
          FromPort: 80
          IpProtocol: tcp
          ToPort: 80
      VpcId:
        Ref: EcsDefaultClusterMnL3mNNYNVpc7788A521
    Metadata:
      aws:cdk:path: HelloEcsStack/MyWebServer/LB/SecurityGroup/Resource
# and so on for another few hundred lines
```

To deploy the service in your AWS account, run the `cdk deploy` command in your application's main directory\. You're asked to approve the IAM policies that the AWS CDK generated\.

The deployment takes several minutes during which the AWS CDK creates several resources\. The last few lines of the output from the deployment include the load balancer's public hostname and your new web server's URL\. They are as follows\.

```
Outputs:
HelloEcsStack.MyWebServerLoadBalancerDNSXXXXXXX = Hello-MyWeb-ZZZZZZZZZZZZZ-ZZZZZZZZZZ.us-west-2.elb.amazonaws.com
HelloEcsStack.MyWebServerServiceURLYYYYYYYY = http://Hello-MyWeb-ZZZZZZZZZZZZZ-ZZZZZZZZZZ.us-west-2.elb.amazonaws.com
```

## Step 3: Test the web server<a name="ecs-web-server-cdk-step-3"></a>

Copy the URL from the deployment output and paste it into your web browser\. The following welcome message from the web server is displayed\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/simple-php-app-congrats.png)

## Step 4: Clean up<a name="ecs-web-server-cdk-step-4"></a>

After you're finished with the web server, end the service using the CDK by running the `cdk destroy` command in your application's main directory\. Doing this prevents you from incurring any unintended charges in the future\.

## Next steps<a name="ecs-web-server-cdk-next-steps"></a>

To learn more about how to develop AWS infrastructure using the AWS CDK, see the [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/)\.

For information about writing AWS CDK apps in your language of choice, see the following:

------
#### [ TypeScript ]

[Working with the AWS CDK in TypeScript](https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-typescript.html)

------
#### [ JavaScript ]

[Working with the AWS CDK in JavaScript](https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-javascript.html)

------
#### [ Python ]

[Working with the AWS CDK in Python](https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-python.html)

------
#### [ Java ]

[Working with the AWS CDK in Java](https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-java.html)

------
#### [ C\# ]

[Working with the AWS CDK in C\#](https://docs.aws.amazon.com/cdk/v2/guide/work-with-cdk-csharp.html)

------

For more information about the AWS Construct Library modules used in this topic, see the following AWS CDK API Reference overviews\.
+ [https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs-readme.html](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs-readme.html)
+  [https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs_patterns-readme.html](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-cdk-lib.aws_ecs_patterns-readme.html)