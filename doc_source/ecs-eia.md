# Working with Amazon Elastic Inference on Amazon ECS<a name="ecs-eia"></a>

Amazon ECS supports attaching Amazon Elastic Inference accelerators to your containers to make running deep learning inference workloads more efficient\. When specifying an Elastic Inference accelerator type in your task definition, Amazon ECS will manage the lifecycle of and configuration for the accelerator\. For more information about Elastic Inference accelerators, see [Amazon Elastic Inference Basics](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/basics.html)\.

## Considerations for Working with Elastic Inference Accelerators<a name="eia-considerations"></a>

Before you begin working with Elastic Inference accelerators on Amazon ECS, be aware of the following considerations:
+ This feature is supported when using Linux containers and tasks that use the EC2 launch type\. Tasks using the Fargate launch type and Windows containers are not supported at this time\.
+ Your container image must contain an Elastic Inference enabled version of TensorFlow or MXNet, otherwise the task will fail or will continue to run using local resources\. We provide an example Dockerfile that works in the [Create a Docker Image](#eia-create-image) section\.

## Prerequisites<a name="eia-prerequisites"></a>

Before you begin working with Elastic Inference accelerators on Amazon ECS, the following must be completed\.
+ An interface VPC endpoint must be created to enable your container instance to privately connect with the associated Elastic Inference accelerator\. For more information, see [Configuring AWS PrivateLink Endpoint Services](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/setting-up-ei.html) in the *Amazon Elastic Inference Developer Guide*\.
+ To enable your task to connect to the Elastic Inference accelerator, you must also either create a new task IAM role, or edit an existing one, with the following inline policy text\. For more information, see [IAM Roles for Tasks](task-iam-roles.md)\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "elastic-inference:Connect",
                  "iam:List*",
                  "iam:Get*",
                  "ec2:Describe*",
                  "ec2:Get*"
              ],
              "Resource": "*"
          }
      ]
  }
  ```
+ You should have an Amazon ECS cluster with at least one container instance registered to it\. For more information, see [Creating a Cluster](create_cluster.md)\.

## Create a Docker Image<a name="eia-create-image"></a>

The following steps walk you through creating a Dockerfile that uses the latest AWS Deep Learning Containers image which enable you to take advantage of Amazon Elastic Inference on Amazon ECS\.

**To create a Docker image**

1. Create a file called `Dockerfile`\. A Dockerfile is a manifest that describes the base image to use for your Docker image and what you want installed and running on it\. For more information about Dockerfiles, go to the [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)\.

   ```
   touch Dockerfile
   ```

1. Log in to the AWS Deep Learning Containers image repository\. Specify your Region in the following command\.

   ```
   $(aws ecr get-login --no-include-email --region us-west-2 --registry-ids 763104351884)
   ```

1. Edit the `Dockerfile` you just created and add the following content, ensuring that you replace the Region reference with the Region you authenticated to in the previous step\. This example Dockerfile will work with the TensorFlow framework\.

   ```
   FROM 763104351884.dkr.ecr.us-west-2.amazonaws.com/tensorflow-inference:1.13-cpu-py27-ubuntu16.04
   
   RUN wget 'https://amazonei-healthcheck.s3.amazonaws.com/v1.3.3/ei_health_check_1.3.3.tar.gz' -O /home/ei_health_check_1.3.3.tar.gz
   
   RUN tar -xvf /home/ei_health_check_1.3.3.tar.gz -C /opt \
       && rm -rf /home/ei_health_check_1.3.3.tar.gz
   
   RUN apt-get --assume-yes install unzip
   RUN wget https://s3.amazonaws.com/amazonei-tensorflow/tensorflow-serving/v1.13/ubuntu/latest/tensorflow-serving-1-13-1-ubuntu-ei-1-1.tar.gz \
       && tar xzvf tensorflow-serving-1-13-1-ubuntu-ei-1-1.tar.gz \
       && chmod +x tensorflow-serving-1-13-1-ubuntu-ei-1-1/amazonei_tensorflow_model_server \
       && rm -f tensorflow-serving-1-13-1-ubuntu-ei-1-1.tar.gz
   
   RUN wget https://s3-us-west-2.amazonaws.com/aws-tf-serving-ei-example/ssd_resnet.zip -P /models/ssdresnet/ \
       && unzip -j /models/ssdresnet/ssd_resnet.zip -d /models/ssdresnet/1
   
   HEALTHCHECK CMD LD_LIBRARY_PATH=/opt/ei_health_check/lib /opt/ei_health_check/bin/health_check
   ENTRYPOINT tensorflow-serving-1-13-1-ubuntu-ei-1-1/amazonei_tensorflow_model_server --port=8001 --rest_api_port=8002 --model_name=ssdresnet --model_base_path=/models/ssdresnet
   ```

   This Dockerfile uses the AWS Deep Learning Containers image\. The `RUN` instructions pulls Elastic Inference healthcheck binaries and an Elastic Inference\-enabled TensorFlow framework\.

1. Build the Docker image from your Dockerfile\.
**Note**  
Some versions of Docker may require the full path to your Dockerfile in the following command, instead of the relative path shown below\.

   ```
   docker build -t eia-demo .
   ```

1. Run docker images to verify that the image was created correctly\.

   ```
   docker images --filter reference=eia-demo
   ```

   Output:

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   eia-demo            latest              8713dfee52a1        23 minutes ago      1.78GB
   ```

