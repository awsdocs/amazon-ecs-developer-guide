# Cannot Pull Container Image Error<a name="task_cannot_pull_image"></a>

The following Docker errors indicate that when creating a task, the container image specified was not able to be retrieved\.

Connection timed out  
When a Fargate task is launched, its elastic network interface requires a route to the internet to pull container images\. If you receive an error similar to the following when launching a task, it is because a route to the internet does not exist:  

```
CannotPullContainerError: API error (500): Get https://111122223333.dkr.ecr.us-east-1.amazonaws.com/v2/: net/http: request canceled while waiting for connection"
```
To resolve this issue, you can:  
+ For tasks in public subnets, specify **ENABLED** for **Auto\-assign public IP** when launching the task\. For more information, see [Running Tasks](ecs_run_task.md)\.
+ For tasks in private subnets, specify **DISABLED** for **Auto\-assign public IP** when launching the task, and configure a NAT Gateway in your VPC to route requests to the internet\. For more information, see [NAT Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. For more information about creating a VPC with public and private subnets, including a NAT gateway for the private subnets, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Clusters](create-public-private-vpc.md)\.

Image not found  
When specifying an Amazon ECR image in your container definition you must use the full ARN or URI of your ECR repository along with the image name in that repository\. If the repository or image cannot be found you will receive the following error:  

```
CannotPullContainerError: API error (404): repository 111122223333.dkr.ecr.us-east-1.amazonaws.com/<repo>/<image> not found
```
To resolve this issue, verify the repository ARN or URI and the image name\. Also ensure you have set up the proper access using the task execution IAM role\. For more information about the task execution role, see [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)\.

Insufficient disk space

On [ECS-Optimized AMI version 2015.09.d and later](ecs-ami-storage-config.md), it launches with an 8-GiB volume for the operating system that is attached at /dev/xvda and mounted as the root of the file system. If your root volume of the container instance without having sufficient disk space when pulling container image, you will see the following error:

```
CannotPullContainerError: write /var/lib/docker/tmp/GetImageBlob111111111: no space left on device
```

To resolve this issue, you need to identify which file filled out the disk space, you can run the following command to retrieve last 20 large files on your filesystem:

```
du -Sh / | sort -rh | head -20
```

Output:
```
5.7G    /var/lib/docker/containers/50501b5f4cbf90b406e0ca60bf4e6d4ec8f773a6c1d2b451ed8e0195418ad0d2
1.2G    /var/log/ecs
594M    /var/lib/docker/devicemapper/mnt/c8e3010e36ce4c089bf286a623699f5233097ca126ebd5a700af023a5127633d/rootfs/data/logs
197M    /var/lib/docker/devicemapper/mnt/4da2a272d836fbe5bdfd79ed640a93e8120a1e08d83323cea91fd1dcd2d8a7cb/rootfs/data/logs
143M    /var/cache/ecs
...
41M     /var/lib/docker/devicemapper/mnt/4da2a272d836fbe5bdfd79ed640a93e8120a1e08d83323cea91fd1dcd2d8a7cb/rootfs/usr/bin

```

In some case, the root volume may be filled out by the running container if using default json-file log without size limit. If you can notice that the `/var/lib/docker/container/XXXX` folder has been use large amount of your disk space, you can use the docker ps command to see which container used the space by mapping the container ID:

```
docker ps
```

Output:
```
CONTAINER ID   IMAGE                            COMMAND             CREATED             STATUS              PORTS                            NAMES
50501b5f4cbf   amazon/amazon-ecs-agent:latest   "/agent"            4 days ago          Up 4 days                                            ecs-agent
```

By default, Docker captures the standard output (and standard error) of all your containers, and writes them in files using the JSON format. If you can see the disk space filled out by the Docker json logging file. You can set the limit of the json file by using the max-size option in daemon.json file. For more detail please see [JSON File logging driver](https://docs.docker.com/config/containers/logging/json-file/#usage) in the Docker documentation:

```
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "256m"
    }
}

```

If you are using the EC2 launch type to run your ECS tasks, you can also use [awslogs Log Driver](using_awslogs.md), which enables sending log information to CloudWatch Logs, and it prevents your container logs from taking up disk space on your container instances.
