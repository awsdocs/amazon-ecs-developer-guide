# Getting started with Amazon ECS using the AWS CDK<a name="tutorial-ecs-web-server-cdk"></a>

This tutorial shows you how to deploy a containerized Web server with Amazon Elastic Container Service and the AWS Cloud Development Kit \(CDK\) on Fargate\. The AWS CDK is an Infrastructure as Code \(IAC\) framework that lets you define AWS infrastructure using a full\-fledged programming language\. You write an app in one of the CDK's supported languages, containing one or more stacks, then synthesize it to an AWS CloudFormation template and deploy the resources to your AWS account\.

The AWS Construct Library, included with the CDK, provides APIs that model the resources provided by every AWS service\. For the most popular services, the library provides curated constructs that provide smart defaults and implement best practices with fewer required parameters\. One of these modules, `[aws\-ecs\-patterns](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-ecs-patterns-readme.html)`, provides high\-level abstractions that let you define your containerized service and all necessary supporting resources in only a few lines of code\.

 The construct we'll be using in this tutorial is [https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_aws-ecs-patterns.ApplicationLoadBalancedFargateService.html](https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_aws-ecs-patterns.ApplicationLoadBalancedFargateService.html)\. As you can likely tell from the name, this construct deploys an Amazon ECS service on Fargate behind an application load balancer\. The `aws-ecs-patterns` module also includes constructs that use a network load balancer and/or run on Amazon EC2, if you'd prefer those options\.

