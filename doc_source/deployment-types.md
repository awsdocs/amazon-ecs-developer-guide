# Amazon ECS Deployment types<a name="deployment-types"></a>

An Amazon ECS deployment type determines the deployment strategy that your service uses\. There are three deployment types: rolling update, blue/green, and external\.

You can view information about the service deployment type on the service details page, or by using the `describe-services` API\. For more information, see [DescribeServices](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_DescribeServices.html) in the *Amazon Elastic Container Service API Reference*\.

**Topics**
+ [Rolling update](deployment-type-ecs.md)
+ [Blue/Green deployment with CodeDeploy](deployment-type-bluegreen.md)
+ [External deployment](deployment-type-external.md)