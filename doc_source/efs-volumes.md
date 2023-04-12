# Amazon EFS volumes<a name="efs-volumes"></a>

Amazon Elastic File System \(Amazon EFS\) provides simple, scalable file storage for use with your Amazon ECS tasks\. With Amazon EFS, storage capacity is elastic\. It grows and shrinks automatically as you add and remove files\. Your applications can have the storage they need and when they need it\.

You can use Amazon EFS file systems with Amazon ECS to export file system data across your fleet of container instances\. That way, your tasks have access to the same persistent storage, no matter the instance on which they land\. Your task definitions must reference volume mounts on the container instance to use the file system\. The following sections describe how to get started using Amazon EFS with Amazon ECS\.

For a tutorial, see [Tutorial: Using Amazon EFS file systems with Amazon ECS using the classic console](tutorial-efs-volumes.md)\.

## Amazon EFS volume considerations<a name="efs-volume-considerations"></a>

 Consider the following when using Amazon EFS volumes:
+ For tasks that use the EC2 launch type, Amazon EFS file system support was added as a public preview with Amazon ECS\-optimized AMI version `20191212` with container agent version 1\.35\.0\. However, Amazon EFS file system support entered general availability with Amazon ECS\-optimized AMI version `20200319` with container agent version 1\.38\.0, which contained the Amazon EFS access point and IAM authorization features\. We recommend that you use Amazon ECS\-optimized AMI version `20200319` or later to use these features\. For more information, see [Amazon ECS\-optimized AMI versions](ecs-ami-versions.md)\.
**Note**  
If you create your own AMI, you must use container agent 1\.38\.0 or later, `ecs-init` version 1\.38\.0\-1 or later, and run the following commands on your Amazon EC2 instance to enable the Amazon ECS volume plugin\. The commands are dependent on whether you're using Amazon Linux 2 or Amazon Linux as your base image\.  
Amazon Linux 2  

  ```
  yum install amazon-efs-utils
  systemctl enable --now amazon-ecs-volume-plugin
  ```
Amazon Linux  

  ```
  yum install amazon-efs-utils
  sudo shutdown -r now
  ```
+ For tasks that are hosted on Fargate, Amazon EFS file systems are supported on platform version 1\.4\.0 or later \(Linux\)\. For more information, see [AWS Fargate platform versions](platform_versions.md)\.
+ When using Amazon EFS volumes for tasks that are hosted on Fargate, Fargate creates a supervisor container that's responsible for managing the Amazon EFS volume\. The supervisor container uses a small amount of the task's memory\. The supervisor container is visible when querying the task metadata version 4 endpoint\. Additionally, it is visible in CloudWatch Container Insights as the container name `aws-fargate-supervisor`\. For more information, see [Task metadata endpoint version 4](task-metadata-endpoint-v4.md)\.
+ Using Amazon EFS volumes or specifying an `EFSVolumeConfiguration` isn't supported on external instances\.
+ We recommended that you set the `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION` parameter in the agent configuration file to a value that is less than the default \(about 1 hour\)\. This change helps prevent EFS mount credential expiration and allows for cleanup of mounts that are not in use\.  For more information, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

## Using Amazon EFS access points<a name="efs-volume-accesspoints"></a>

