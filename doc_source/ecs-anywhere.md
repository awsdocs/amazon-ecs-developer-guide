# Amazon ECS Anywhere<a name="ecs-anywhere"></a>

Amazon ECS provides support for registering an on\-premise server or virtual machine \(VM\), referred to as an *external instance*, to your Amazon ECS cluster\. External instances are optimized for running applications that generate outbound traffic or process data\. If your application requires inbound traffic, such as a web service, the lack of Elastic Load Balancing support makes running these workloads less efficient\. Amazon ECS added a new `EXTERNAL` launch type to use when creating services or running tasks to run on your external instances\.

The following shows a high level system architecture overview of Amazon ECS Anywhere\.

![\[Diagram showing architecture of Amazon ECS Anywhere.\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/overview-ecsanywhere.png)

**Topics**
+ [Considerations](#ecs-anywhere-considerations)
+ [IAM permissions for Amazon ECS Anywhere](ecs-anywhere-iam.md)
+ [Registering an external instance to a cluster](ecs-anywhere-registration.md)
+ [Deregistering an external instance](ecs-anywhere-deregistration.md)
+ [Running workloads on external instances](ecs-anywhere-runtask.md)
+ [Updating the AWS Systems Manager Agent and Amazon ECS container agent on an external instance](ecs-anywhere-updates.md)

## Considerations<a name="ecs-anywhere-considerations"></a>

Before you begin using external instances, be aware of the following considerations\.
+ Registering external instances with Amazon ECS Anywhere is not supported in the China \(Beijing\) and China \(Ningxia\) Regions\.
+ An external instance can only be registered to one cluster at a time\. For steps on how to register an external instance with a different cluster, see [Deregistering an external instance](ecs-anywhere-deregistration.md)\.
+ Your external instances require an IAM role that permits them to communicate with AWS APIs\. For more information, see [Required IAM permissions for external instances](ecs-anywhere-iam.md#ecs-anywhere-iam-required)\.
+ To send container logs to CloudWatch Logs, ensure you create and specify a task execution IAM role in your task definition\. For more information, see [Conditional IAM permissions](ecs-anywhere-iam.md#ecs-anywhere-iam-conditional)\.
+ When an external instance is registered to a cluster, the `ecs.capability.external` attribute is associated with the instance\. This attribute identifies the instance as an external instance, so when you run a task or create a service and specify the `EXTERNAL` launch type, the scheduler knows to run the tasks on that instance\.
+ Custom attributes can be added to your external instances to use as task placement constraints\. For more information, see [Custom attributes](task-placement-constraints.md#ecs-custom-attributes)\.
+ Adding resource tags to your external instance is supported\. For more information, see [Adding tags to an external container instance](ecs-using-tags.md#instance-details-tags-external)\.
+ The following are considerations specific to networking with your External instances\. For more information, see [Networking with ECS Anywhere](#ecs-anywhere-networking)\.
  + Service load balancing isn't supported\.
  + Service discovery isn't supported\.
  + Tasks run on external instances must use either the `bridge`, `host`, or `none` network modes\. The `awsvpc` network mode isn't supported\.
  + There are Amazon ECS service domains in each AWS Region that must be allowed to send traffic to your external instances\.
  + The SSM Agent installed on your external instance maintains IAM credentials that are rotated every 30 minutes using a hardware fingerprint\. If your external instance loses connection to AWS, the SSM Agent will automatically refresh the credentials once the connection is reestablished\. For more information, see [Validating on\-premises servers and virtual machines using a hardware fingerprint](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-technical-details.html#fingerprint-validation) in the *AWS Systems Manager User Guide*\.
+ The `UpdateContainerAgent` API isn't supported\. For more information on updating the SSM Agent or the Amazon ECS agent on your External instances, see [Updating the AWS Systems Manager Agent and Amazon ECS container agent on an external instance](ecs-anywhere-updates.md)\.
+ Amazon ECS capacity providers aren't supported\. To create a service or run a standalone task on your external instances, use the `EXTERNAL` launch type\.
+ ECS Exec isn't supported\.
+ SELinux isn't supported\.
+ Specifying GPU requirements in the task definition isn't supported\.
+ Integration with App Mesh isn't supported\.

### Supported operating systems and system architectures<a name="ecs-anywhere-supported-os"></a>

The following is the list of supported operating systems and system architectures\.
+ CentOS 7, CentOS 8
+ RHEL 7 — Neither Docker or RHEL's open package repositories support installing Docker natively on RHEL\. You must ensure that Docker is installed prior to running the install script described in this document\.
+ Fedora 32, Fedora 33 — Fedora 32 and Fedora 33 default to using `cgroups.v2` which is not supported by Amazon ECS\. As a result, the server's default grub configuration must be changed and the server rebooted\. For more information, see [Changing cgroup version](https://docs.docker.com/config/containers/runmetrics/#changing-cgroup-version) in the Docker documentation\.
+ openSUSE Tumbleweed
+ Ubuntu 18, Ubuntu 20
+ Debian 9, Debian 10
+ SUSE Enterprise Server 15
+ The `x86_64` and `ARM64` CPU architectures are supported\.

### Networking with ECS Anywhere<a name="ecs-anywhere-networking"></a>

Amazon ECS external instances are optimized for running applications that generate outbound traffic or process data\. If your application requires inbound traffic, such as a web service, the lack of Elastic Load Balancing support makes running these workloads less efficient because there isn't support for placing these workloads behind a load balancer\.

The tasks you run on your external instances must use either the `bridge`, `host`, or `none` network modes\. The `awsvpc` network mode isn't supported\. For more details about each network mode, see [Choosing a network mode](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/networking-networkmode.html) in the *Amazon ECS Best Practices Guide*\.

The following domains are used for communication between the Amazon ECS service and the Amazon ECS agent installed on your external instance\. You should ensure traffic is allowed and that DNS resolution works\. For each endpoint, *region* represents the Region identifier for an AWS Region supported by Amazon ECS, such as `us-east-2` for the US East \(Ohio\) Region\. The endpoints for all Regions you use should be allowed\. For the `ecs-a` and `ecs-t` endpoints, you should include an asterisk, for example `ecs-a-*`\.
+ `ecs-a-*.region.amazonaws.com` — This endpoint is used when managing tasks\.
+ `ecs-t-*.region.amazonaws.com` — This endpoint is used to manage task and container metrics\.
+ `ecs.region.amazonaws.com` — This is the service endpoint for Amazon ECS\.
+ If your tasks require communication with any other AWS services, for example Amazon ECR to pull container images or CloudWatch for CloudWatch Logs, ensure that those service endpoints are allowed\. For more details, see [Service endpoints](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *AWS General Reference*\.