# Working with inference workloads on Amazon ECS<a name="ecs-inference"></a>

Amazon ECS supports machine learning inference workloads by enabling you to register [Amazon EC2 Inf1](http://aws.amazon.com/ec2/instance-types/inf1/) instances to your clusters\. Amazon EC2 Inf1 instances are powered by [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/) chips, which are custom built by AWS to provide high performance and lowest cost inference in the cloud\. Machine learning models are deployed to containers using [AWS Neuron](http://aws.amazon.com/machine-learning/neuron/), a specialized software development kit \(SDK\) consisting of a compiler, runtime, and profiling tools that optimize the machine learning inference performance of Inferentia chips\. AWS Neuron supports popular machine learning frameworks such as TensorFlow, PyTorch, and Apache MXNet \(Incubating\)\.

## Considerations<a name="ecs-inference-considerations"></a>

Before you begin deploying Neuron on Amazon ECS, be aware of the following considerations:
+ Your clusters can contain a mix of Inf1 and non\-Inf1 instances\.
+ We recommend that you place only one task with an Inferentia resource requirement per Inf1 instance\.
+ When creating a service or running a standalone task, you can use instance type attributes when configuring task placement constraints to ensure which of your container instances the task is launched on\. By doing this, you can more effectively use your resources while also ensuring your tasks for inference workloads land on your Inf1 instances\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  The following example runs a task on a `Inf1.xlarge` instance on your `default` cluster\.

  ```
  aws ecs run-task \
       --cluster default \
       --task-definition ecs-inference-task-def \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == Inf1.xlarge"
  ```
+ Inferentia resource requirements can't be defined in a task definition\. However, you can configure a container to use specific Inferentia available on the host container instance by using the `linuxParameters` parameter and specifying the device details\. For more information, see [Task definition requirements](#ecs-inference-requirements)\.

## Using the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI<a name="ecs-inference-ami"></a>

Amazon ECS provides an Amazon ECS\-optimized AMI based on Amazon Linux 2 for Inferentia workloads that comes pre\-configured with AWS Inferentia drivers and the AWS Neuron runtime for Docker\. This AMI makes running machine learning inference workloads easier on Amazon ECS\.

We recommend using the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI when launching your Amazon EC2 Inf1 instances\. You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI IDs by Region\.


|  Region name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  |  `us-east-1`  |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  |  `us-east-2`  |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-2#)  | 
|  US West \(Oregon\)  |  `us-west-2`  |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Mumbai\)  |  `ap-south-1`  |  [View AMI ID](https://ap-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-south-1#)  | 
|  Asia Pacific \(Hong Kong\)  |  `ap-east-1`  |  [View AMI ID](https://ap-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-east-1#)  | 
|  Asia Pacific \(Tokyo\)  |  `ap-northeast-1`  |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Asia Pacific \(Seoul\)  |  `ap-northeast-2`  |  [View AMI ID](https://ap-northeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-2#)  | 
|  Asia Pacific \(Singapore\)  |  `ap-southeast-1`  |  [View AMI ID](https://ap-southeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-1#)  | 
|  Asia Pacific \(Sydney\)  |  `ap-southeast-2`  |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Canada \(Central\)  |  `ca-central-1`  |  [View AMI ID](https://ca-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ca-central-1#)  | 
|  Europe \(Frankfurt\)  |  `eu-central-1`  |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Milan\)  |  `eu-south-1`  |  [View AMI ID](https://eu-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-south-1#)  | 
|  Europe \(Ireland\)  |  `eu-west-1`  |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-1#)  | 
|  Europe \(Paris\)  |  `eu-west-3`  |  [View AMI ID](https://eu-west-3.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-3#)  | 
|  Middle East \(Bahrain\)  |  `me-south-1`  |  [View AMI ID](https://me-south-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=me-south-1#)  | 
|  South America \(São Paulo\)  |  `sa-east-1`  |  [View AMI ID](https://sa-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=sa-east-1#)  | 
|  AWS GovCloud \(US\-East\)  | `us-gov-east-1` |  [View AMI ID](https://us-gov-east-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-gov-east-1#)  | 
|  AWS GovCloud \(US\-West\)  | `us-gov-west-1` |  [View AMI ID](https://us-gov-west-1.console.amazonaws-us-gov.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-gov-west-1#)  | 
|  China \(Beijing\)  |  `cn-north-1`  |  [View AMI ID](https://cn-north-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=cn-north-1#)  | 
|  China \(Ningxia\)  |  `cn-northwest-1`  |  [View AMI ID](https://cn-northwest-1.console.amazonaws.cn/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=cn-northwest-1#)  | 

## Task definition requirements<a name="ecs-inference-requirements"></a>

To deploy Neuron on Amazon ECS, your task definition must contain the container definition for a pre\-built container serving the inference model for TensorFlow provided by AWS Deep Learning Containers\. This container contains the AWS Neuron runtime and the TensorFlow Serving application\. At start up, this container will fetch your model from Amazon S3, launch Neuron TensorFlow Serving with the saved model, and wait for prediction requests\. The following container image has TensorFlow 1\.15 and Ubuntu 18\.04\. A complete list of pre\-built Deep Learning Containers optimized for Neuron is maintained on GitHub\. For more information, see [Neuron Inference Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#neuron-inference-containers)\.

```
763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference-neuron:1.15.4-neuron-py37-ubuntu18.04
```

Alternatively, you can build your own Neuron sidecar container image\. For more information, see [Tutorial: Neuron TensorFlow Serving](https://github.com/aws/aws-neuron-sdk/blob/master/neuron-guide/neuron-frameworks/tensorflow-neuron/tutorials/tensorflow-tutorial-setup.rst) on GitHub\.

Currently, the Inferentia resource requirement can't be defined in a task definition\. However, you can configure a container to use specific Inferentia available on the host container instance using the `linuxParameters` parameter\. The following is an example Linux containers on Fargate task definition, displaying the syntax to use\.

```
{
    "family": "ecs-neuron",
    "executionRoleArn": "${YOUR_EXECUTION_ROLE}",
    "containerDefinitions": [
        {
            "entryPoint": [
                "/usr/local/bin/entrypoint.sh",
                "--port=8500",
                "--rest_api_port=9000",
                "--model_name=resnet50_neuron",
                "--model_base_path=s3://your-bucket-of-models/resnet50_neuron/"
            ],
            "portMappings": [
                {
                    "hostPort": 8500,
                    "protocol": "tcp",
                    "containerPort": 8500
                },
                {
                    "hostPort": 8501,
                    "protocol": "tcp",
                    "containerPort": 8501
                },
                {
                    "hostPort": 0,
                    "protocol": "tcp",
                    "containerPort": 80
                }
            ],
            "linuxParameters": {
                "devices": [
                    {
                        "containerPath": "/dev/neuron0",
                        "hostPath": "/dev/neuron0",
                        "permissions": [
                            "read",
                            "write"
                        ]
                    }
                ],
                "capabilities": {
                    "add": [
                        "IPC_LOCK"
                    ]
                }
            },
            "cpu": 0,
            "memoryReservation": 1000,
            "image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference-neuron:1.15.4-neuron-py37-ubuntu18.04",
            "essential": true,
            "name": "resnet50"
        }
    ]
}
```