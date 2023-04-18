# Using AWS Neuron on Amazon Linux 2 on Amazon ECS<a name="ecs-inference"></a>

You can register [Amazon EC2 Trn1](http://aws.amazon.com/ec2/instance-types/trn1/), [Amazon EC2 Inf1](http://aws.amazon.com/ec2/instance-types/inf1/), and [Amazon EC2 Inf2](http://aws.amazon.com/ec2/instance-types/inf2/) instances to your clusters for machine learning workloads\.

Amazon EC2 Trn1 instances are powered by [AWS Trainium](http://aws.amazon.com/machine-learning/trainium/) chips\. These instances provide high performance and low cost training for machine learning in the cloud\. You can train a machine learning inference model using a machine learning framework with AWS Neuron on a Trn1 instance\. Then, you can run the model on a Inf1 instance, or an Inf2 instance to use the acceleration of the AWS Inferentia chips\.

The Amazon EC2 Inf1 instances and Inf2 instances are powered by [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/) chips They provide high performance and lowest cost inference in the cloud\.

Machine learning models are deployed to containers using [AWS Neuron](http://aws.amazon.com/machine-learning/neuron/), which is a specialized Software Developer Kit \(SDK\)\. The SDK consists of a compiler, runtime, and profiling tools that optimize the machine learning performance of AWS machine learning chips\. AWS Neuron supports popular machine learning frameworks such as TensorFlow, PyTorch, and Apache MXNet\.

## Considerations<a name="ecs-inference-considerations"></a>

Before you begin deploying Neuron on Amazon ECS, consider the following:
+ Your clusters can contain a mix of Trn1, Inf1, Inf2 and other instances\.
+ You need a Linux application in a container that uses a machine learning framework that supports AWS Neuron\.
**Important**  
Applications that use other frameworks might not have improved performance on Trn1, Inf1, and Inf2 instances\.
+ Only one inference or inference\-training task can run on each [AWS Trainium](http://aws.amazon.com/machine-learning/trainium/) or [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/) chip\. For Inf1, each chip has 4 NeuronCores\. For Trn1 and Inf2 each chip has 2 NeuronCores\. You can run as many tasks as there are chips for each of your Trn1, Inf1, and Inf2 instances\.
+ When creating a service or running a standalone task, you can use instance type attributes when you configure task placement constraints\. This ensures that the task is launched on the container instance that you specify\. Doing so can help you optimize overall resource utilization and ensure that tasks for inference workloads are on your Trn1, Inf1, and Inf2 instances\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  In the following example, a task is run on an `Inf1.xlarge` instance on your `default` cluster\.

  ```
  aws ecs run-task \
       --cluster default \
       --task-definition ecs-inference-task-def \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == Inf1.xlarge"
  ```
+ Neuron resource requirements can't be defined in a task definition\. Instead, you configure a container to use specific AWS Trainium or AWS Inferentia chips available on the host container instance\. Do this by using the `linuxParameters` parameter and specifying the device details\. For more information, see [Task definition requirements](#ecs-inference-requirements)\.

## Using the Amazon ECS optimized Amazon Linux 2 \(Neuron\) AMI<a name="ecs-inference-ami"></a>

Amazon ECS provides an Amazon ECS optimized AMI that's based on Amazon Linux 2 for AWS Trainium and AWS Inferentia workloads\. It comes with the AWS Neuron drivers and runtime for Docker\. This AMI makes running machine learning inference workloads easier on Amazon ECS\.

We recommend using the Amazon ECS optimized Amazon Linux 2 \(Neuron\) AMI when launching your Amazon EC2 Trn1, Inf1, and Inf2 instances\. 

You can retrieve the current Amazon ECS optimized Amazon Linux 2 \(Neuron\) AMI using the AWS CLI with the following command\.

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

## Task definition requirements<a name="ecs-inference-requirements"></a>

To deploy Neuron on Amazon ECS, your task definition must contain the container definition for a pre\-built container serving the inference model for TensorFlow\. It's provided by AWS Deep Learning Containers\. This container contains the AWS Neuron runtime and the TensorFlow Serving application\. At startup, this container fetches your model from Amazon S3, launches Neuron TensorFlow Serving with the saved model, and waits for prediction requests\. In the following example, the container image has TensorFlow 1\.15 and Ubuntu 18\.04\. A complete list of pre\-built Deep Learning Containers optimized for Neuron is maintained on GitHub\. For more information, see [Using AWS Neuron TensorFlow Serving](https://docs.aws.amazon.com/dlami/latest/devguide/tutorial-inferentia-tf-neuron-serving.html)\.

```
763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference-neuron:1.15.4-neuron-py37-ubuntu18.04
```

Alternatively, you can build your own Neuron sidecar container image\. For more information, see [Tutorial: Neuron TensorFlow Serving](https://github.com/aws-neuron/aws-neuron-sdk/blob/master/frameworks/tensorflow/tensorflow-neuron/tutorials/tutorials-tensorflow-utilizing-neuron-capabilities.rst) in the *AWS Deep Learning AMI Developer Guide*\.

The task definition must be specific to a single instance type\. You must configure a container to use specific AWS Trainium or AWS Inferentia devices that are available on the host container instance\. You can do so using the `linuxParameters` parameter\. The following table details the chips that are specific to each instance type\.


| Instance Type | vCPUs | RAM \(GiB\) | AWS ML accelerator chips | Device Paths | 
| --- | --- | --- | --- | --- | 
| trn1\.2xlarge | 8 | 32 | 1 | /dev/neuron0 | 
| trn1\.32xlarge | 128 | 512 | 16 |  /dev/neuron0, /dev/neuron1, /dev/neuron2, /dev/neuron3, /dev/neuron4, /dev/neuron5, /dev/neuron6, /dev/neuron7, /dev/neuron8, /dev/neuron9, /dev/neuron10, /dev/neuron11, /dev/neuron12, /dev/neuron13, /dev/neuron14, /dev/neuron15  | 
| inf1\.xlarge | 4 | 8 | 1 | /dev/neuron0 | 
| inf1\.2xlarge | 8 | 16 | 1 | /dev/neuron0 | 
| inf1\.6xlarge | 24 | 48 | 4 | /dev/neuron0, /dev/neuron1, /dev/neuron2, /dev/neuron3 | 
| inf1\.24xlarge | 96 | 192 | 16 |  /dev/neuron0, /dev/neuron1, /dev/neuron2, /dev/neuron3, /dev/neuron4, /dev/neuron5, /dev/neuron6, /dev/neuron7, /dev/neuron8, /dev/neuron9, /dev/neuron10, /dev/neuron11, /dev/neuron12, /dev/neuron13, /dev/neuron14, /dev/neuron15  | 
| inf2\.xlarge | 8 | 16 | 1 | /dev/neuron0 | 
| inf2\.8xlarge | 32 | 64 | 1 | /dev/neuron0 | 
| inf2\.24xlarge | 96 | 384 | 6 | /dev/neuron0, /dev/neuron1, /dev/neuron2, /dev/neuron3, /dev/neuron4, /dev/neuron5,  | 
| inf2\.48xlarge | 192 | 768 | 12 | /dev/neuron0, /dev/neuron1, /dev/neuron2, /dev/neuron3, /dev/neuron4, /dev/neuron5, /dev/neuron6, /dev/neuron7, /dev/neuron8, /dev/neuron9, /dev/neuron10, /dev/neuron11 | 

The following is an example Linux task definition for `inf1.xlarge`, displaying the syntax to use\.

```
{
    "family": "ecs-neuron",
    "requiresCompatibilities": ["EC2"],
    "placementConstraints": [
        {
            "type": "memberOf",
            "expression": "attribute:ecs.os-type == linux"
        },
        {
            "type": "memberOf",
            "expression": "attribute:ecs.instance-type == inf1.xlarge"
        }
    ],
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