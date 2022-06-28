# FSx for Windows File Server volumes<a name="wfsx-volumes"></a>

FSx for Windows File Server provides fully managed Microsoft Windows file servers, that are backed by a Windows file system\. When using FSx for Windows File Server together with ECS, you can provision your Windows tasks with persistent, distributed, shared, static file storage\. For more information, see [What Is FSx for Windows File Server?](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html)\.

**Note**  
EC2 instances that use the Amazon ECS\-Optimized Windows Server 2016 Full AMI do not support FSx for Windows File Server ECS task volumes\.  
You cannot use FSx for Windows File Server volumes in a Windows containers on Fargate configuration\.

You can use FSx for Windows File Server to deploy Windows workloads that require access to shared external storage, highly\-available Regional storage, or high\-throughput storage\. You can mount one or more FSx for Windows File Server file system volumes to an ECS container that runs on an ECS Windows instance\. You can share FSx for Windows File Server file system volumes between multiple ECS containers within a single ECS task\.

To enable the use of FSx for Windows File Server with ECS, include the FSx for Windows File Server file system ID and the related information in a task definition\. This is in the following example task definition JSON snippet\. Before you create and run a task definition, you need the following\.
+ An ECS Windows EC2 instance that's joined to a valid domain\. It can be hosted by an [AWS Directory Service for Microsoft Active Directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html), On\-premises Active Directory or self\-hosted Active Directory on Amazon EC2\.
+ An AWS Secrets Manager secret or Systems Manager parameter that contains the credentials that are used to domain join the Active Directory and attach the FSx for Windows File Server file system\. The credential values are the name and password credentials that you entered when creating the Active Directory\.

The following sections describe how to get started using FSx for Windows File Server with Amazon ECS\.

For a related tutorial, see [Tutorial: Using FSx for Windows File Server file systems with Amazon ECS](tutorial-wfsx-volumes.md)\.

## FSx for Windows File Server volume considerations<a name="wfsx-volume-considerations"></a>

Consider the following when using FSx for Windows File Server volumes:
+ FSx for Windows File Server with Amazon ECS only supports Windows Amazon EC2 instances\. Linux Amazon EC2 instances aren't supported\.
+ Amazon ECS only supports FSx for Windows File Server\. Amazon ECS doesn't support FSx for Lustre\.
+ FSx for Windows File Server with Amazon ECS doesn't support AWS Fargate\.
+ FSx for Windows File Server with Amazon ECS with `awsvpc` network mode requires version `1.54.0` or later of the container agent\.
+ The maximum number of drive letters that can be used for an Amazon ECS task is 23\. Each task with an FSx for Windows File Server volume gets a drive letter assigned to it\.
+ By default, task resource cleanup time is three hours after the task ended\. Even if no tasks use it, a file mapping that's created by a task persists for three hours\. The default cleanup time can be configured by using the Amazon ECS environment variable `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION`\. For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.
+ Tasks typically only run in the same VPC as the FSx for Windows File Server file system\. However, it's possible to have cross\-VPC support if there's an established network connectivity between the Amazon ECS cluster VPC and the FSx for Windows File Server file\-system through VPC peering\.
+ You control access to an FSx for Windows File Server file system at the network level by configuring the VPC security groups\. Only tasks that are hosted on EC2 instances joined to the AD domain with correctly configured AD security groups can access the FSx for Windows File Server file\-share\. If the security groups are misconfigured, ECS fails to launch the task with the following error message: `unable to mount file system fs-id`\.” 
+ FSx for Windows File Server is integrated with AWS Identity and Access Management \(IAM\) to control the actions that your IAM users and groups can take on specific FSx for Windows File Server resources\. With client authorization, customers can define IAM roles that allow or deny access to specific FSx for Windows File Server file systems, optionally require read\-only access, and optionally allow or disallow root access to the file system from the client\. For more information, see [Security](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/security.html) in the Amazon FSx Windows User Guide\.

## Specifying an FSx for Windows File Server file system in your task definition<a name="specify-wfsx-config"></a>

To use FSx for Windows File Server file system volumes for your containers, specify the volume and mount point configurations in your task definition\. The following task definition JSON snippet shows the syntax for the `volumes` and `mountPoints` objects for a container\.

```
{
    "containerDefinitions": [
        {
            "entryPoint": [
                "powershell",
                "-Command"
            ],
            "portMappings": [],
            "command": ["New-Item -Path C:\\fsx-windows-dir\\index.html -ItemType file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>It Works!</h2> <p>You are using Amazon FSx for Windows File Server file system for persistent container storage.</p>' -Force"],
            "cpu": 512,
            "memory": 256,
            "image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
            "essential": false,
            "name": "container1",
            "mountPoints": [
                {
                    "sourceVolume": "fsx-windows-dir",
                    "containerPath": "C:\\fsx-windows-dir",
                    "readOnly": false
                }
            ]
        },
        {
            "entryPoint": [
                "powershell",
                "-Command"
            ],
            "portMappings": [
                {
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "containerPort": 80
                }
            ],
            "command": ["Remove-Item -Recurse C:\\inetpub\\wwwroot\\* -Force; Start-Sleep -Seconds 120; Move-Item -Path C:\\fsx-windows-dir\\index.html -Destination C:\\inetpub\\wwwroot\\index.html -Force; C:\\ServiceMonitor.exe w3svc"],
            "mountPoints": [
                {
                    "sourceVolume": "fsx-windows-dir",
                    "containerPath": "C:\\fsx-windows-dir",
                    "readOnly": false
                }
            ],
            "cpu": 512,
            "memory": 256,
            "image": "mcr.microsoft.com/windows/servercore/iis:windowsservercore-ltsc2019",
            "essential": true,
            "name": "container2"
        }
    ],
    "family": "fsx-windows",
    "executionRoleArn": "arn:aws:iam::111122223333:role/ecsTaskExecutionRole",
    "volumes": [
        {
            "name": "fsx-windows-vol",
            "fsxWindowsFileServerVolumeConfiguration": {
                "fileSystemId": "fs-0eeb5730b2EXAMPLE",
                "authorizationConfig": {
                    "domain": "example.com",
                    "credentialsParameter": "arn:arn-1234"
                },
                "rootDirectory": "share"
            }
        }
    ]
}
```

`FSxWindowsFileServerVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when you're using [FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) file system for task storage\.    
`fileSystemId`  
Type: String  
Required: Yes  
The FSx for Windows File Server file system ID to use\.  
`rootDirectory`  
Type: String  
Required: Yes  
The directory within the FSx for Windows File Server file system to mount as the root directory inside the host\.  
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
A fully qualified domain name that's hosted by an [AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html) Managed Microsoft AD \(Active Directory\) or self\-hosted EC2 AD\.

### Credential storage methods<a name="creds"></a>

There are two different methods of storing credentials for use with the credentials parameter\.
+ **AWS Secrets Manager secret**

  This credential can be created in the AWS Secrets Manager console by using the *Other type of secret* category\. You add a row for each key/value pair, username/admin and password/*password*\.
+ **Systems Manager parameter**

  This credential can be created in the Systems Manager parameter console by entering text in the form that's in the following example code snippet\.

  ```
  {
    "username": "admin",
    "password": "password"
  }
  ```

The `credentialsParameter` in the task definition `FSxWindowsFileServerVolumeConfiguration` parameter holds either the secret ARN or the Systems Manager parameter ARN\. For more information, see [What is AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *Secrets Manager User Guide* and [Systems Manager Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) from the *Systems Manager User Guide*\.