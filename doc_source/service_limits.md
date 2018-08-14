# Amazon ECS Service Limits<a name="service_limits"></a>

The following table provides the default limits for Amazon ECS for an AWS account which can be changed\. For more information on the service limits for other AWS services that you can use with Amazon ECS, such as Elastic Load Balancing and Auto Scaling, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.


| Resource | Default Limit | 
| --- | --- | 
| Number of clusters per region, per account | 1000 | 
| Number of container instances per cluster | 1000 | 
| Number of services per cluster | 500 | 
| Number of tasks using the EC2 launch type per service \(the desired count\) | 1000 | 
| Number of tasks using the Fargate launch type, per region, per account | 20 | 
| Number of public IP addresses for tasks using the Fargate launch type | 20 | 

The following table provides other limitations for Amazon ECS that cannot be changed\.


| Resource | Limit | 
| --- | --- | 
| Number of load balancers per service | 1 | 
| Number of tasks launched \(count\) per run\-task | 10 | 
| Number of container instances per start\-task | 10 | 
| Throttle on container instance registration rate | 1 per second / 60 max per minute | 
| Task definition size limit | 32 KiB | 
| Task definition max containers | 10 | 
| Throttle on task definition registration rate | 1 per second / 60 max per minute | 
| Number of subnets specified in awsvpcConfiguration | 16 | 
| Number of security groups specified in awsvpcConfiguration | 5 | 
| Maximum layer size of an image used by a task using the Fargate launch type | 4 GB | 
| Maximum size of a shared volume used by multiple containers within a task using the Fargate launch type | 4 GB | 
| Maximum container storage for tasks using the Fargate launch type | 10 GB | 

The following table provides the supported task CPU and memory values for Fargate tasks, that cannot be changed\.


| CPU value | Memory value \(MiB\) | 
| --- | --- | 
| 256 \(\.25 vCPU\) | 512 \(0\.5GB\), 1024 \(1GB\), 2048 \(2GB\) | 
| 512 \(\.5 vCPU\) | 1024 \(1GB\), 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\) | 
| 1024 \(1 vCPU\) | 2048 \(2GB\), 3072 \(3GB\), 4096 \(4GB\), 5120 \(5GB\), 6144 \(6GB\), 7168 \(7GB\), 8192 \(8GB\) | 
| 2048 \(2 vCPU\) | Between 4096 \(4GB\) and 16384 \(16GB\) in increments of 1024 \(1GB\) | 
| 4096 \(4 vCPU\) | Between 8192 \(8GB\) and 30720 \(30GB\) in increments of 1024 \(1GB\) | 