# AWS Fargate Platform Versions<a name="platform_versions"></a>

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure\. It is a combination of the kernel and container runtime versions\. 

New platform versions are released as the runtime environment evolves, for example, if there are kernel or operating system updates, new features, bug fixes, or security updates\. Security updates and patches are deployed automatically for your Fargate tasks\.

**Topics**
+ [Platform Version Considerations](#platform-version-considerations)
+ [Available AWS Fargate Platform Versions](#available_pv)

## Platform Version Considerations<a name="platform-version-considerations"></a>

The following should be considered when specifying a platform version:
+ When specifying a platform version, you can use either the version number \(for example, `1.1.0`\) or `LATEST`\.
+ To use a specific platform version, specify the version number when creating or updating your service\. If you specify `LATEST`, your tasks use the most current platform version available, which may not be the most recent platform version\.
+ If you have a service with running tasks and want to update their platform version, you can update your service, specify a new platform version, and choose **Force new deployment**\. Your tasks are redeployed with the new platform version\. For more information, see [Updating a Service](update-service.md)\.
+ If your service is scaled up without updating the platform version, those tasks receive the platform version that was specified on the service's current deployment\.

## Available AWS Fargate Platform Versions<a name="available_pv"></a>

The following is a list of the platform versions currently available:

Fargate Platform Version‐1\.1\.0  
+ Added support for the Amazon ECS task metadata endpoint\. For more information, see [Amazon ECS Task Metadata Endpoint](task-metadata-endpoint.md)\.
+ Added support for Docker health checks in container definitions\. For more information, see [Health Check](task_definition_parameters.md#container_definition_healthcheck)\.
+ Added support for Amazon ECS service discovery\. For more information, see [Service Discovery](service-discovery.md)\.

Fargate Platform Version‐1\.0\.0  
+ Based on Amazon Linux 2017\.09\.
+ Initial release\.