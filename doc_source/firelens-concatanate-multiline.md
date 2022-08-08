# Concatenate multiline or stack\-trace log messages<a name="firelens-concatanate-multiline"></a>

Beginning with AWS for Fluent Bit version 2\.22\.0, a multiline filter is included\. The multiline filter helps concatenate log messages that originally belong to one context but were split across multiple records or log lines\. For more information on the multiline filter, see the [ Fluent Bit documentation\.](https://docs.fluentbit.io/manual/pipeline/filters/multiline-stacktrace) 

Common examples of split log messages are:
+ Stack traces\. Follow the steps below to concatenate messages split by newlines\.
+ Applications that print logs on multiple lines\. Follow the steps below to concatenate messages split by newlines\.
+ Log messages that were split because they were longer than the specified runtime max buffer size\. You can concatenate log messages split by the container runtime by following the example on GitHub: [FireLens Example: Concatenate Partial/Split Container Logs](https://github.com/aws-samples/amazon-ecs-firelens-examples/tree/mainline/examples/fluent-bit/filter-multiline-partial-message-mode)\.

Compare the images of the logs below to determine if you need the multiline filter\.

------
#### [ Before ]

The following image shows the CloudWatch Logs console with the default log setting with multiple entries for each line in a stack trace\. 

![\[Shows the CloudWatch Logs console with the default log setting with multiple entries for each line in a stack trace.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ecs-multiline-filter-before.png)

------
#### [ After ]

The following image shows the CloudWatch Logs console with the multiline log setting with a single entry that includes all the lines in a stack trace in the `log` field\. 

![\[Shows the CloudWatch Logs console with the multiline log setting with a single entry that includes all the lines in a stack trace.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ecs-multiline-filter-after.png)

------

To parse logs and concatenate lines that were split because of newlines, you can use either of these two options\.
+ Create your own parser file that contains the rules to parse and concatenate lines that belong to the same message\.
+ Use a Fluent Bit built\-in parser\. For a list of languages supported by the Fluent Bit built\-in parsers, see [ Fluent Bit documentation\.](https://docs.fluentbit.io/manual/pipeline/filters/multiline-stacktrace)

The following tutorial walks you through the steps for each use case\. The steps show you how to concatenate multilines and send the logs to Amazon CloudWatch\. You can specify a different destination for your logs\.

## Required IAM permissions<a name="w610aac17c52c21c19b1"></a>

For each use case you must first make sure you have the necessary IAM permissions for the container agent to pull the container images from Amazon ECR and for the container to route logs to CloudWatch Logs\.

For these permissions, you must have the following roles: 
+ A task IAM role\. 
+ An Amazon ECS task execution IAM role\. 

**Create the task IAM role**

This task role grants the FireLens log router container the permissions needed to route the logs to the destination\. In this example we are routing the logs to CloudWatch Logs\. To create this role, create a policy with the permissions to create a log stream, log group, and write log events\. Then associate the policy to the role\. 

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and then choose **Create Policy**\. 

1. Choose **JSON** and paste the following permissions: 

   ```
   {
   	"Version": "2012-10-17",
   	"Statement": [{
   		"Effect": "Allow",
   		"Action": [
   			"logs:CreateLogStream",
   			"logs:CreateLogGroup",
   			"logs:PutLogEvents"
   		],
   		"Resource": "*"
   	}]
   }
   ```

1. Choose **Next: Tags** and add any tags to the policy to help you organize them\. Then choose **Next: Review**\. 

1. On the **Review policy** page, for **Name** type a unique name for the policy\. In this example, we will use `ecs-policy-for-firelens`\. You may specify an optional description for the policy as well\. 

1. Choose **Create policy** to finish\. 

1. In the navigation pane, choose **Roles** and then choose **Create Roles**\. 

1. In the **Trusted entity type** section, choose **AWS service**\. 

1. For **Use case**, choose **Elastic Container Service\.**\.

1. Choose **Elastic Container Service Task** and then **Next**\. 

1. Associate the role with the `ecs-policy-for-firelens` policy you created and choose **Next**\. 

1. Enter a unique name for the role\. In this example, use: **ecs\-task\-role\-for\-firelens**\. 

**Verify that you have an Amazon ECS task execution IAM role**

You must have a task execution role to grant the container agent permission to pull the container images from Amazon ECR\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then search for `ecsTaskExecutionRole`\. 

1. If you do not see the `ecsTaskExecutionRole` role, you must create the role\. For information on how to create the role, see [Amazon ECS task execution IAM role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Example: Use a parser that you create<a name="w610aac17c52c21c19b3"></a>

In this example, you will complete the following steps: 

1. Build and upload the image for a Fluent Bit container\. 

1. Build and upload the image for a demo multiline application that runs, fails, and generates a multiline stack trace\.

1. Create the task definition and run the task\. 

1. View the logs to verify that messages that span multiple lines appear concatenated\. 

**Build and upload the image for a Fluent Bit container**

This image will include the parser file where you specify the regular expression and a configuration file that references the parser file\. 

1. Create a folder with the name `FluentBitDockerImage`\. 

1. Within the folder, create a parser file that contains the rules to parse the log and concatenate lines that belong in the same message\.

   1. Paste the following contents in the parser file:

      ```
      [MULTILINE_PARSER]
          name          multiline-regex-test
          type          regex
          flush_timeout 1000
          #
          # Regex rules for multiline parsing
          # ---------------------------------
          #
          # configuration hints:
          #
          #  - first state always has the name: start_state
          #  - every field in the rule must be inside double quotes
          #
          # rules |   state name  | regex pattern                  | next state
          # ------|---------------|--------------------------------------------
          rule      "start_state"   "/(Dec \d+ \d+\:\d+\:\d+)(.*)/"  "cont"
          rule      "cont"          "/^\s+at.*/"                     "cont"
      ```

      As you customize your regex pattern, we recommend you use a regular expression editor to test the expression\.

   1. Save the file as `parsers_multiline.conf`\. 

1. Within the `FluentBitDockerImage` folder, create a custom configuration file that references the parser file that you created in the previous step\.

   For more information on the custom configuration file, see [Specifying a custom configuration file](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/firelens-taskdef.html#firelens-taskdef-customconfig) in the *Amazon Elastic Container Service Developer Guide* 

   1. Paste the following contents in the file:

      ```
      [SERVICE]
          flush                 1
          log_level             info
          parsers_file          /parsers_multiline.conf
          
      [FILTER]
          name                  multiline
          match                 *
          multiline.key_content log
          multiline.parser      multiline-regex-test
      ```
**Note**  
You must use the absolute path of the parser\. 

   1. Save the file as `extra.conf`\. 

1. Within the `FluentBitDockerImage` folder, create the Dockerfile with the Fluent Bit image and the parser and configuration files that you created\.

   1. Paste the following contents in the file:

      ```
      FROM public.ecr.aws/aws-observability/aws-for-fluent-bit:latest
      
      ADD parsers_multiline.conf /parsers_multiline.conf
      ADD extra.conf /extra.conf
      ```

   1. Save the file as `Dockerfile`\.

1. Using the Dockerfile, build a custom Fluent Bit image with the parser and custom configuration files included\.
**Note**  
You can place the parser file and configuration file anywhere in the Docker image except `/fluent-bit/etc/fluent-bit.conf` as this file path is used by FireLens\.

   1. Build the image: `docker build -t fluent-bit-multiline-image .`

      Where: `fluent-bit-multiline-image` is the name for the image in this example\.

   1. Verify that the image was created correctly: `docker images —filter reference=fluent-bit-multiline-image` 

      If successful, the output shows the image and the `latest` tag\.

1. Upload the custom Fluent Bit image to Amazon Elastic Container Registry\.

   1. Create an Amazon ECR repository to store the image: `aws ecr create-repository --repository-name fluent-bit-multiline-repo --region us-east-1`

      Where: `fluent-bit-multiline-repo` is the name for the repository and `us-east-1` is the region in this example\. 

      The output gives you the details of the new repository\. 

   1. Tag your image with the `repositoryUri` value from the previous output: `docker tag fluent-bit-multiline-image repositoryUri` 

      Example: `docker tag fluent-bit-multiline-image xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-repo` 

   1. Run the docker image to verify it ran correctly: `docker images —filter reference=repositoryUri`

      In the output, the repository name changes from fluent\-bit\-multiline\-repo to the `repositoryUri`\.

   1. Authenticate to Amazon ECR by running the `aws ecr get-login-password` command and specifying the registry ID you want to authenticate to: `aws ecr get-login-password | docker login --username AWS --password-stdin registry ID.dkr.ecr.region.amazonaws.com` 

      Example: `ecr get-login-password | docker login --username AWS --password-stdin xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com`

      A successful login message appears\.

   1. Push the image to Amazon ECR: `docker push registry ID.dkr.ecr.region.amazonaws.com/repository name` 

      Example: `docker push xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-repo`

**Build and upload the image for a demo multiline application**

This image will include a Python script file that runs the application and a sample log file\. 

When you run the task, the application simulates runs, then fails and creates a stack trace\. 

1. Create a folder named `multiline-app`: `mkdir multiline-app` 

1. Create a Python script file\.

   1. Within the `multiline-app` folder, create a file and name it `main.py`\.

   1. Paste the following contents in the file:

      ```
      import os
      import time
      file1 = open('/test.log', 'r')
      Lines = file1.readlines()
       
      count = 0
      
      for i in range(10):
          print("app running normally...")
          time.sleep(1)
      
      # Strips the newline character
      for line in Lines:
          count += 1
          print(line.rstrip())
      print(count)
      print("app terminated.")
      ```

   1. Save the `main.py` file\.

1. Create a sample log file\. 

   1. Within the `multiline-app` folder, create a file and name it `test.log`\.

   1. Paste the following contents in the file:

      ```
      single line...
      Dec 14 06:41:08 Exception in thread "main" java.lang.RuntimeException: Something has gone wrong, aborting!
          at com.myproject.module.MyProject.badMethod(MyProject.java:22)
          at com.myproject.module.MyProject.oneMoreMethod(MyProject.java:18)
          at com.myproject.module.MyProject.anotherMethod(MyProject.java:14)
          at com.myproject.module.MyProject.someMethod(MyProject.java:10)
          at com.myproject.module.MyProject.main(MyProject.java:6)
      another line...
      ```

   1. Save the `test.log` file\.

1. Within the `multiline-app` folder, create the Dockerfile\.

   1. Paste the following contents in the file:

      ```
      FROM public.ecr.aws/amazonlinux/amazonlinux:latest
      ADD test.log /test.log
      
      RUN yum upgrade -y && yum install -y python3
      
      WORKDIR /usr/local/bin
      
      COPY main.py .
      
      CMD ["python3", "main.py"]
      ```

   1. Save the `Dockerfile` file\.

1. Using the Dockerfile, build an image\.

   1. Build the image: `docker build -t multiline-app-image .`

      Where: `multiline-app-image` is the name for the image in this example\.

   1. Verify that the image was created correctly: `docker images —filter reference=multiline-app-image` 

      If successful, the output shows the image and the `latest` tag\.

1. Upload the image to Amazon Elastic Container Registry\.

   1. Create an Amazon ECR repository to store the image: `aws ecr create-repository --repository-name multiline-app-repo --region us-east-1`

      Where: `multiline-app-repo` is the name for the repository and `us-east-1` is the region in this example\. 

      The output gives you the details of the new repository\. Note the `repositoryUri` value as you will need it in the next steps\. 

   1. Tag your image with the `repositoryUri` value from the previous output: `docker tag multiline-app-image repositoryUri` 

      Example: `docker tag multiline-app-image xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/multiline-app-repo` 

   1. Run the docker image to verify it ran correctly: `docker images —filter reference=repositoryUri`

      In the output, the repository name changes from `multiline-app-repo` to the `repositoryUri` value\.

   1. Push the image to Amazon ECR: `docker push aws_account_id.dkr.ecr.region.amazonaws.com/repository name` 

      Example: `docker push xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/multiline-app-repo`

**Create the task definition and run the task**

1. Create a task definition file with the file name `multiline-task-definition.json`\. 

1. Paste the following contents in the `multiline-task-definition.json` file: 

   ```
   {
       "family": "firelens-example-multiline",
       "taskRoleArn": "task role ARN,
       "executionRoleArn": "execution role ARN",
       "containerDefinitions": [
           {
               "essential": true,
               "image": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-image:latest",
               "name": "log_router",
               "firelensConfiguration": {
                   "type": "fluentbit",
                   "options": {
                       "config-file-type": "file",
                       "config-file-value": "/extra.conf"
                   }
               },
               "memoryReservation": 50
           },
           {
               "essential": true,
               "image": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/multiline-app-image:latest",
               "name": "app",
               "logConfiguration": {
                   "logDriver": "awsfirelens",
                   "options": {
                       "Name": "cloudwatch_logs",
                       "region": "us-east-1",
                       "log_group_name": "multiline-test/application",
                       "auto_create_group": "true",
                       "log_stream_prefix": "multiline-"
                   }
               },
               "memoryReservation": 100
           }
       ],
       "requiresCompatibilities": ["FARGATE"],
       "networkMode": "awsvpc",
       "cpu": "256",
       "memory": "512"
   }
   ```

   Replace the following in the `multiline-task-definition.json` task definition:

   1. `task role ARN`

      To find the task role ARN, go to the IAM console\. Choose **Roles** and find the `ecs-task-role-for-firelens` task role that you created\. Choose the role and copy the **ARN** that appears in the **Summary** section\.

   1. `execution role ARN`

      To find the execution role ARN, go to the IAM console\. Choose **Roles** and find the `ecsTaskExecutionRole` role\. Choose the role and copy the **ARN** that appears in the **Summary** section\.

   1. `aws_account_id`

      To find your `aws_account_id`, log into the AWS Management Console\. Choose your user name on the top right and copy your Account ID\.

   1. `us-east-1`

      Replace the region if necessary\.

1. Register the task definition file: `aws ecs register-task-definition --cli-input-json file://multiline-task-definition.json --region us-east-1` 

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions** and then choose the `firelens-example-multiline` family because we registered the task definition to this family in the first line of the task definition above\.

1. Choose the latest version\. 

1. Choose the **Actions**, **Run Task**\. 

1. For **Launch type**, choose **Fargate**\. 

1. For **Subnets**, choose the available subnets for your task\. 

1. Choose **Run Task**\. 

**Verify that multiline log messages in Amazon CloudWatch appear concatenated**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. From the navigation pane, expand **Logs** and choose **Log groups**\. 

1. Choose the `multiline-test/applicatio` log group\. 

1. Choose the log\. View messages\. Lines that matched the rules in the parser file are concatenated and appear as a single message\. 

   The following log snippet shows lines concatenated in a single Java stack trace event: 

   ```
   {
       "container_id": "xxxxxx",
       "container_name": "app",
       "source": "stdout",
       "log": "Dec 14 06:41:08 Exception in thread \"main\" java.lang.RuntimeException: Something has gone wrong, aborting!\n    at com.myproject.module.MyProject.badMethod(MyProject.java:22)\n    at com.myproject.module.MyProject.oneMoreMethod(MyProject.java:18)\n    at com.myproject.module.MyProject.anotherMethod(MyProject.java:14)\n    at com.myproject.module.MyProject.someMethod(MyProject.java:10)\n    at com.myproject.module.MyProject.main(MyProject.java:6)",
       "ecs_cluster": "default",
       "ecs_task_arn": "arn:aws:ecs:us-east-1:xxxxxxxxxxxx:task/default/xxxxxx",
       "ecs_task_definition": "firelens-example-multiline:2"
   }
   ```

   The following log snippet shows how the same message appears with just a single line if you run an ECS container that is not configured to concatenate multiline log messages\. 

   ```
   {
       "log": "Dec 14 06:41:08 Exception in thread \"main\" java.lang.RuntimeException: Something has gone wrong, aborting!",
       "container_id": "xxxxxx-xxxxxx",
       "container_name": "app",
       "source": "stdout",
       "ecs_cluster": "default",
       "ecs_task_arn": "arn:aws:ecs:us-east-1:xxxxxxxxxxxx:task/default/xxxxxx",
       "ecs_task_definition": "firelens-example-multiline:3"
   }
   ```

## Example: Use a Fluent Bit built\-in parser<a name="w610aac17c52c21c19b5"></a>

In this example, you will complete the following steps: 

1. Build and upload the image for a Fluent Bit container\. 

1. Build and upload the image for a demo multiline application that runs, fails, and generates a multiline stack trace\.

1. Create the task definition and run the task\. 

1. View the logs to verify that messages that span multiple lines appear concatenated\. 

**Build and upload the image for a Fluent Bit container**

This image will include a configuration file that references the Fluent Bit parser\. 

1. Create a folder with the name `FluentBitDockerImage`\. 

1. Within the `FluentBitDockerImage` folder, create a custom configuration file that references the Fluent Bit built\-in parser file\.

   For more information on the custom configuration file, see [Specifying a custom configuration file](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/firelens-taskdef.html#firelens-taskdef-customconfig) in the *Amazon Elastic Container Service Developer Guide* 

   1. Paste the following contents in the file:

      ```
      [FILTER]
          name                  multiline
          match                 *
          multiline.key_content log
          multiline.parser      go
      ```

   1. Save the file as `extra.conf`\. 

1. Within the `FluentBitDockerImage` folder, create the Dockerfile with the Fluent Bit image and the parser and configuration files that you created\.

   1. Paste the following contents in the file:

      ```
      FROM public.ecr.aws/aws-observability/aws-for-fluent-bit:latest
      ADD extra.conf /extra.conf
      ```

   1. Save the file as `Dockerfile`\.

1. Using the Dockerfile, build a custom Fluent Bit image with the custom configuration file included\.
**Note**  
You can place the configuration file anywhere in the Docker image except `/fluent-bit/etc/fluent-bit.conf` as this file path is used by FireLens\.

   1. Build the image: `docker build -t fluent-bit-multiline-image .`

      Where: `fluent-bit-multiline-image` is the name for the image in this example\.

   1. Verify that the image was created correctly: `docker images —filter reference=fluent-bit-multiline-image` 

      If successful, the output shows the image and the `latest` tag\.

1. Upload the custom Fluent Bit image to Amazon Elastic Container Registry\.

   1. Create an Amazon ECR repository to store the image: `aws ecr create-repository --repository-name fluent-bit-multiline-repo --region us-east-1`

      Where: `fluent-bit-multiline-repo` is the name for the repository and `us-east-1` is the region in this example\. 

      The output gives you the details of the new repository\. 

   1. Tag your image with the `repositoryUri` value from the previous output: `docker tag fluent-bit-multiline-image repositoryUri` 

      Example: `docker tag fluent-bit-multiline-image xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-repo` 

   1. Run the docker image to verify it ran correctly: `docker images —filter reference=repositoryUri`

      In the output, the repository name changes from fluent\-bit\-multiline\-repo to the `repositoryUri`\.

   1. Authenticate to Amazon ECR by running the `aws ecr get-login-password` command and specifying the registry ID you want to authenticate to: `aws ecr get-login-password | docker login --username AWS --password-stdin registry ID.dkr.ecr.region.amazonaws.com` 

      Example: `ecr get-login-password | docker login --username AWS --password-stdin xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com`

      A successful login message appears\.

   1. Push the image to Amazon ECR: `docker push registry ID.dkr.ecr.region.amazonaws.com/repository name` 

      Example: `docker push xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-repo`

**Build and upload the image for a demo multiline application**

This image will include a Python script file that runs the application and a sample log file\. 

1. Create a folder named `multiline-app`: `mkdir multiline-app` 

1. Create a Python script file\.

   1. Within the `multiline-app` folder, create a file and name it `main.py`\.

   1. Paste the following contents in the file:

      ```
      import os
      import time
      file1 = open('/test.log', 'r')
      Lines = file1.readlines()
       
      count = 0
      
      for i in range(10):
          print("app running normally...")
          time.sleep(1)
      
      # Strips the newline character
      for line in Lines:
          count += 1
          print(line.rstrip())
      print(count)
      print("app terminated.")
      ```

   1. Save the `main.py` file\.

1. Create a sample log file\. 

   1. Within the `multiline-app` folder, create a file and name it `test.log`\.

   1. Paste the following contents in the file:

      ```
      panic: my panic
      
      goroutine 4 [running]:
      panic(0x45cb40, 0x47ad70)
        /usr/local/go/src/runtime/panic.go:542 +0x46c fp=0xc42003f7b8 sp=0xc42003f710 pc=0x422f7c
      main.main.func1(0xc420024120)
        foo.go:6 +0x39 fp=0xc42003f7d8 sp=0xc42003f7b8 pc=0x451339
      runtime.goexit()
        /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003f7e0 sp=0xc42003f7d8 pc=0x44b4d1
      created by main.main
        foo.go:5 +0x58
      
      goroutine 1 [chan receive]:
      runtime.gopark(0x4739b8, 0xc420024178, 0x46fcd7, 0xc, 0xc420028e17, 0x3)
        /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc420053e30 sp=0xc420053e00 pc=0x42503c
      runtime.goparkunlock(0xc420024178, 0x46fcd7, 0xc, 0x1000f010040c217, 0x3)
        /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc420053e70 sp=0xc420053e30 pc=0x42512e
      runtime.chanrecv(0xc420024120, 0x0, 0xc420053f01, 0x4512d8)
        /usr/local/go/src/runtime/chan.go:506 +0x304 fp=0xc420053f20 sp=0xc420053e70 pc=0x4046b4
      runtime.chanrecv1(0xc420024120, 0x0)
        /usr/local/go/src/runtime/chan.go:388 +0x2b fp=0xc420053f50 sp=0xc420053f20 pc=0x40439b
      main.main()
        foo.go:9 +0x6f fp=0xc420053f80 sp=0xc420053f50 pc=0x4512ef
      runtime.main()
        /usr/local/go/src/runtime/proc.go:185 +0x20d fp=0xc420053fe0 sp=0xc420053f80 pc=0x424bad
      runtime.goexit()
        /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc420053fe8 sp=0xc420053fe0 pc=0x44b4d1
      
      goroutine 2 [force gc (idle)]:
      runtime.gopark(0x4739b8, 0x4ad720, 0x47001e, 0xf, 0x14, 0x1)
        /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc42003e768 sp=0xc42003e738 pc=0x42503c
      runtime.goparkunlock(0x4ad720, 0x47001e, 0xf, 0xc420000114, 0x1)
        /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc42003e7a8 sp=0xc42003e768 pc=0x42512e
      runtime.forcegchelper()
        /usr/local/go/src/runtime/proc.go:238 +0xcc fp=0xc42003e7e0 sp=0xc42003e7a8 pc=0x424e5c
      runtime.goexit()
        /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003e7e8 sp=0xc42003e7e0 pc=0x44b4d1
      created by runtime.init.4
        /usr/local/go/src/runtime/proc.go:227 +0x35
      
      goroutine 3 [GC sweep wait]:
      runtime.gopark(0x4739b8, 0x4ad7e0, 0x46fdd2, 0xd, 0x419914, 0x1)
        /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc42003ef60 sp=0xc42003ef30 pc=0x42503c
      runtime.goparkunlock(0x4ad7e0, 0x46fdd2, 0xd, 0x14, 0x1)
        /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc42003efa0 sp=0xc42003ef60 pc=0x42512e
      runtime.bgsweep(0xc42001e150)
        /usr/local/go/src/runtime/mgcsweep.go:52 +0xa3 fp=0xc42003efd8 sp=0xc42003efa0 pc=0x419973
      runtime.goexit()
        /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003efe0 sp=0xc42003efd8 pc=0x44b4d1
      created by runtime.gcenable
        /usr/local/go/src/runtime/mgc.go:216 +0x58
      one more line, no multiline
      ```

   1. Save the `test.log` file\.

1. Within the `multiline-app` folder, create the Dockerfile\.

   1. Paste the following contents in the file:

      ```
      FROM public.ecr.aws/amazonlinux/amazonlinux:latest
      ADD test.log /test.log
      
      RUN yum upgrade -y && yum install -y python3
      
      WORKDIR /usr/local/bin
      
      COPY main.py .
      
      CMD ["python3", "main.py"]
      ```

   1. Save the `Dockerfile` file\.

1. Using the Dockerfile, build an image\.

   1. Build the image: `docker build -t multiline-app-image .`

      Where: `multiline-app-image` is the name for the image in this example\.

   1. Verify that the image was created correctly: `docker images —filter reference=multiline-app-image` 

      If successful, the output shows the image and the `latest` tag\.

1. Upload the image to Amazon Elastic Container Registry\.

   1. Create an Amazon ECR repository to store the image: `aws ecr create-repository --repository-name multiline-app-repo --region us-east-1`

      Where: `multiline-app-repo` is the name for the repository and `us-east-1` is the region in this example\. 

      The output gives you the details of the new repository\. Note the `repositoryUri` value as you will need it in the next steps\. 

   1. Tag your image with the `repositoryUri` value from the previous output: `docker tag multiline-app-image repositoryUri` 

      Example: `docker tag multiline-app-image xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/multiline-app-repo` 

   1. Run the docker image to verify it ran correctly: `docker images —filter reference=repositoryUri`

      In the output, the repository name changes from `multiline-app-repo` to the `repositoryUri` value\.

   1. Push the image to Amazon ECR: `docker push aws_account_id.dkr.ecr.region.amazonaws.com/repository name` 

      Example: `docker push xxxxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com/multiline-app-repo`

**Create the task definition and run the task**

1. Create a task definition file with the file name `multiline-task-definition.json`\. 

1. Paste the following contents in the `multiline-task-definition.json` file: 

   ```
   {
       "family": "firelens-example-multiline",
       "taskRoleArn": "task role ARN,
       "executionRoleArn": "execution role ARN",
       "containerDefinitions": [
           {
               "essential": true,
               "image": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/fluent-bit-multiline-image:latest",
               "name": "log_router",
               "firelensConfiguration": {
                   "type": "fluentbit",
                   "options": {
                       "config-file-type": "file",
                       "config-file-value": "/extra.conf"
                   }
               },
               "memoryReservation": 50
           },
           {
               "essential": true,
               "image": "aws_account_id.dkr.ecr.us-east-1.amazonaws.com/multiline-app-image:latest",
               "name": "app",
               "logConfiguration": {
                   "logDriver": "awsfirelens",
                   "options": {
                       "Name": "cloudwatch_logs",
                       "region": "us-east-1",
                       "log_group_name": "multiline-test/application",
                       "auto_create_group": "true",
                       "log_stream_prefix": "multiline-"
                   }
               },
               "memoryReservation": 100
           }
       ],
       "requiresCompatibilities": ["FARGATE"],
       "networkMode": "awsvpc",
       "cpu": "256",
       "memory": "512"
   }
   ```

   Replace the following in the `multiline-task-definition.json` task definition:

   1. `task role ARN`

      To find the task role ARN, go to the IAM console\. Choose **Roles** and find the `ecs-task-role-for-firelens` task role that you created\. Choose the role and copy the **ARN** that appears in the **Summary** section\.

   1. `execution role ARN`

      To find the execution role ARN, go to the IAM console\. Choose **Roles** and find the `ecsTaskExecutionRole` role\. Choose the role and copy the **ARN** that appears in the **Summary** section\.

   1. `aws_account_id`

      To find your `aws_account_id`, log into the AWS Management Console\. Choose your user name on the top right and copy your Account ID\.

   1. `us-east-1`

      Replace the region if necessary\.

1. Register the task definition file: `aws ecs register-task-definition --cli-input-json file://multiline-task-definition.json --region us-east-1` 

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task Definitions** and then choose the `firelens-example-multiline` family because we registered the task definition to this family in the first line of the task definition above\.

1. Choose the latest version\. 

1. Choose the **Actions**, **Run Task**\. 

1. For **Launch type**, choose **Fargate**\. 

1. For **Subnets**, choose the available subnets for your task\. 

1. Choose **Run Task**\. 

**Verify that multiline log messages in Amazon CloudWatch appear concatenated**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. From the navigation pane, expand **Logs** and choose **Log groups**\. 

1. Choose the `multiline-test/applicatio` log group\. 

1. Choose the log and view the messages\. Lines that matched the rules in the parser file are concatenated and appear as a single message\. 

   The following log snippet shows a Go stack trace that is concatenated into a single event: 

   ```
   {
       "log": "panic: my panic\n\ngoroutine 4 [running]:\npanic(0x45cb40, 0x47ad70)\n  /usr/local/go/src/runtime/panic.go:542 +0x46c fp=0xc42003f7b8 sp=0xc42003f710 pc=0x422f7c\nmain.main.func1(0xc420024120)\n  foo.go:6 +0x39 fp=0xc42003f7d8 sp=0xc42003f7b8 pc=0x451339\nruntime.goexit()\n  /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003f7e0 sp=0xc42003f7d8 pc=0x44b4d1\ncreated by main.main\n  foo.go:5 +0x58\n\ngoroutine 1 [chan receive]:\nruntime.gopark(0x4739b8, 0xc420024178, 0x46fcd7, 0xc, 0xc420028e17, 0x3)\n  /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc420053e30 sp=0xc420053e00 pc=0x42503c\nruntime.goparkunlock(0xc420024178, 0x46fcd7, 0xc, 0x1000f010040c217, 0x3)\n  /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc420053e70 sp=0xc420053e30 pc=0x42512e\nruntime.chanrecv(0xc420024120, 0x0, 0xc420053f01, 0x4512d8)\n  /usr/local/go/src/runtime/chan.go:506 +0x304 fp=0xc420053f20 sp=0xc420053e70 pc=0x4046b4\nruntime.chanrecv1(0xc420024120, 0x0)\n  /usr/local/go/src/runtime/chan.go:388 +0x2b fp=0xc420053f50 sp=0xc420053f20 pc=0x40439b\nmain.main()\n  foo.go:9 +0x6f fp=0xc420053f80 sp=0xc420053f50 pc=0x4512ef\nruntime.main()\n  /usr/local/go/src/runtime/proc.go:185 +0x20d fp=0xc420053fe0 sp=0xc420053f80 pc=0x424bad\nruntime.goexit()\n  /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc420053fe8 sp=0xc420053fe0 pc=0x44b4d1\n\ngoroutine 2 [force gc (idle)]:\nruntime.gopark(0x4739b8, 0x4ad720, 0x47001e, 0xf, 0x14, 0x1)\n  /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc42003e768 sp=0xc42003e738 pc=0x42503c\nruntime.goparkunlock(0x4ad720, 0x47001e, 0xf, 0xc420000114, 0x1)\n  /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc42003e7a8 sp=0xc42003e768 pc=0x42512e\nruntime.forcegchelper()\n  /usr/local/go/src/runtime/proc.go:238 +0xcc fp=0xc42003e7e0 sp=0xc42003e7a8 pc=0x424e5c\nruntime.goexit()\n  /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003e7e8 sp=0xc42003e7e0 pc=0x44b4d1\ncreated by runtime.init.4\n  /usr/local/go/src/runtime/proc.go:227 +0x35\n\ngoroutine 3 [GC sweep wait]:\nruntime.gopark(0x4739b8, 0x4ad7e0, 0x46fdd2, 0xd, 0x419914, 0x1)\n  /usr/local/go/src/runtime/proc.go:280 +0x12c fp=0xc42003ef60 sp=0xc42003ef30 pc=0x42503c\nruntime.goparkunlock(0x4ad7e0, 0x46fdd2, 0xd, 0x14, 0x1)\n  /usr/local/go/src/runtime/proc.go:286 +0x5e fp=0xc42003efa0 sp=0xc42003ef60 pc=0x42512e\nruntime.bgsweep(0xc42001e150)\n  /usr/local/go/src/runtime/mgcsweep.go:52 +0xa3 fp=0xc42003efd8 sp=0xc42003efa0 pc=0x419973\nruntime.goexit()\n  /usr/local/go/src/runtime/asm_amd64.s:2337 +0x1 fp=0xc42003efe0 sp=0xc42003efd8 pc=0x44b4d1\ncreated by runtime.gcenable\n  /usr/local/go/src/runtime/mgc.go:216 +0x58",
       "container_id": "xxxxxx-xxxxxx",
       "container_name": "app",
       "source": "stdout",
       "ecs_cluster": "default",
       "ecs_task_arn": "arn:aws:ecs:us-east-1:xxxxxxxxxxxx:task/default/xxxxxx",
       "ecs_task_definition": "firelens-example-multiline:2"
   }
   ```

   The following log snippet shows how the same event appears if you run an ECS container that is not configured to concatenate multiline log messages\. The log field contains a single line\.

   ```
   {
       "log": "panic: my panic",
       "container_id": "xxxxxx-xxxxxx",
       "container_name": "app",
       "source": "stdout",
       "ecs_cluster": "default",
       "ecs_task_arn": "arn:aws:ecs:us-east-1:xxxxxxxxxxxx:task/default/xxxxxx",
       "ecs_task_definition": "firelens-example-multiline:3"
   ```

**Note**  
If your logs go to log files instead of the standard output, we recommend specifying the `multiline.parser` and `multiline.key_content` configuration parameters in the [Tail input plugin](https://docs.fluentbit.io/manual/pipeline/inputs/tail#multiline-support) instead of the Filter\.