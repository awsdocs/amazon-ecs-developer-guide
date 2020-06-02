# Amazon ECS Service Quotas<a name="service-quotas"></a>

The following table provides the default service quotas, also referred to as limits, for Amazon ECS for an AWS account which can be changed\. For more information on the service limits for other AWS services that you can use with Amazon ECS, such as Elastic Load Balancing and Auto Scaling, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.

To request a quota increase, see [Requesting a Quota Increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-increase.html) in the *Service Quotas User Guide*\.


****  

| Service quota | Description | Default quota value | 
| --- | --- | --- | 
|  Clusters per account  |  The maximum number of clusters per Region, per account\.  |  10,000  | 
|  Container instances per cluster  |  The maximum number of container instances per cluster\.  |  2,000  | 
|  Services per cluster  |  The maximum number of services per cluster\.  |  1,000  | 
|  Tasks using the EC2 launch type per service \(the desired count\)  |  The maximum number of tasks using the EC2 launch type per service \(the desired count\)\. This limit applies to both standalone tasks and tasks launched as part of a service\.  |  1,000  | 
|  Tasks using the Fargate launch type or the `FARGATE` capacity provider, per Region, per account  |  The maximum number of tasks using the Fargate launch type or the `FARGATE` capacity provider, per Region\. This limit applies to both standalone tasks and tasks launched as part of a service\.  |  100  | 
|  Fargate Spot tasks, per Region, per account  |  The maximum number of tasks using the `FARGATE_SPOT` capacity provider, per Region\.  |  250  | 
|  Public IP addresses for tasks using the Fargate launch type  | The maximum number of public IP addresses used by tasks using the Fargate launch type, per Region\. |  100  | 

The following table provides other limitations for Amazon ECS that cannot be changed\.

**Note**  
The quotas for Fargate task storage are dependent on the platform version used by the task\. For more information, see [Fargate Task Storage](fargate-task-storage.md)\.


|  Service quota  |  Description  |  Default quota value  | 
| --- | --- | --- | 
|  Classic Load Balancers per service  |  The maximum number of Classic Load Balancers per service\.  |  1  | 
|  Tasks launched \(`count`\) per run\-task  |  The maximum number of tasks that can be launched per `RunTask` API action\.  |  10  | 
|  Container instances per start\-task  |  The maximum number of container instances specified in a `StartTask` API action\.  |  10  | 
|  Revisions per task definition family  |  The maximum number of revisions per task definition family\. Deregistering a task definition revision does not exclude it from being included in this limit\.  |  1,000,000  | 
|  Task definition size limit  |  The maximum size, in KiB, of a task definition\.  |  32  | 
|  Task definition max containers  |  The maximum number of containers definitions within a a task definition\.  |  10  | 
|  Subnets specified in an `awsvpcConfiguration`  |  The maximum number of subnets specified within an `awsvpcConfiguration`\.  |  16  | 
|  Security groups specified in an `awsvpcConfiguration`  |  The maximum number of security groups specified within an `awsvpcConfiguration`\.  |  5  | 
|  Tags per resource  |  The maximum number of tags per resource\. This applies to tasks, services, task definitions, clusters, and container instances\.  |  50  | 

## Using Service Quotas<a name="using-service-quotas"></a>

Amazon Elastic Container Service \(Amazon ECS\) has integrated with Service Quotas, an AWS service that enables you to view and manage your quotas from a central location\. Service quotas are also referred to as limits\. For more information, see [What Is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) in the *Service Quotas User Guide*\.

Service Quotas makes it easy to look up the value of all of the Amazon ECS service quotas\.

**To view Amazon ECS service quotas \(AWS Management Console\)**

1. Open the Service Quotas console at [https://console\.aws\.amazon\.com/servicequotas/](https://console.aws.amazon.com/servicequotas/)\.

1. In the navigation pane, choose **AWS services**\.

1. From the **AWS services** list, search for and select **Amazon Elastic Container Service \(Amazon ECS\)**\.

   In the **Service quotas** list, you can see the service quota name, applied value \(if it is available\), AWS default quota, and whether the quota value is adjustable\.

1. To view additional information about a service quota, such as the description, choose the quota name\.

To request a quota increase, see [Requesting a Quota Increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-increase.html) in the *Service Quotas User Guide*\.