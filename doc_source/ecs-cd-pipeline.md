# Tutorial: Continuous Deployment with CodePipeline<a name="ecs-cd-pipeline"></a>

This tutorial helps you to create a complete, end\-to\-end continuous deployment \(CD\) pipeline with Amazon ECS with CodePipeline\.

## Prerequisites<a name="ecs-cd-prereqs"></a>

There are a few resources that you must have in place before you can use this tutorial to create your CD pipeline\. Here are the things you need to get started:

**Note**  
All of these resources should be created within the same AWS Region\.
+ A source control repository \(this tutorial uses CodeCommit\) with your Dockerfile and application source\. For more information, see [Create an CodeCommit Repository](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-create-repository.html) in the *AWS CodeCommit User Guide*\.
+ A Docker image repository \(this tutorial uses Amazon ECR\) that contains an image you have built from your Dockerfile and application source\. For more information, see [Creating a Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/repository-create.html) and [Pushing an Image](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html) in the *Amazon Elastic Container Registry User Guide*\.
+ An Amazon ECS task definition that references the Docker image hosted in your image repository\. For more information, see [Creating a Task Definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ An Amazon ECS cluster that is running a service that uses your previously mentioned task definition\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create_cluster.html) and [Creating a Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-service.html) in the *Amazon Elastic Container Service Developer Guide*\.

After you have satisfied these prerequisites, you can proceed with the tutorial and create your CD pipeline\.

## Step 1: Add a Build Specification File to Your Source Repository<a name="cd-buildspec"></a>

This tutorial uses CodeBuild to build your Docker image and push the image to Amazon ECR\. Add a `buildspec.yml` file to your source code repository to tell CodeBuild how to do that\. The example build specification below does the following:
+ Pre\-build stage:
  + Log in to Amazon ECR\.
  + Set the repository URI to your ECR image and add an image tag with the first seven characters of the Git commit ID of the source\.
+ Build stage:
  + Build the Docker image and tag the image both as `latest` and with the Git commit ID\.
+ Post\-build stage:
  + Push the image to your ECR repository with both tags\.
  + Write a file called `imagedefinitions.json` in the build root that has your Amazon ECS service's container name and the image and tag\. The deployment stage of your CD pipeline uses this information to create a new revision of your service's task definition, and then it updates the service to use the new task definition\. The `imagedefinitions.json` file is required for the ECS job worker\.

```
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws --version
      - $(aws ecr get-login --region $AWS_DEFAULT_REGION --no-include-email)
      - REPOSITORY_URI=012345678910.dkr.ecr.us-west-2.amazonaws.com/hello-world
      - COMMIT_HASH=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      - IMAGE_TAG=${COMMIT_HASH:=latest}
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...
      - docker build -t $REPOSITORY_URI:latest .
      - docker tag $REPOSITORY_URI:latest $REPOSITORY_URI:$IMAGE_TAG
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker images...
      - docker push $REPOSITORY_URI:latest
      - docker push $REPOSITORY_URI:$IMAGE_TAG
      - echo Writing image definitions file...
      - printf '[{"name":"hello-world","imageUri":"%s"}]' $REPOSITORY_URI:$IMAGE_TAG > imagedefinitions.json
artifacts:
    files: imagedefinitions.json
```

The build specification was written for the following task definition, used by the Amazon ECS service for this tutorial\. The `REPOSITORY_URI` value corresponds to the `image` repository \(without any image tag\), and the `hello-world` value near the end of the file corresponds to the container name in the service's task definition\. 

```
{
  "taskDefinition": {
    "family": "hello-world",
    "containerDefinitions": [
      {
        "name": "hello-world",
        "image": "012345678910.dkr.ecr.us-west-2.amazonaws.com/hello-world:latest",
        "cpu": 100,
        "portMappings": [
          {
            "protocol": "tcp",
            "containerPort": 80,
            "hostPort": 80
          }
        ],
        "memory": 128,
        "essential": true
      }
    ]
  }
}
```

**To add a `buildspec.yml` file to your source repository**

1. Open a text editor and then copy and paste the build specification above into a new file\.

1. Replace the `REPOSITORY_URI` value \(`012345678910.dkr.ecr.us-west-2.amazonaws.com/hello-world`\) with your Amazon ECR repository URI \(without any image tag\) for your Docker image\. Replace `hello-world` with the container name in your service's task definition that references your Docker image\.

