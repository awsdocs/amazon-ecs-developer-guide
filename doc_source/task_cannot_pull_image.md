# CannotPullContainer task errors<a name="task_cannot_pull_image"></a>

The following Docker errors indicate that when creating a task, the container image specified could not be retrieved\.

Connection timed out  
When a Fargate task is launched, its elastic network interface requires a route to the internet to pull container images\. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:  

```
CannotPullContainerError: API error (500): Get https://111122223333.dkr.ecr.us-east-1.amazonaws.com/v2/: net/http: request canceled while waiting for connection"
```
To resolve this issue, you can:  
+ For tasks in public subnets, specify **ENABLED** for **Auto\-assign public IP** when launching the task\. For more information, see [Running tasks](ecs_run_task.md)\.
+ For tasks in private subnets, specify **DISABLED** for **Auto\-assign public IP** when launching the task, and configure a NAT Gateway in your VPC to route requests to the internet\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. For more information about creating a VPC with public and private subnets, including a NAT gateway for the private subnets, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

Context canceled  
The common cause for this error is because the VPC your task is using does not have a route to pull the container image from Amazon ECR\.

Image not found  
When you specify an Amazon ECR image in your container definition, you must use the full URI of your ECR repository along with the image name in that repository\. If the repository or image cannot be found, you receive the following error:  

```
CannotPullContainerError: API error (404): repository 111122223333.dkr.ecr.us-east-1.amazonaws.com/<repo>/<image> not found
```
To resolve this issue, verify the repository URI and the image name\. Also ensure that you have set up the proper access using the task execution IAM role\. For more information about the task execution role, see [Amazon ECS task execution IAM role](task_execution_IAM_role.md)\.

Insufficient disk space  
If the root volume of your container instance has insufficient disk space when pulling the container image, you see an error similar to the following:  

```
CannotPullContainerError: write /var/lib/docker/tmp/GetImageBlob111111111: no space left on device
```
To resolve this issue, free up disk space\.  
If you are using the Amazon ECS\-optimized AMI, you can use the following command to retrieve the 20 largest files on your filesystem:  

```
du -Sh / | sort -rh | head -20
```
Example output:  

```
5.7G    /var/lib/docker/containers/50501b5f4cbf90b406e0ca60bf4e6d4ec8f773a6c1d2b451ed8e0195418ad0d2
1.2G    /var/log/ecs
594M    /var/lib/docker/devicemapper/mnt/c8e3010e36ce4c089bf286a623699f5233097ca126ebd5a700af023a5127633d/rootfs/data/logs
...
```
In some cases, like this example above, the root volume may be filled out by a running container\. If the container is using the default `json-file` log driver without a `max-size` limit, it may be that the log file is responsible for most of that space used\. You can use the `docker ps` command to verify which container is using the space by mapping the directory name from the output above to the container ID\. For example:  

```
CONTAINER ID   IMAGE                            COMMAND             CREATED             STATUS              PORTS                            NAMES
50501b5f4cbf   amazon/amazon-ecs-agent:latest   "/agent"            4 days ago          Up 4 days                                            ecs-agent
```
By default, when using the `json-file` log driver, Docker captures the standard output \(and standard error\) of all of your containers and writes them in files using the JSON format\. You are able to set the `max-size` as a log driver option, which prevents the log file from taking up too much space\. For more information, see [Configure logging drivers](https://docs.docker.com/config/containers/logging/json-file/) in the Docker documentation\.  
The following is a container definition snippet showing how to use this option:  

```
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "256m"
    }
}
```
An alternative if your container logs are taking up too much disk space is to use the `awslogs` log driver\. The `awslogs` log driver sends the logs to CloudWatch, which frees up the disk space that would otherwise be used for your container logs on the container instance\. For more information, see [Using the awslogs Log Driver](using_awslogs.md)\.