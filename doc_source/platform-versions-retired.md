# AWS Fargate platform version deprecation<a name="platform-versions-retired"></a>

This page lists platform versions that AWS Fargate has deprecated or have been scheduled for deprecation\. These platform versions remain available until the published deprecation date\.

A *force update date* is provided for each platform version scheduled for deprecation\. On the force update date, any service using the `LATEST` platform version that is pointed to a platform version that is scheduled for deprecation will be updated using the force new deployment option\. When the service is updated using the force new deployment option, all tasks running on a platform version scheduled for deprecation are stopped and new tasks are launched using the platform version that the `LATEST` tag points to at that time\. Standalone tasks or services with an explicit platform version set are not affected by the force update date\.

We recommend updating your services standalone tasks to use the most recent platform version\. For more information on migrating to the most recent platform version, see [Migrating to platform version 1\.4\.0](platform-linux-fargate.md#platform-version-migration)\.

Once a platform version reaches the *deprecation date*, the platform version will no longer be available for new tasks or services\. Any standalone tasks or services which explicitly use a deprecated platform version will continue using that platform version until the tasks are stopped\. After the deprecation date, a deprecated platform version will no longer receive any security updates or bug fixes\.


| Platform version | Force update date | Deprecation date | 
| --- | --- | --- | 
|  1\.0\.0  |  October 26, 2020  |  December 14, 2020  | 
|  1\.1\.0  |  October 26, 2020  |  December 14, 2020  | 
|  1\.2\.0  |  October 26, 2020  |  December 14, 2020  | 

For information about current platform versions, see [AWS Fargate platform versions](platform_versions.md)\.