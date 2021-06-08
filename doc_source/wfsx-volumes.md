# Amazon FSx for Windows File Server volumes<a name="wfsx-volumes"></a>

Amazon FSx for Windows File Server provides fully managed Microsoft Windows file servers, that are backed by a fully native Windows file system\. When using Amazon FSx for Windows File Server together with ECS, you can provision your Windows tasks with persistent, distributed, shared, static file storage\. For more information, see [What Is Amazon FSx for Windows File Server?](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html)\.

**Note**  
EC2 instances that use the Amazon ECS\-Optimized Windows Server 2016 Full AMI, do not support Amazon FSx for Windows File Server ECS task volumes\.

You can use Amazon FSx for Windows File Server to deploy Windows workloads that require access to shared external storage, highly\-available regional storage, or high\-throughput storage\. You can mount one or more Amazon FSx for Windows File Server file system volumes to an ECS container running on an ECS Windows instance\. You can share Amazon FSx for Windows File Server file system volumes between multiple ECS containers within a single ECS task\.

To enable the use of Amazon FSx for Windows File Server with ECS, you need to include the Amazon FSx for Windows File Server file system id and related information in a task definition as shown in the following example task definition JSON snippet\. Before creating and running a task definition, you need the following\.
+ An ECS Windows EC2 instance that is joined to a valid domain, hosted by an [AWS Directory Service for Microsoft Active Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html), On\-premises Active Directory or self\-hosted Active Directory on Amazon EC2\.
+ An AWS Secrets Manager secret or Systems Manager parameter that contains the credentials that are used to domain join the Active Directory and attach the Amazon FSx for Windows File Server file system\. The credential values are the name and password credentials that you entered when creating the Active Directory\.

The following sections are provided to help you get started using Amazon FSx for Windows File Server with Amazon ECS\.

For a tutorial, see [Tutorial: Using Amazon FSx for Windows File Server file systems with Amazon ECS](tutorial-wfsx-volumes.md)\.

## Amazon FSx for Windows File Server volume considerations<a name="wfsx-volume-considerations"></a>

Consider the following when using Amazon FSx for Windows File Server volumes\.
+ Amazon FSx for Windows File Server with Amazon ECS only supports [Windows Amazon ECS\-optimized AMI](ecs-optimized_AMI.md#ecs-optimized-ami-windows)\. Linux Amazon EC2 instances aren't supported\.
+ Amazon ECS only supports Amazon FSx for Windows File Server\. Amazon ECS doesn't support Amazon FSx for Lustre\.
+ Amazon FSx for Windows File Server with Amazon ECS doesn't support AWS Fargate\.
+ Amazon FSx for Windows File Server with Amazon ECS doesn't support the `awsvpc` network mode\.
+ The maximum number of drive letters that can be used for an Amazon ECS task is 23\. Each task with an Amazon FSx for Windows File Server volume gets a drive letter assigned to it\.
+ Task resource cleanup time is 3 hours by default\. A file mapping created by a task persists for 3 hours even if no tasks are using it\. The default cleanup time can be configured by using the Amazon ECS environment variable `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION`\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.
+ Tasks typically only run in the same VPC as the Amazon FSx for Windows File Server file system\. However, it is possible to have cross\-VPC support if there is an established network connectivity between the Amazon ECS cluster VPC and the Amazon FSx for Windows File Server file\-system through VPC peering\.
+ You control access to an Amazon FSx for Windows File Server file system at the network level by configuring the VPC security groups\. Only tasks hosted on EC2 instances joined to the AD domain with correctly configured AD security groups will be able to access the Amazon FSx for Windows File Server file\-share\. If the security groups are misconfigured, ECS will fail the Task launch with the following error message: `unable to mount file system fs-id`\.” 
+ Amazon FSx for Windows File Server is integrated with AWS Identity and Access Management \(IAM\) to control the actions that your IAM users and groups can take on specific Amazon FSx for Windows File Server resources\. With client authorization, customers can define IAM roles that allow or deny access to specific Amazon FSx for Windows File Server file systems, optionally require read\-only access, and optionally allow or disallow root access to the file system from the client\. For more information, see [Security](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/security.html) in the Amazon FSx Windows User Guide\.

## Specifying an Amazon FSx for Windows File Server file system in your task definition<a name="specify-wfsx-config"></a>

To use Amazon FSx for Windows File Server file system volumes for your containers, you must specify the volume and mount point configurations in your task definition\. The following task definition JSON snippet shows the syntax for the `volumes` and `mountPoints` objects for a container:

```
{
    "containerDefinitions": [
        {
            "name": "container-using-fsx",
            "image": "iis:2",
            "entryPoint": [
                "sh",
                "-c"
            ],
            "command": [
                "ls -la \\mount\\fsx"
            ],
            "mountPoints": [
                {
                    "sourceVolume": "myFsxVolume",
                    "containerPath": "\\mount\\fsx",
                    "readOnly": true
                }
            ]
        }
    ],
    "volumes": [
        {
            "name": "myFsxVolume",
            "FSxWindowsFileServerVolumeConfiguration": {
                "fileSystemId": "fs-1234",
                "rootDirectory": "\\path\\to\\my\\data",
                "credentialsParameter": "arn-1234",
                "domain": "corp.fullyqualified.com",
            }
        }
    ]
}
```

`FSxWindowsFileServerVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when you are using [Amazon FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) file system for task storage\.    
`fileSystemId`  
Type: String  
Required: Yes  
The Amazon FSx for Windows File Server file system ID to use\.  
`rootDirectory`  
Type: String  
Required: Yes  
The directory within the Amazon FSx for Windows File Server file system to mount as the root directory inside the host\.  
`authorizationConfig`    
`credentialsParameter`  
Type: String  
Required: Yes  
The authorization credential options:  
+ Amazon Resource Name \(ARN\) of an [Secrets Manager](https://docs.aws.amazon.com/secretsmanager) secret\.
+ Amazon Resource Name \(ARN\) of an [Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html) parameter\.  
`domain`  
Type: String  
Required: Yes  
A fully qualified domain name hosted by an [AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html) Managed Microsoft AD \(Active Directory\) or self\-hosted EC2 AD\.

### Credential storage methods<a name="creds"></a>

There are two different methods of storing credentials for use with the credentials parameter\.
+ **AWS Secrets Manager secret**

  This credential can be created in the AWS Secrets Manager console by using the *Other type of secret* category\. You add a row for each key/value pair, username/admin and password/*password*\.
+ **Systems Manager parameter**

  This credential can be created in the Systems Manager parameter console by entering text in the form shown in the following example code snippet\.

  ```
  {
    "username": "admin",
    "password": "password"
  }
  ```

The `credentialsParameter` in the task definition `FSxWindowsFileServerVolumeConfiguration` parameter will hold either the secret ARN or the Systems Manager parameter ARN\. For more information, see [What is AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *Secrets Manager User Guide* and [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) from the *Systems Manager User Guide*\.