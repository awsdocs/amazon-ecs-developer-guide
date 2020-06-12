# Using Amazon ECR with Amazon ECS<a name="ecr-repositories"></a>

Amazon ECR is a managed AWS Docker registry service\. Customers can use the familiar Docker CLI to push, pull, and manage images\. Amazon ECR provides a secure, scalable, and reliable registry\. Amazon ECR supports private Docker repositories with resource\-based permissions using AWS IAM so that specific users or Amazon EC2 instances can access repositories and images\. Developers can use the Docker CLI to author and manage images\.

For more information on how to create repositories, push and pull images from Amazon ECR, and set access controls on your repositories, see the [Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)\.

## Using Amazon ECR Images with Amazon ECS<a name="ECR_on_ECS_ecs-dg"></a>

You can use your ECR images with Amazon ECS, but you need to satisfy the following prerequisites\.
+ Your container instances must be using at least version 1\.7\.0 of the Amazon ECS container agent\. The latest version of the Amazon ECS–optimized AMI supports ECR images in task definitions\. For more information, including the latest Amazon ECS–optimized AMI IDs, see [Amazon ECS Container Agent Versions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-agent-versions.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ The Amazon ECS container instance role \(`ecsInstanceRole`\) that you use with your container instances must possess the following IAM policy permissions for Amazon ECR\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "ecr:BatchCheckLayerAvailability",
                  "ecr:BatchGetImage",
                  "ecr:GetDownloadUrlForLayer",
                  "ecr:GetAuthorizationToken"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

  If you use the `AmazonEC2ContainerServiceforEC2Role` managed policy for your container instances, then your role has the proper permissions\. To check that your role supports Amazon ECR, see [Amazon ECS Container Instance IAM Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ In your ECS task definitions, make sure that you are using the full `registry/repository:tag` naming for your ECR images\. For example, `aws_account_id.dkr.ecr.region.amazonaws.com``/my-web-app:latest`\.