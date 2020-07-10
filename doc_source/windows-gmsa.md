# Using gMSAs for Windows Containers<a name="windows-gmsa"></a>

Amazon ECS supports Active Directory authentication for Windows containers through a special kind of service account called a *group Managed Service Account* \(gMSA\)\.

Windows based network applications such as \.NET applications often use Active Directory to facilitate authentication and authorization management between users and services\. Developers commonly design their applications to integrate with Active Directory and run on domain\-joined servers for this purpose\. Because Windows containers cannot be domain\-joined, you must configure a Windows container to run with gMSA\.

A Windows container running with gMSA relies on its host Amazon EC2 instance to retrieve the gMSA credentials from the Active Directory domain controller and provide them to the container instance\. For more information, see [Create gMSAs for Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)\.

**Topics**
+ [Considerations](#windows-gmsa-considerations)
+ [Prerequisites](#windows-gmsa-prerequisites)
+ [Setting Up gMSA\-capable Windows Containers on Amazon ECS](#windows-gmsa-setup)

## Considerations<a name="windows-gmsa-considerations"></a>

The following should be considered when using gMSAs for Windows containers:
+ When using the Amazon ECS\-optimized Windows Server 2016 Full AMI for your container instances, the container hostname must be the same as the gMSA account name defined in the credential spec file\. To specify a hostname for a container, use the `hostname` container definition parameter\. For more information, see [Network Settings](task_definition_parameters.md#container_definition_network)\.

## Prerequisites<a name="windows-gmsa-prerequisites"></a>

The following are prerequisites for using the gMSA for Windows containers feature with Amazon ECS\.
+ An Active Directory that your Amazon ECS Windows container instances can join\. Amazon ECS supports the following:
  + AWS Directory Service, which is an AWS managed Active Directory hosted on Amazon EC2\. For more information, see [Getting Started with AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started.html) in the *AWS Directory Service Administration Guide*\.
  + On\-premises Active Directory, as long as the Amazon ECS Windows container instance can join the domain\. For more information, see [AWS Direct Connect](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-direct-connect-network-to-amazon.html)\.
+ An existing gMSA account in the Active Directory\. For more information, see [Create gMSAs for Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)\.
+ The Amazon ECS Windows container instance hosting the Amazon ECS task must be domain joined to the Active Directory and be a member of the Active Directory security group that has access to the gMSA account\.

## Setting Up gMSA\-capable Windows Containers on Amazon ECS<a name="windows-gmsa-setup"></a>

Amazon ECS uses a credential spec file that contains the gMSA metadata used to propagate the gMSA account context to the Windows container\. You can generate the credential spec file and reference it in the `dockerSecurityOptions` field in your task definition\. The credential spec file does not contain any secrets\.

The following is an example credential spec file:

```
{
    "CmsPlugins": [
        "ActiveDirectory"
    ],
    "DomainJoinConfig": {
        "Sid": "S-1-5-21-2554468230-2647958158-2204241789",
        "MachineAccountName": "WebApp01",
        "Guid": "8665abd4-e947-4dd0-9a51-f8254943c90b",
        "DnsTreeName": "example.com",
        "DnsName": "example.com",
        "NetBiosName": "example"
    },
    "ActiveDirectoryConfig": {
        "GroupManagedServiceAccounts": [
            {
                "Name": "WebApp01",
                "Scope": "example.com"
            }
        ]
    }
}
```

### Referencing a Credential Spec File in a Task Definition<a name="windows-gmsa-credentialspec"></a>

Amazon ECS supports the following ways to reference the credential spec file in the `dockerSecurityOptions` field of a task definition\.

**Topics**
+ [Amazon S3 Bucket](#gmsa-credspec-s3)
+ [SSM Parameter Store parameter](#gmsa-credspec-ssm)
+ [Local File](#gmsa-credspec-file)

#### Amazon S3 Bucket<a name="gmsa-credspec-s3"></a>

Add the credential spec to an Amazon S3 bucket and then reference the Amazon Resource Name \(ARN\) of the Amazon S3 bucket in the `dockerSecurityOptions` field of the task definition\.

```
{
    "family": "",
    "executionRoleArn": "",
    "containerDefinitions": [
        {
            "name": "",
            ...
            "dockerSecurityOptions": [
                "credentialspec:arn:aws:s3:::${BucketName}/${ObjectName}"
            ],
            ...
        }
    ],
    ...
}
```

You must also add the following permissions as an inline policy to the Amazon ECS task execution IAM role to give your tasks access to the Amazon S3 bucket\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor",
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Resource": [
                "arn:aws:s3:::{bucket_name}",
                "arn:aws:s3:::{bucket_name}/{object}"
            ]
        }
    ]
}
```

#### SSM Parameter Store parameter<a name="gmsa-credspec-ssm"></a>

Add the credential spec to an SSM Parameter Store parameter and then reference the Amazon Resource Name \(ARN\) of the SSM Parameter Store parameter in the `dockerSecurityOptions` field of the task definition\.

```
{
    "family": "",
    "executionRoleArn": "",
    "containerDefinitions": [
        {
            "name": "",
            ...
            "dockerSecurityOptions": [
                "credentialspec:arn:aws:ssm:region:111122223333:parameter/parameter_name"
            ],
            ...
        }
    ],
    ...
}
```

You must also add the following permissions as an inline policy to the Amazon ECS task execution IAM role to give your tasks access to the SSM Parameter Store parameter\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters"
            ],
            "Resource": [
                "arn:aws:ssm:region:111122223333:parameter/parameter_name"
            ]
        }
    ]
}
```

#### Local File<a name="gmsa-credspec-file"></a>

With the credential spec details in a local file, reference the file path in the `dockerSecurityOptions` field of the task definition\.

```
{
    "family": "",
    "executionRoleArn": "",
    "containerDefinitions": [
        {
            "name": "",
            ...
            "dockerSecurityOptions": [
                "credentialspec:file://CredentialSpecFile.json"
            ],
            ...
        }
    ],
    ...
}
```