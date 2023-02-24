# AWS Fargate platform versions<a name="platform_versions"></a>

AWS Fargate platform versions are used to refer to a specific runtime environment for Fargate task infrastructure\. It is a combination of the kernel and container runtime versions\. You select a platform version when you run a task or when you create a service to maintain a number of identical tasks\.

New revisions of platform versions are released as the runtime environment evolves, for example, if there are kernel or operating system updates, new features, bug fixes, or security updates\. A Fargate platform version is updated by making a new platform version revision\. Each task runs on one platform version revision during its lifecycle\. If you want to use the latest platform version revision, then you must start a new task\. A new task that runs on Fargate always runs on the latest revision of a platform version, ensuring that tasks are always started on secure and patched infrastructure\.

If a security issue is found that affects an existing platform version, AWS creates a new patched revision of the platform version and retires tasks running on the vulnerable revision\. In some cases, you may be notified that your tasks on Fargate have been scheduled for retirement\. For more information, see [AWS Fargate task maintenance](task-maintenance.md)\.

**Topics**
+ [Linux platform versions](platform-linux-fargate.md)
+ [Windows platform versions](platform-windows-fargate.md)