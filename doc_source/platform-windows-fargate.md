# Windows platform versions<a name="platform-windows-fargate"></a>

## Platform version considerations<a name="platform-windows-version-considerations"></a>

Consider the following when specifying a platform version:
+ When specifying a platform version, you can use either a specific version number, for example `1.0.0`, or `LATEST`\.

  When the **LATEST** platform version is selected the `1.0.0` platform is used\.
+ If you have a service with running tasks and want to update their platform version, you can update your service, specify a new platform version, and choose **Force new deployment**\. Your tasks are redeployed with the latest platform version\. 
+ If your service is scaled up without updating the platform version, those tasks receive the platform version that was specified on the service's current deployment\.

The following are the available platform versions for Windows containers\.

## 1\.0\.0<a name="platform-version-w1-0"></a>

The following is the changelog for platform version `1.0.0`\.
+ Initial release for support on the following Microsoft Windows operating systems:
  + Windows Server 2019 Full
  + Windows Server 2019 Core
  + Windows Server 2022 Full
  + Windows Server 2022 Core