# Creating a task definition using the new console<a name="create-task-definition"></a>

Create your task definitions using the new Amazon ECS console experience\. To make the task definition creation process as easy as possible, the console has default selections for many choices which we describe below\. There are also help panels available for most of the sections in the console which provide further context\.

**To create a new task definition \(New Amazon ECS console\)**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**, **Create new task definition**\.

1. For **Task definition family**, specify a unique name for the task definition\.

1. For each container in your task definition, complete the following steps\.

   1. For **Name**, specify a name for the container\.

   1. For **Image URI**, specify the image to use to start a container\. Images in the Docker Hub registry are available by default\. You can also specify other repositories using either the `repository-url/image:tag` or `repository-url/image@digest` formats\.

   1. For **Container port** and **Protocol**, specify the port mapping to use for the container\. A port mapping allows the container to access ports on the host to send or receive traffic\.

   1. Choose **Add more port mappings** to specify additional container port mappings\.

   1. Expand the **Environment variables** section to specify environment variables to inject into the container\. You can specify environment variables either individually using key\-value pairs or in bulk by specifying an environment variable file hosted in an Amazon S3 bucket\.

   1. \(Optional\) Choose **Add more containers** to add additional containers to the task definition\. Choose **Next** once all containers have been defined\.

1. For **App environment**, choose **AWS Fargate \(serverless\)**\. Amazon ECS performs validation using this value to ensure the task definition parameters are valid for the infrastructure type\.

1. For **Task size**, specify the CPU and memory values to reserve for the task\. The CPU value is specified as vCPUs and memory is specified as GB\.

   For tasks hosted on Fargate, the following table shows the valid CPU and memory combinations\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)

   For tasks hosted on Amazon EC2, supported task CPU values are between 128 CPU units \(0\.125 vCPUs\) and 10240 CPU units \(10 vCPUs\)\.

1. \(Optional\) Expand the **Task roles, network mode** section to specify an IAM role to assign to the task\.

1. \(Optional\) The **Storage** section is used to expand the amount of ephemeral storage for tasks hosted on Fargate as well as the data volume configuration for the task\.

   1. For **Amount**, to expand the available ephemeral storage beyond the default value of 20 GiB for your Fargate tasks, specify a value up to 200 GiB\.

1. \(Optional\) Choose **Add volume** to add a data volume configuration for the task\. For each data volume, complete the following steps\.

   1. For **Volume type**, choose **Bind mount**\.

   1. For **Volume name**, specify a name for the data volume\. The data volume name is used when creating a container mount point in a later step\.

   1. Expand the **Container mount points** section and choose **Add**\.

   1. For **Container**, choose the container for the mount point\.

   1. For **Source volume**, choose the data volume to mount to the container\.

   1. For **Container path**, specify the path on the container to mount the volume\.

   1. For **Read only**, specify whether to make the volume read only\.

   1. Choose **Add** to add additional mount points until each data volume defined in the task definition has a mount point defined\.

1. \(Optional\) Select the **Use log collection** option to specify a log configuration\. For each available log driver, there are log driver options to specify\. The default option sends container logs to CloudWatch Logs\. The other log driver options are configured using AWS FireLens\. For more information, see [Custom log routing](using_firelens.md)\.

   The following describes each container log destination in more detail\.
   + **Amazon CloudWatch** — Configure the task to send container logs to CloudWatch Logs\. The default log driver options are provided which creates a CloudWatch log group on your behalf\. To specify a different log group name, change the driver option values\.
   + **Amazon Kinesis Data Firehose** — Configure the task to send container logs to Kinesis Data Firehose\. The default log driver options are provided which sends logs to an Kinesis Data Firehose delivery stream\. To specify a different delivery stream name, change the driver option values\.
   + **Amazon Kinesis Data Streams** — Configure the task to send container logs to Kinesis Data Streams\. The default log driver options are provided which sends logs to an Kinesis Data Streams stream\. To specify a different stream name, change the driver option values\.
   + **Amazon OpenSearch Service** — Configure the task to send container logs to an OpenSearch Service domain\. The log driver options must be provided\. For more information, see [Forwarding logs to an Amazon OpenSearch Service domain](firelens-example-taskdefs.md#firelens-example-opensearch)\.
   + **Amazon S3** — Configure the task to send container logs to an Amazon S3 bucket\. The default log driver options are provided but you must specify a valid Amazon S3 bucket name\.

1. \(Optional\) Select the **Use trace collection** option to configure your tasks to route trace data from your application to AWS X\-Ray\. When this option is selected, Amazon ECS creates an AWS Distro for OpenTelemetry container sidecar which is preconfigured to send the trace data\.
**Important**  
Your application must be setup to send trace data, otherwise no data is sent to AWS X\-Ray\. For more information, see [Instrumenting your application for AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-instrumenting-your-app.html) in the *AWS X\-Ray Developer Guide*\.

1. \(Optional\) Expand the **Tags** section to add tags, as key\-value pairs, to the task definition\.

1. Choose **Next** to review the task definition\.

1. On the **Review and create** page, review each task definition section\. Choose **Edit** to make changes\. Once the task definition is complete, choose **Create** to register the task definition\.