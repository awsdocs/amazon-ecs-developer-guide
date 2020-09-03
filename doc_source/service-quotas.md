# Amazon ECS service quotas<a name="service-quotas"></a>

The following tables provide the default service quotas, also referred to as limits, for Amazon ECS for an AWS account\. For more information on the service quotas for other AWS services that you can use with Amazon ECS, such as Elastic Load Balancing and Auto Scaling, see [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.

## Amazon ECS service quotas<a name="service-quotas-ecs"></a>

The following are Amazon ECS service quotas\.

Most of these service quotas, but not all, are listed under the Amazon Elastic Container Service \(Amazon ECS\) namespace in the Service Quotas console\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.


****  

|  Service quota  |  Description  |  Default quota value  |  Adjustable  | 
| --- | --- | --- | --- | 
|  Clusters  |  The maximum number of clusters in this account in the current Region\.  |  10,000  |  Yes  | 
|  Container instances per cluster  |  The maximum number of container instances per cluster\.  |  2,000  |  Yes  | 
|  Services per cluster  |  The maximum number of services per cluster\.  |  2,000  |  Yes  | 
|  Tasks per service  |  The maximum number of tasks per service \(the desired count\)\.  |  2,000  |  Yes  | 
|  Tasks launched \(`count`\) per run\-task  |  The maximum number of tasks that can be launched per `RunTask` API action\.  |  10  |  No  | 
|  Container instances per start\-task  |  The maximum number of container instances specified in a `StartTask` API action\.  |  10  |  No  | 
|  Revisions per task definition family  |  The maximum number of revisions per task definition family\. Deregistering a task definition revision does not exclude it from being included in this limit\.  |  1,000,000  |  No  | 
|  Task definition size limit  |  The maximum size, in KiB, of a task definition\.  |  32  |  No  | 
|  Task definition max containers  |  The maximum number of containers definitions within a task definition\.  |  10  |  No  | 
|  Subnets specified in an `awsvpcConfiguration`  |  The maximum number of subnets specified within an `awsvpcConfiguration`\.  |  16  |  No  | 
|  Security groups specified in an `awsvpcConfiguration`  |  The maximum number of security groups specified within an `awsvpcConfiguration`\.  |  5  |  No  | 
|  Target groups per service  |  The maximum number of target groups per service, if using an Application Load Balancer or a Network Load Balancer\.  |  5  |  No  | 
|  Classic Load Balancers per service  |  The maximum number of Classic Load Balancers per service\.  |  1  |  No  | 
|  Tags per resource  |  The maximum number of tags per resource\. This applies to task definitions, clusters, tasks, and services\.  |  50  |  No  | 

## AWS Fargate service quotas<a name="service-quotas-fargate"></a>

The following are Amazon ECS on AWS Fargate service quotas\.

These service quotas are listed under the AWS Fargate namespace in the Service Quotas console\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.


****  

|  Service quota  |  Description  |  Default quota value  |  Adjustable  | 
| --- | --- | --- | --- | 
|  Fargate On\-Demand resource count  |  The maximum number of Amazon ECS tasks and Amazon EKS pods running concurrently on Fargate in this account in the current Region\.  |  100  | Yes | 
|  Fargate Spot resource count  |  The maximum number of Amazon ECS tasks running concurrently on Fargate Spot in this account in the current Region\.  |  250  | Yes | 

## Managing your Amazon ECS and AWS Fargate service quotas in the AWS Management Console<a name="service-quotas-manage"></a>

Amazon ECS has integrated with Service Quotas, an AWS service that enables you to view and manage your quotas from a central location\. For more information, see [What Is Service Quotas?](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) in the *Service Quotas User Guide*\.

Service Quotas makes it easy to look up the value of your Amazon ECS service quotas\.

------
#### [ AWS Management Console ]

**To view Amazon ECS and Fargate service quotas using the AWS Management Console**

1. Open the Service Quotas console at [https://console\.aws\.amazon\.com/servicequotas/](https://console.aws.amazon.com/servicequotas/)\.

1. In the navigation pane, choose **AWS services**\.

1. From the **AWS services** list, search for and select **Amazon Elastic Container Service \(Amazon ECS\)** or **AWS Fargate**\.

   In the **Service quotas** list, you can see the service quota name, applied value \(if it is available\), AWS default quota, and whether the quota value is adjustable\.

1. To view additional information about a service quota, such as the description, choose the quota name\.

1. \(Optional\) To request a quota increase, select the quota that you want to increase, select **Request quota increase**, enter or select the required information, and select **Request**\.

To work more with service quotas using the AWS Management Console see the [Service Quotas User Guide](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)\. To request a quota increase, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.

------
#### [ AWS CLI ]

**To view Amazon ECS and Fargate service quotas using the AWS CLI**  
Run the following command to view the default Amazon ECS quotas\.

```
aws service-quotas list-aws-default-service-quotas \
    --query 'Quotas[*].{Adjustable:Adjustable,Name:QuotaName,Value:Value,Code:QuotaCode}' \
    --service-code ecs \
    --output table
```

Run the following command to view the default Fargate quotas\.

```
aws service-quotas list-aws-default-service-quotas \
    --query 'Quotas[*].{Adjustable:Adjustable,Name:QuotaName,Value:Value,Code:QuotaCode}' \
    --service-code fargate \
    --output table
```

Run the following command to view your applied Fargate quotas\.

```
aws service-quotas list-service-quotas \
    --service-code fargate
```

**Note**  
Amazon ECS does not support applied quotas\.

To work more with service quotas using the AWS CLI, see the [Service Quotas AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/index.html#cli-aws-service-quotas)\. To request a quota increase, see the [https://docs.aws.amazon.com/cli/latest/reference/service-quotas/request-service-quota-increase.html](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/request-service-quota-increase.html) command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/service-quotas/index.html#cli-aws-service-quotas)\.

------