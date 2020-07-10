# Rolling update<a name="deployment-type-ecs"></a>

The *rolling update* deployment type is controlled by Amazon ECS\. This involves the service scheduler replacing the current running version of the container with the latest version\. The number of tasks that Amazon ECS adds or removes from the service during a rolling update is controlled by the deployment configuration\. A deployment configuration consists of the minimum and maximum number of tasks allowed during a service deployment\.

To create a new Amazon ECS service that uses the rolling update deployment type, see [Creating a service](create-service.md)\.