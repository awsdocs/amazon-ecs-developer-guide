# Windows platform versions<a name="platform-windows-fargate"></a>

## Platform version considerations<a name="platform-windows-version-considerations"></a>

Consider the following when specifying a platform version:
+ When specifying a platform version, you can use either a specific version number, for example `1.0.0`, or `LATEST`\.

  When the **LATEST** platform version is selected the `1.0.0` platform is used\.
+ New tasks always run on the latest revision of a platform version, ensuring that tasks are always started on secured and patched infrastructure\.
+ Microsoft Windows Server container images must be created from a specific version of Windows Server\. You must select the same version of Windows Server in the `platformFamily` when you run a task or create a service that matches the Windows Server container image\. Additionally, you can provide a matching `operatingSystemFamily` in the task definition to prevent tasks from being run on the wrong Windows version\. For more information, see [Matching container host version with container image versions](https://learn.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/version-compatibility#matching-container-host-version-with-container-image-versions) on the Microsoft Learn website\.
+ The platform version numbers for Linux containers and Windows containers on Fargate are independent\. For example, the behavior, features, and software used in platform version `1.0.0` for Windows containers on Fargate aren't comparable to those of platform version `1.0.0` for Linux containers on Fargate\.

The following are the available platform versions for Windows containers\.

## 1\.0\.0<a name="platform-version-w1-0"></a>

The following is the changelog for platform version `1.0.0`\.
+ Initial release for support on the following Microsoft Windows Server operating systems:
  + Windows Server 2019 Full
  + Windows Server 2019 Core
  + Windows Server 2022 Full
  + Windows Server 2022 Core