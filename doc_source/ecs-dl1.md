# Using deep learning DL1 instances on Amazon ECS<a name="ecs-dl1"></a>

To use deep learning workloads on Amazon ECS, register [Amazon EC2 DL1](http://aws.amazon.com/ec2/instance-types/dl1/) instances to your clusters\. Amazon EC2 DL1 instances are powered by Gaudi accelerators from Habana Labs \(an Intel company\)\. Use the Habana SynapseAI SDK to connect to the Habana Gaudi accelerators\. The SDK supports the popular machine learning frameworks, TensorFlow and PyTorch\.

## Considerations<a name="ecs-dl1-considerations"></a>

Before you begin deploying DL1 on Amazon ECS, consider the following:
+ Your clusters can contain a mix of DL1 and non\-DL1 instances\.
+ When creating a service or running a standalone task, you can use instance type attributes specifically when you configure task placement constraints to ensure that your task is launched on the container instance that you specify\. Doing so ensures that your resources are used effectively and that your tasks for deep learning workloads are on your DL1 instances\. For more information, see [Amazon ECS task placement](task-placement.md)\.

  The following example runs a task on a `dl1.24xlarge` instance on your `default` cluster\.

  ```
  aws ecs run-task \
       --cluster default \
       --task-definition ecs-dl1-task-def \
       --placement-constraints type=memberOf,expression="attribute:ecs.instance-type == dl1.24xlarge"
  ```

## Using a DL1 AMI<a name="ecs-dl1-ami"></a>

You have three options for running an AMI on Amazon EC2 DL1 instances for Amazon ECS:
+ AWS Marketplace AMIs that are provided by Habana [here](https://aws.amazon.com/marketplace/pp/prodview-h24gzbgqu75zq)\.
+ Habana Deep Learning AMIs that are provided by Amazon Web Services\. Because it's not included, you need to install the Amazon ECS container agent separately\.
+ Use Packer to build a custom AMI that's provided by the [GitHub repo](https://github.com/aws-samples/aws-habana-baseami-pipeline)\. For more information, see [the Packer documentation](https://www.packer.io/docs)\.

## Task definition requirements<a name="ecs-dl1-requirements"></a>

To run Habana Gaudi accelerated deep learning containers on Amazon ECS, your task definition must contain the container definition for a pre\-built container that serves the deep learning model for TensorFlow or PyTorch using Habana SynapseAI that's provided by AWS Deep Learning Containers\.

The following container image has TensorFlow 2\.7\.0 and Ubuntu 20\.04\. A complete list of pre\-built Deep Learning Containers that's optimized for the Habana Gaudi accelerators is maintained on GitHub\. For more information, see [Habana Training Containers](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#habana-training-containers)\.

```
763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training-habana:2.7.0-hpu-py38-synapseai1.2.0-ubuntu20.04
```

The following is an example task definition for Linux containers on Amazon EC2, displaying the syntax to use\. This example uses an image containing the Habana Labs System Management Interface Tool \(HL\-SMI\) found here: `vault.habana.ai/gaudi-docker/1.1.0/ubuntu20.04/habanalabs/tensorflow-installer-tf-cpu-2.6.0:1.1.0-614`

```
{
  "family": "dl-test",
  "requiresCompatibilities": ["EC2"],
  "placementConstraints": [
        {
            "type": "memberOf",
            "expression": "attribute:ecs.os-type == linux"
        },
        {
            "type": "memberOf",
            "expression": "attribute:ecs.instance-type == dl1.24xlarge"
        }
    ],
  "networkMode": "host",
  "cpu": "10240",
  "memory": "1024",
  "containerDefinitions": [
    {
      "entryPoint": [
        "sh",
        "-c"
      ],
      "command": [
        "hl-smi"
      ],
      "cpu": 8192,
      "environment": [
        {
          "name": "HABANA_VISIBLE_DEVICES",
          "value": "all"
        }
      ],
      "image": 
"vault.habana.ai/gaudi-docker/1.1.0/ubuntu20.04/habanalabs/tensorflow-installer-tf-cpu-2.6.0:1.1.0-614",

      "essential": true,
      "name": "tensorflow-installer-tf-hpu"
    }
  ]
}
```