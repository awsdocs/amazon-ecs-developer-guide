# AWS Deep Learning Containers on Amazon ECS<a name="deep-learning-containers"></a>

AWS Deep Learning Containers provide a set of Docker images for training and serving models in TensorFlow and Apache MXNet on Amazon ECS\. Deep Learning Containers enable optimized environments with TensorFlow, NVIDIA CUDA \(for GPU instances\), and Intel MKL \(for CPU instances\) libraries\. Container images for Deep Learning Containers are available in Amazon ECR to reference in Amazon ECS task definitions\. You can use Deep Learning Containers along with Amazon Elastic Inference to lower your inference costs\.

To get started using Deep Learning Containers without Elastic Inference on Amazon ECS, see [Deep Learning Containers on Amazon ECS](https://docs.aws.amazon.com/dlami/latest/devguide/deep-learning-containers-ecs.html) in the *AWS Deep Learning AMI Developer Guide*\.

## Deep Learning Containers with Elastic Inference on Amazon ECS<a name="dlc-elastic-inference"></a>

AWS Deep Learning Containers provide a set of Docker images for serving models in TensorFlow and Apache MXNet that take advantage of Amazon Elastic Inference accelerators\. Amazon ECS provides task definition parameters to attach Elastic Inference accelerators to your containers\. When you specify an Elastic Inference accelerator type in your task definition, Amazon ECS manages the lifecycle of, and configuration for, the accelerator\. The Amazon ECS service\-linked role is required when using this feature\. For more information about Elastic Inference accelerators, see [Amazon Elastic Inference Basics](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/basics.html)\.

**Important**  
Your Amazon ECS container instances require at least version `1.30.0` of the container agent\. For information about checking your agent version and updating to the latest version, see [Updating the Amazon ECS Container Agent](ecs-agent-update.md)\.

To get started using Deep Learning Containers with Elastic Inference on Amazon ECS, see [Deep Learning Containers with Elastic Inference on Amazon ECS](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/ei-dlc-ecs.html) in the *Amazon Elastic Inference Developer Guide*\.