1. In the next section we show steps for pushing the image to Amazon ECR\. You can use Amazon ECR or your choice of public or private image repositories\.

## \(Optional\) Push your image to Amazon Elastic Container Registry<a name="eia-use-ecr"></a>

Amazon ECR is a managed AWS Docker registry service\. The following shows how to create a new repository and push the Docker image to it so the image can be referenced in an Amazon ECS task definition\.

This section requires the following:
+ You have the AWS CLI installed and configured\. If you do not have the AWS CLI installed on your system, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide*\.
+ Your user has the required IAM permissions to access the Amazon ECR service\. For more information, see [Amazon ECR Managed Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/ecr_managed_policies.html)\.

**To tag your image and push it to Amazon ECR**

1. Create an Amazon ECR repository to store your `eia-demo` image\. Note the `repositoryUri` in the output\.

   ```
   aws ecr create-repository --repository-name eia-demo --region us-west-2
   ```

   Output:

   ```
   {
       "repository": {
           "registryId": "aws_account_id",
           "repositoryName": "eia-demo",
           "repositoryArn": "arn:aws:ecr:us-west-2:aws_account_id:repository/eia-demo",
           "createdAt": 1505337806.0,
           "repositoryUri": "aws_account_id.dkr.ecr.us-west-2.amazonaws.com/eia-demo"
       }
   }
   ```

1. Tag the `eia-demo` image with the `repositoryUri` value from the previous step\.

   ```
   docker tag eia-demo aws_account_id.dkr.ecr.us-west-2.amazonaws.com/eia-demo
   ```

1. Run the aws ecr get\-login \-\-no\-include\-email command to get the docker login authentication command string for your registry\. 

   ```
   aws ecr get-login --no-include-email --region us-west-2
   ```

1. Run the docker login command that was returned in the previous step\. This command provides an authorization token that is valid for 12 hours\.
**Important**  
When you execute this docker login command, the command string can be visible to other users on your system in a process list \(ps \-e\) display\. Because the docker login command contains authentication credentials, there is a risk that other users on your system could view them this way\. They could use the credentials to gain push and pull access to your repositories\. If you are not on a secure system, you should consider this risk and log in interactively by omitting the `-p password` option, and then entering the password when prompted\.

1. Push the image to Amazon ECR with the `repositoryUri` value from the earlier step\.

   ```
   docker push aws_account_id.dkr.ecr.us-west-2.amazonaws.com/eia-demo
   ```

## Specifying an Inference Accelerator in Your Task Definition<a name="ecs-eia-specifying"></a>

The following shows the JSON format for the Elastic Inference accelerator requirements in a task definition\. The container healthcheck confirms that the Elastic Inference accelerator is reachable\. If the healthcheck fails, check the container logs to troubleshoot further\.

For more information on available Elastic Inference accelerator types, see [Amazon Elastic Inference Basics](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/basics.html)\.

```
{
   "containerDefinitions": [ 
      { 
         ...
         "resourceRequirements": [ 
            { 
               "type": "InferenceAccelerator",
               "value": "eia_device1"
            }
         ],
         "healthCheck": {
             "retries": 2,
             "command": [
                  "CMD-SHELL",
                  "LD_LIBRARY_PATH=/opt/ei_health_check/lib /opt/ei_health_check/bin/health_check"
             ],
             "timeout": 15,
             "interval": 30,
             "startPeriod": 60
         },
         ...
      }
   ],
   "inferenceAccelerators": [ 
      { 
         "deviceName": "eia_device1",
         "deviceType": "eia1.medium"
      }
   ],
   ...
}
```