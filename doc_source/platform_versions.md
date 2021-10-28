# AWS Fargate platform versions<a name="platform_versions"></a>

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure\. It is a combination of the kernel and container runtime versions\. 

New platform versions are released as the runtime environment evolves, for example, if there are kernel or operating system updates, new features, bug fixes, or security updates\. Security updates and patches are deployed automatically for your Fargate tasks\. If a security issue is found that affects a platform version, AWS patches the platform version\. In some cases, you may be notified that your Fargate tasks have been scheduled for retirement\. For more information, [Task maintenance](https://docs.aws.amazon.com/AmazonECS/latest/userguide/task-maintenance.html) in the *Amazon Elastic Container Service User Guide for AWS Fargate*\.

**Topics**
+ [Linux platform versions](platform-linux-fargate.md)
+ [Windows platform versions](platform-windows-fargate.md)