Before embarking on this tutorial, set up your AWS CDK development environment as described in [Getting Started With the AWS CDK \- Prerequisites](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html#getting_started_prerequisites), then install the AWS CDK by issuing:

```
npm install -g aws-cdk
```

**Topics**
+ [Step 1: Set up your AWS CDK project](#ecs-web-server-cdk-step-1)
+ [Step 2: Use the AWS CDK to define a containerized Web server on Fargate](#ecs-web-server-cdk-step-2)
+ [Step 3: Test the Web server](#ecs-web-server-cdk-step-3)
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

After the project has been initialized, activate the project's virtual environment and install the AWS CDK's baseline dependencies\.

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

Import this Maven project to your Java IDE \(for example, in Eclipse, use **File** > **Import** > **Maven** > **Existing Maven Projects**\)\.

------
#### [ C\# ]

```
mkdir hello-ecs
cd hello-ecs
cdk init --language csharp
```

------

**Note**  
Be sure to name the directory `hello-ecs` as shown\. The AWS CDK application template uses the name of the project directory to generate names for source files and classes\. If you use a different name, your app will not match this tutorial\.

Now install the `aws-ecs-patterns` construct library module\.

------
#### [ TypeScript ]

```
npm install @aws-cdk/aws-ecs-patterns
```

------
#### [ JavaScript ]

```
npm install @aws-cdk/aws-ecs-patterns
```

------
#### [ Python ]

```
python -m pip install aws-cdk.aws-ecs-patterns
```

------
#### [ Java ]

Edit the project's `pom.xml` to add the following dependencies inside the existing `<dependencies>` container\.

```
        <dependency>
            <groupId>software.amazon.awscdk</groupId>
            <artifactId>ecs</artifactId>
            <version>${cdk.version}</version>
        </dependency>
        <dependency>
            <groupId>software.amazon.awscdk</groupId>
            <artifactId>ecs-patterns</artifactId>
            <version>${cdk.version}</version>
        </dependency>
```

Maven automatically installs these dependencies the next time you build your app\. To build, issue `mvn compile` or use your Java IDE's **Build** command\.

------
#### [ C\# ]

```
dotnet add src\HelloEcs package Amazon.CDK.AWS.ECS.Patterns
```

You may also install the indicated packages using the Visual Studio NuGet GUI, available via **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**\.

------

When you install the `aws-ecs-patterns` module, the `aws-ecs` module is also installed because it is a dependency of `aws-ecs-patterns`\. You can use these modules in your AWS CDK app by importing the corresponding package\. 

------
#### [ TypeScript ]

```
@aws-cdk/aws-ecs
@aws-cdk/aws-ecs-patterns
```

------
#### [ JavaScript ]

```
@aws-cdk/aws-ecs
@aws-cdk/aws-ecs-patterns
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

## Step 2: Use the AWS CDK to define a containerized Web server on Fargate<a name="ecs-web-server-cdk-step-2"></a>

We'll use the container image [https://hub.docker.com/r/amazon/amazon-ecs-sample](https://hub.docker.com/r/amazon/amazon-ecs-sample) from DockerHub\. This image contains a PHP Web app running under Amazon Linux 2\.

In the AWS CDK project you created, edit the file containing the definition of the stack to look like the code below\. You'll recognize the instantiation of the `ApplicationLoadBalancedFargateService` construct—or at least its name\.

**Note**  
What's a stack? The stack is the unit of deployment: all resources must be in a stack, and all the resources in a stack are deployed together\. If a resource fails to deploy, any other resources already deployed are rolled back\. An AWS CDK app can contain multiple stacks, and resources in one stack can refer to resources in in another\.

------
#### [ TypeScript ]

Update `lib/hello-ecs-stack.ts` to read as follows\.

```
import * as cdk from '@aws-cdk/core';

import * as ecs from '@aws-cdk/aws-ecs';
import * as ecsp from '@aws-cdk/aws-ecs-patterns';

export class HelloEcsStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
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

Update `lib/hello-ecs-stack.js` to read as follows\.

```
const cdk = require('@aws-cdk/core');

const ecs = require('@aws-cdk/aws-ecs');
const ecsp = require('@aws-cdk/aws-ecs-patterns');

class HelloEcsStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
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

Update `hello-ecs/hello_ecs_stack.py` to read as follows\.

```
from aws_cdk import core as cdk

import aws_cdk.aws_ecs as ecs
import aws_cdk.aws_ecs_patterns as ecsp

class HelloEcsStack(cdk.Stack):

    def __init__(self, scope: cdk.Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        ecsp.ApplicationLoadBalancedFargateService(self, "MyWebServer",
            task_image_options=ecsp.ApplicationLoadBalancedTaskImageOptions(
                image=ecs.ContainerImage.from_registry("amazon/amazon-ecs-sample")),
            public_load_balancer=True
        )
```

------
#### [ Java ]

Update `src/main/java/com.myorg/HelloEcsStack.java` to read as follows\.

```
package com.myorg;

import software.amazon.awscdk.core.Construct;
import software.amazon.awscdk.core.Stack;
import software.amazon.awscdk.core.StackProps;

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

Update `src/HelloEcs/HelloEcsStack.cs` to read as follows\.

```
using Amazon.CDK;
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

You can see in this short snippet:
+ The service's logical name, `MyWebServer`\.
+ The container image, obtained from DockerHub, `amazon/amazon-ecs-sample`\.\.
+ The fact that the load balancer will have a public address and will thus be accessible from the Internet\.

If you omit, as we have done here, the Amazon ECS cluster, the underlying Amazon Virtual Private Cloud and Amazon EC2 instances, an Auto Scaling Group, the Application Load Balancer, the necessary IAM roles and policies, and other AWS resources required to deploy the Web server, the AWS CDK will also create these resources\. Some automatically\-provisioned resources will be shared by all Amazon ECS services defined in the stack\.

Save the source file, then issue `cdk synth` in the app's main directory\. The AWS CDK runs the app and synthesizes an AWS CloudFormation template from it, then displays the template\. The template is about 600 lines of YAML, so only the beginning is shown here\. \(Your template may have differences from ours\.\)

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

To actually deploy the service in your AWS account, issue `cdk deploy`\. You'll be asked to approve the IAM policies the AWS CDK has generated\.

Deployment will take several minutes\. You'll see the AWS CDK create quite a number of resources\. The last few lines of the output from the deployment include the public hostname of the load balancer and an HTTP URL for your new Web server\.

```
Outputs:
HelloEcsStack.MyWebServerLoadBalancerDNSXXXXXXX = Hello-MyWeb-ZZZZZZZZZZZZZ-ZZZZZZZZZZ.us-west-2.elb.amazonaws.com
HelloEcsStack.MyWebServerServiceURLYYYYYYYY = http://Hello-MyWeb-ZZZZZZZZZZZZZ-ZZZZZZZZZZ.us-west-2.elb.amazonaws.com
```

## Step 3: Test the Web server<a name="ecs-web-server-cdk-step-3"></a>

Copy the URL from the deployment output and paste it into your Web browser\. You should see a welcome message from the Web server\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/simple-php-app-congrats.png)

## Step 4: Clean up<a name="ecs-web-server-cdk-step-4"></a>

Now that you're done with the Web server \(it doesn't do anything besides display the Congratulations message\), you can tear down the service using the AWS CDK\. Issue `cdk destroy` in your app's main directory\. Doing this will prevent unintended AWS charges\.

## Next steps<a name="ecs-web-server-cdk-next-steps"></a>

To learn more about developing AWS infrastructure using the AWS CDK, see the [AWS CDK Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

For information about writing AWS CDK apps in your language of choice, see:

------
#### [ TypeScript ]

[Working with the AWS CDK in TypeScript](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-typescript.html)

------
#### [ JavaScript ]

[Working with the AWS CDK in JavaScript](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-javascript.html)

------
#### [ Python ]

[Working with the AWS CDK in Python](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-python.html)

------
#### [ Java ]

[Working with the AWS CDK in Java](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-java.html)

------
#### [ C\# ]

[Working with the AWS CDK in C\#](https://docs.aws.amazon.com/cdk/latest/guide/work-with-cdk-csharp.html)

------

For more information on the AWS Construct Library modules used in this tutorial, see the AWS CDK API Reference overviews below\.
+ [aws\-ecs](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-ecs-readme.html)
+ [aws\-ecs\-patterns](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-ecs-patterns-readme.html)