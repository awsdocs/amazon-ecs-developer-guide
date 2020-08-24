# Working with inference workloads on Amazon ECS<a name="ecs-inference"></a>

Amazon ECS supports machine learning inference workloads by enabling you to register [Amazon EC2 Inf1](http://aws.amazon.com/ec2/instance-types/inf1/) instances to your clusters\. Amazon EC2 Inf1 instances are powered by [AWS Inferentia](http://aws.amazon.com/machine-learning/inferentia/) chips, which are custom built by AWS to provide high performance and lowest cost inference in the cloud\. Machine learning models are deployed to containers using [AWS Neuron](http://aws.amazon.com/machine-learning/neuron/), a specialized software development kit \(SDK\) consisting of a compiler, run\-time, and profiling tools that optimize the machine learning inference performance of Inferentia chips\. AWS Neuron supports popular machine learning frameworks such as TensorFlow, PyTorch, and MXNet\.

## Considerations<a name="ecs-inference-considerations"></a>

Before you begin working with Inferentia on Amazon ECS, be aware of the following considerations:
+ Your clusters can contain a mix of Inf1 and non\-Inf1 container instances\.
+ When running a task or creating a service, you can use instance type attributes when configuring task placement constraints to ensure which of your container instances the task is launched on\. This will enable you to effectively use your resources while ensuring your tasks for inference workloads land on your Inf1 instances\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  The following example launches a task on a `Inf1.xlarge` container instance in your `default` cluster\.

  ```
  aws ecs run-task \
       --cluster default \
       --task-definition ecs-inference-task-def \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == Inf1.xlarge" \
       --region us-west-2
  ```
+ Currently, you cannot define the Inferentia resource requirement in a task definition\. However, you can configure a container to use specific Inferentia available on the container instance by using the `AWS_NEURON_VISIBLE_DEVICES` environment variable\.
+ It is recommended that you place only one task with an Inferentia resource requirement per Inf1 instance\.

## Using the Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI<a name="ecs-inference-ami"></a>

Amazon ECS provides an Amazon ECS\-optimized AMI based on Amazon Linux 2 for Inferentia workloads that comes comes pre\-configured with AWS Inferentia drivers and the AWS Neuron runtime for Docker\. This AMI makes running machine learning inference workloads easier on Amazon ECS\.

The Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI is recommended for use when launching your Amazon EC2 Inf1 instances\. You can retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI using the AWS CLI with the following command:

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended
```

The following table provides a link to retrieve the current Amazon ECS\-optimized Amazon Linux 2 \(Inferentia\) AMI IDs by Region\.


|  Region Name  |  Region  |  AMI ID  | 
| --- | --- | --- | 
|  US East \(N\. Virginia\)  | us\-east\-1 |  [View AMI ID](https://us-east-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-1#)  | 
|  US East \(Ohio\)  | us\-east\-2 |  [View AMI ID](https://us-east-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-east-2#)  | 
|  US West \(Oregon\)  | us\-west\-2 |  [View AMI ID](https://us-west-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=us-west-2#)  | 
|  Asia Pacific \(Sydney\)  | ap\-southeast\-2 |  [View AMI ID](https://ap-southeast-2.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-southeast-2#)  | 
|  Asia Pacific \(Tokyo\)  | ap\-northeast\-1 |  [View AMI ID](https://ap-northeast-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=ap-northeast-1#)  | 
|  Europe \(Frankfurt\)  | eu\-central\-1 |  [View AMI ID](https://eu-central-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-central-1#)  | 
|  Europe \(Ireland\)  | eu\-west\-1 |  [View AMI ID](https://eu-west-1.console.aws.amazon.com/systems-manager/parameters/aws/service/ecs/optimized-ami/amazon-linux-2/inf/recommended/image_id/description?region=eu-west-1#)  | 

## Task definition requirements<a name="ecs-inference-requirements"></a>

The following are the requirements for creating a task definition for working with Inferentia on Amazon ECS\.
+ The task definition must contain the following container definitions:
  + A `neuron-rtd` sidecar container\. You can use the public `neuron-rtd` container image by specifying either `neuron-rtd:latest` or a specific `neuron-rtd` version, for example `neuron-rtd:1.0.8444.0`\. The full image name including the repository URI is `790709498068.dkr.ecr.us-west-2.amazonaws.com/neuron-rtd:latest`\.

    Alternatively, you can build your own Neuron sidecar container image\. For more information, see [aws\-neuron\-sdk](https://github.com/aws/aws-neuron-sdk/blob/master/docs/neuron-container-tools/docker-example/Dockerfile.neuron-rtd) on GitHub\. 
  + A container serving the inference model\. For an example, see [aws\-neuron\-sdk](https://github.com/aws/aws-neuron-sdk/blob/master/docs/tensorflow-neuron/tutorial-tensorflow-serving.md) on GitHub\.
+ The `neuron-rtd` sidecar container must start first\. This can be defined using container dependency parameters\.
+ The `neuron-rtd` sidecar container must have elevated privileges by adding the `SYS_ADMIN` and `IPC_LOCK` kernel capabilities\. This is done by using the `linuxParameters` container definition parameter\. These capabilities will be dropped following initialization\.
+ The two containers must have a shared volume\.
+ Currently, you cannot define the Inferentia resource requirement in a task definition\. However, you can configure a container to use specific Inferentia available on the container instance through the `AWS_NEURON_VISIBLE_DEVICES` environment variable\. The AWS Neuron runtime expects the `AWS_NEURON_VISIBLE_DEVICES` environment variable to be set in the container in order for it to work\. We recommend your container use all available Inferentia devices by specifying `AWS_NEURON_VISIBLE_DEVICES="ALL"`\. Alternatively, to use the first two inferentia devices you would specify `AWS_NEURON_VISIBLE_DEVICES="0,1"`\. The specified devices must always be contiguous\.

The following is an example task definition, displaying the syntax to use\.

```
{
    "family": "ecs-neuron",
    "executionRoleArn": "${YOUR_EXECUTION_ROLE}",
    "containerDefinitions": [
        {
            "entryPoint": [
                "sh",
                "-c"
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
            "command": [
                "tensorflow_model_server_neuron --port=8500 --rest_api_port=8501 --model_name=bert --model_base_path=/bert"
            ],
            "cpu": 0,
            "dependsOn": [
               {
                    "containerName": "neuron-rtd",
                    "condition": "START"
                }
            ],
            "environment": [
                {
                    "name": "NEURON_RTD_ADDRESS",
                    "value": "unix:/sock/neuron-rtd.sock"
                }
            ],
            "mountPoints": [
                {
                    "containerPath": "/sock",
                    "sourceVolume": "sock"
                }
            ],
            "memoryReservation": 1000,
            "image": "${YOUR_IMAGE}",
            "essential": true,
            "name": "bert"
        },
        {
            "entryPoint": [
                "sh",
                "-c"
            ],
            "portMappings": [],
            "command": [
                "neuron-rtd -g unix:/sock/neuron-rtd.sock"
            ],
            "cpu": 0,
            "environment": [
                {
                    "name": "AWS_NEURON_VISIBLE_DEVICES",
                    "value": "ALL"
                }
            ],
            "mountPoints": [
                {
                    "containerPath": "/sock",
                    "sourceVolume": "sock"
                }
            ],
            "memoryReservation": 1000,
            "image": "790709498068.dkr.ecr.us-east-1.amazonaws.com/neuron-rtd:latest",
            "essential": true,
            "linuxParameters": {
                "capabilities": {
                    "add": [
                        "SYS_ADMIN",
                        "IPC_LOCK"
                    ]
                }
            },
            "name": "neuron-rtd"
        }
    ],
    "volumes": [
        {
            "name": "sock",
            "host": {
                "sourcePath": "/tmp/sock"
            }
        }
    ]
}
```