1. Commit and push your `buildspec.yml` file to your source repository\.

   1. Add the file\.

      ```
      git add .
      ```

   1. Commit the change\.

      ```
      git commit -m "Adding build specification."
      ```

   1. Push the commit\.

      ```
      git push
      ```

## Step 2: Creating Your Continuous Deployment Pipeline<a name="pipeline-wizard"></a>

Use the CodePipeline wizard to create your pipeline stages and connect your source repository to your ECS service\.

**To create your pipeline**

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)\.

1. On the **Welcome** page, choose **Create pipeline**\. 

   If this is your first time using CodePipeline, an introductory page appears instead of **Welcome**\. Choose **Get Started Now**\.

1. On the **Step 1: Name** page, for **Pipeline name**, type the name for your pipeline and choose **Next step**\. For this tutorial, the pipeline name is **hello\-world**\.

1. On the **Step 2: Source** page, for **Source provider**, choose **CodeCommit**\.

   1. For **Repository name**, choose the name of the CodeCommit repository to use as the source location for your pipeline\.

   1. For **Branch name**, choose the branch to use and choose **Next step**\.

1. On the **Step 3: Build** page, choose **CodeBuild**, and then choose **Create a new build project**\.

   1. For **Project name**, choose a unique name for your build project\. For this tutorial, the project name is **hello\-world**\.

   1. For **Operating system**, choose **Ubuntu**\.

   1. For **Runtime**, choose **Docker**\.

   1. For **Version**, choose **aws/codebuild/docker:17\.09\.0 **\.

   1. Choose **Save build project**\.

   1. Choose **Next step**\.
**Note**  
The wizard creates an CodeBuild service role for your build project, called **code\-build\-*build\-project\-name*\-service\-role**\. Note this role name, as you add Amazon ECR permissions to it later\.

1. On the **Step 4: Deploy** page, for **Deployment provider**, choose **Amazon ECS**\.

   1. For **Cluster name**, choose the Amazon ECS cluster in which your service is running\. For this tutorial, the cluster is **default**\.

   1. For **Service name**, choose the service to update and choose **Next step**\. For this tutorial, the service name is **hello\-world**\.

1. On the **Step 5: Service Role** page, choose **Create role**\. On the IAM console page that describes the role to be created for you, choose **Allow**\. 

1. Choose **Next step**\.

1. On the **Step 6: Review** page, review your pipeline configuration and choose **Create pipeline** to create the pipeline\.
**Note**  
Now that the pipeline has been created, it attempts to run through the different pipeline stages\. However, the default CodeBuild role created by the wizard does not have permissions to execute all of the commands contained in the `buildspec.yml` file, so the build stage fails\. The next section adds the permissions for the build stage\.

## Step 3: Add Amazon ECR Permissions to the CodeBuild Role<a name="code-build-perms"></a>

The CodePipeline wizard created an IAM role for the CodeBuild build project, called **code\-build\-*build\-project\-name*\-service\-role**\. For this tutorial, the name is **code\-build\-hello\-world\-service\-role**\. Because the `buildspec.yml` file makes calls to Amazon ECR API operations, the role must have a policy that allows permissions to make these Amazon ECR calls\. The following procedure helps you attach the proper permissions to the role\.

**To add Amazon ECR permissions to the CodeBuild role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. In the search box, type **code\-build\-** and choose the role that was created by the CodePipeline wizard\. For this tutorial, the role name is **code\-build\-hello\-world\-service\-role**\.

1. On the **Summary** page, choose **Attach policy**\.

1. Select the box to the left of the **AmazonEC2ContainerRegistryPowerUser** policy, and choose **Attach policy**\.

## Step 4: Test Your Pipeline<a name="commit-change"></a>

Your pipeline should have everything for running an end\-to\-end native AWS continuous deployment\. Now, test its functionality by pushing a code change to your source repository\.

**To test your pipeline**

1. Make a code change to your configured source repository, commit, and push the change\.

1. Open the CodePipeline console at [https://console\.aws\.amazon\.com/codepipeline/](https://console.aws.amazon.com/codepipeline/)\.

1. Choose your pipeline from the list\.

1. Watch the pipeline progress through its stages\. Your pipeline should complete and your Amazon ECS service runs the Docker image that was created from your code change\.  
![\[CodePipeline completed\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/code-pipeline-complete.png)