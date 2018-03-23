# AWS Fargate Platform Versions<a name="platform_versions"></a>

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure\. It is a combination of the Kernel version and the container runtime version\. When specifying the `LATEST` platform version when running a task or creating a service, you will get the most current platform version available for your tasks\. When you scale up your service, those tasks will receive the platform version that was specified on the service's current deployment\.

New platform versions will be released as the runtime environment evolves, for example if there are kernel or operating system updates, new features, bug fixes, or security updates\.

The following is a list of the platform versions currently available:

## Available Platform Versions<a name="available_pv"></a>

Fargate Platform Version‐1\.0\.0  
+ Based on Amazon Linux 2017\.09
+ Initial release