Amazon EFS access points are application\-specific entry points into an EFS file system for managing application access to shared datasets\. For more information about Amazon EFS access points and how to control access to them, see [Working with Amazon EFS Access Points](https://docs.aws.amazon.com/efs/latest/ug/efs-access-points.html) in the *Amazon Elastic File System User Guide*\.

Access points can enforce a user identity, including the user's POSIX groups, for all file system requests that are made through the access point\. Access points can also enforce a different root directory for the file system\. This is so that clients can only access data in the specified directory or its subdirectories\.

**Note**  
When creating an EFS access point, specify a path on the file system to serve as the root directory\. When referencing the EFS file system with an access point ID in your Amazon ECS task definition, the root directory must either be omitted or set to `/`, which enforces the path set on the EFS access point\.

You can use an Amazon ECS task IAM role to enforce that specific applications use a specific access point\. By combining IAM policies with access points, you can provide secure access to specific datasets for your applications\. For more information about how to use task IAM roles, see [Task IAM role](task-iam-roles.md)\.

## Specifying an Amazon EFS file system in your task definition<a name="specify-efs-config"></a>

To use Amazon EFS file system volumes for your containers, you must specify the volume and mount point configurations in your task definition\. The following task definition JSON snippet shows the syntax for the `volumes` and `mountPoints` objects for a container\.

```
{
    "containerDefinitions": [
        {
            "name": "container-using-efs",
            "image": "amazonlinux:2",
            "entryPoint": [
                "sh",
                "-c"
            ],
            "command": [
                "ls -la /mount/efs"
            ],
            "mountPoints": [
                {
                    "sourceVolume": "myEfsVolume",
                    "containerPath": "/mount/efs",
                    "readOnly": true
                }
            ]
        }
    ],
    "volumes": [
        {
            "name": "myEfsVolume",
            "efsVolumeConfiguration": {
                "fileSystemId": "fs-1234",
                "rootDirectory": "/path/to/my/data",
                "transitEncryption": "ENABLED",
                "transitEncryptionPort": integer,
                "authorizationConfig": {
                    "accessPointId": "fsap-1234",
                    "iam": "ENABLED"
                }
            }
        }
    ]
}
```

`efsVolumeConfiguration`  
Type: Object  
Required: No  
This parameter is specified when using Amazon EFS volumes\.    
`fileSystemId`  
Type: String  
Required: Yes  
The Amazon EFS file system ID to use\.  
`rootDirectory`  
Type: String  
Required: No  
The directory within the Amazon EFS file system to mount as the root directory inside the host\. If this parameter is omitted, the root of the Amazon EFS volume is used\. Specifying `/` has the same effect as omitting this parameter\.  
If an EFS access point is specified in the `authorizationConfig`, the root directory parameter must either be omitted or set to `/`, which enforces the path set on the EFS access point\.  
`transitEncryption`  
Type: String  
Valid values: `ENABLED` \| `DISABLED`  
Required: No  
Specifies whether to enable encryption for Amazon EFS data in transit between the Amazon ECS host and the Amazon EFS server\. If Amazon EFS IAM authorization is used, transit encryption must be enabled\. If this parameter is omitted, the default value of `DISABLED` is used\. For more information, see [Encrypting Data in Transit](https://docs.aws.amazon.com/efs/latest/ug/encryption-in-transit.html) in the *Amazon Elastic File System User Guide*\.  
`transitEncryptionPort`  
Type: Integer  
Required: No  
The port to use when sending encrypted data between the Amazon ECS host and the Amazon EFS server\. If you don't specify a transit encryption port, it uses the port selection strategy that the Amazon EFS mount helper uses\. For more information, see [EFS Mount Helper](https://docs.aws.amazon.com/efs/latest/ug/efs-mount-helper.html) in the *Amazon Elastic File System User Guide*\.  
`authorizationConfig`  
Type: Object  
Required: No  
The authorization configuration details for the Amazon EFS file system\.    
`accessPointId`  
Type: String  
Required: No  
The access point ID to use\. If an access point is specified, the root directory value in the `efsVolumeConfiguration` must either be omitted or set to `/`, which enforces the path set on the EFS access point\. If an access point is used, transit encryption must be enabled in the `EFSVolumeConfiguration`\. For more information, see [Working with Amazon EFS Access Points](https://docs.aws.amazon.com/efs/latest/ug/efs-access-points.html) in the *Amazon Elastic File System User Guide*\.  
`iam`  
Type: String  
Valid values: `ENABLED` \| `DISABLED`  
Required: No  
 Specifies whether to use the Amazon ECS task IAM role defined in a task definition when mounting the Amazon EFS file system\. If enabled, transit encryption must be enabled in the `EFSVolumeConfiguration`\. If this parameter is omitted, the default value of `DISABLED` is used\. For more information, see [IAM Roles for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)\.