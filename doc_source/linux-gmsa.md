# Using gMSAs for Linux Containers<a name="linux-gmsa"></a>

Amazon ECS supports Active Directory authentication for Linux containers through a special kind of service account called a *group Managed Service Account* \(gMSA\)\.

Linux based network applications such as \.NET Core applications can use Active Directory to facilitate authentication and authorization management between users and services\. Developers can design their applications to integrate with Active Directory and run on domain\-joined servers for this purpose\. Because Linux containers cannot be domain\-joined, you must configure a Linux container to run with gMSA\.

A Linux container running with gMSA relies on the `credentials-fetcher` daemon running on its host Amazon EC2 instance to retrieve the gMSA credentials from the Active Directory domain controller and provide them to the container instance\. For more information about service accounts, see [Create gMSAs for Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)\.

**Note**  
This feature is not supported on Fargate\. For Linux on Fargate, you can follow the example [Using Windows Authentication with Linux Containers on Amazon ECS](http://aws.amazon.com/blogs/containers/using-windows-authentication-with-linux-containers-on-amazon-ecs/)\.

**Topics**
+ [Considerations](#linux-gmsa-considerations)
+ [Prerequisites](#linux-gmsa-prerequisites)
+ [Setting up gMSA\-capable Linux Containers on Amazon ECS](#linux-gmsa-setup)
+ [Credential specification file](#linux-gmsa-credentialspec)

## Considerations<a name="linux-gmsa-considerations"></a>

The following should be considered when using gMSAs for Linux containers:
+ You can use gMSAs for Windows containers and Linux containers if the containers run on EC2\. Fargate isn't supported\.
+ You might need a Windows computer that is joined to the domain to complete the prerequisites\. For example, you might need a Windows computer that is joined to the domain to create the gMSA in Active Directory with PowerShell\. This is because the RSAT Active Director PowerShell tools are only available for Windows\. For more information, see [Installing the Active Directory administration tools](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_install_ad_tools.html)\.

## Prerequisites<a name="linux-gmsa-prerequisites"></a>

The following are prerequisites for using the gMSA for Linux containers feature with Amazon ECS\.
+ An Active Directory domain with the resources that you want your containers to access\. Amazon ECS supports the following:
  + AWS Directory Service, which is an AWS managed Active Directory hosted on Amazon EC2\. For more information, see [Getting Started with AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started.html) in the *AWS Directory Service Administration Guide*\.
  + On\-premises Active Directory, as long as the Amazon ECS Linux container instance can join the domain\. For more information, see [AWS Direct Connect](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-direct-connect-network-to-amazon.html)\.
+ An existing gMSA account in the Active Directory\. For more information, see [Using gMSAs for Linux Containers](#linux-gmsa)\.
+ The Amazon ECS Linux container instance must have the `credentials-fetcher` daemon installed and running\. Additionally, the `credentials-fetcher` daemon must also have an initial set of credentials to authenticate with the Active Directory\. The `credentials-fetcher` daemon is only available for Amazon Linux 2023 and Fedora 36 and newer\. It isn't available for Amazon Linux 2\. For more information, see [aws/credentials\-fetcher](https://github.com/aws/credentials-fetcher) on GitHub\.
+ The Amazon ECS Linux container instance hosting the Amazon ECS task must have initial credentials for a service account that is a member of the Active Directory security group that has access to the gMSA account\. There are multiple options in [Decide if you want to join the instances to the domain, or use domainless gMSA\.](#linux-gmsa-initial-creds)\.
+ There are required IAM permissions for the methods that you choose for the initial credentials and for storing the credential specification\.
  + IAM permissions for AWS Secrets Manager are required on the Amazon EC2 instance role if you choose to use *domainless gMSA* for initial credentials\.
  + IAM permissions for Amazon EC2 Systems Manager Parameter Store are required on the task execution role if you choose to store the credential specification in SSM Parameter Store\.
  + IAM permissions for Amazon Simple Storage Service are required on the task execution role if you choose to store the credential specification in Amazon S3\.

## Setting up gMSA\-capable Linux Containers on Amazon ECS<a name="linux-gmsa-setup"></a>
<a name="linux-gmsa-setup-infra"></a>
**Prepare the infrastructure**  
The following steps are considerations and setup that are performed once\. After you have completed these steps, you can automate the creation of container instances to reuse this configuration\.

Decide how the initial credentials are provided and configure the EC2 user data in a reusable EC2 launch template to install the `credentials-fetcher` daemon\.

1. <a name="linux-gmsa-initial-creds"></a>

**Decide if you want to join the instances to the domain, or use domainless gMSA\.**
   + <a name="linux-gmsa-initial-join"></a>

**Join EC2 instances to the Active Directory domain**

     
     + <a name="linux-gmsa-initial-join-userdata"></a>

**Join the instances by user data**

       Add the steps to join the Active Directory domain to your EC2 user data in an EC2 launch template\. Multiple Amazon EC2 Auto Scaling groups can use the same launch template\.

       You can use these steps [Joining an Active Directory or FreeIPA domain](https://docs.fedoraproject.org/en-US/quick-docs/join-active-directory-freeipa/) in the Fedora Docs\.
   + <a name="linux-gmsa-initial-domainless"></a>

**Make an Active Directory user for domainless gMSA**

     The `credentials-fetcher` daemon has a feature called *domainless gMSA*\. This feature requires a domain, but the EC2 instance doesn't need to be joined to the domain\. This way, other applications on the instance can't authenticate to the domain using the host credentials\. Instead, you provide the name of a secret in AWS Secrets Manager in the ECS agent `ecs.config` file with a username and password to log in to the domain\.

     This feature is similar to the *gMSA support for non\-domain\-joined container hosts* feature\. For more information about the Windows feature, see [gMSA architecture and improvements](https://learn.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#gmsa-architecture-and-improvements) on the Microsoft Learn website\.

     1. Make a user in your Active Directory domain\. The user in Active Directory must have permission to access the gMSA service accounts that you use in the tasks\.

     1. Create a secret in AWS Secrets Manager, after you have made the user in Active Directory\. For more information, see [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html)\.

     1. Enter the username and password of the user into a JSON key called `username` with a value of the name of your user, and a JSON key called `password` with a value of the password for the user\.

        ```
        {"username":"username","password":"passw0rd"}
        ```

     1. Add configuration to the ECS agent `/etc/ecs/ecs.config` file with the name of the secret in Secrets Manager\. Set the variable `CREDENTIALS_FETCHER_SECRET_NAME_FOR_DOMAINLESS_GMSA` to the name of the secret\. For more information about the configuration variables in the ECS agent, see [Amazon ECS container agent configuration](ecs-agent-config.md)\.

     1. The *domainless gMSA* feature needs additional permissions in the Amazon EC2 instance role\. Follow the step [\(Optional\) domainless gMSA secret](#linux-gmsa-domainless-secret)\.

1. <a name="linux-gmsa-install"></a>

**Configure instances and install `credentials-fetcher` daemon**

   You can install the `credentials-fetcher` daemon with a user data script in your EC2 Launch Template\. The following examples demonstrate two types of user data, `cloud-config` YAML or bash script\. These examples are for Amazon Linux 2023 \(AL2023\)\. Replace `MyCluster` with the name of the Amazon ECS cluster that you want these instances to join\.
   + <a name="linux-gmsa-install-yaml"></a>

**`cloud-config` YAML**

     ```
     Content-Type: text/cloud-config
     package_reboot_if_required: true
     packages:
       # prerequisites
       - dotnet
       - realmd
       - oddjob
       - oddjob-mkhomedir
       - sssd
       - adcli
       - krb5-workstation
       - samba-common-tools
       # https://github.com/aws/credentials-fetcher gMSA credentials management for containers
       - credentials-fetcher
     write_files:
     # configure the ECS Agent to join your cluster.
     # replace MyCluster with the name of your cluster.
     - path: /etc/ecs/ecs.config
       owner: root:root
       permissions: '0644'
       content: |
         ECS_CLUSTER=MyCluster
         ECS_GMSA_SUPPORTED=true
     runcmd:
     # start the credentials-fetcher daemon and if it succeeded, make it start after every reboot
     - "systemctl start credentials-fetcher"
     - "systemctl is-active credentials-fetch && systemctl enable credentials-fetcher"
     ```
   + <a name="linux-gmsa-install-userdata"></a>

**bash script**

     If you are more comfortable with bash scripts and have multiple variables to write to `/etc/ecs/ecs.config`, use the following `heredoc` format\. This format writes everything between the lines beginning with cat and `EOF` to the configuration file\.

     ```
     #!/usr/bin/env bash
     set -euxo pipefail
     
     # prerequisites
     timeout 30 dnf install -y dotnet realmd oddjob oddjob-mkhomedir sssd adcli krb5-workstation samba-common-tools
     # install https://github.com/aws/credentials-fetcher gMSA credentials management for containers
     timeout 30 dnf install -y credentials-fetcher
     
     # start credentials-fetcher
     systemctl start credentials-fetcher
     systemctl is-active credentials-fetch && systemctl enable credentials-fetcher
     
     cat <<'EOF' >> /etc/ecs/ecs.config
     ECS_CLUSTER=MyCluster
     ECS_GMSA_SUPPORTED=true
     EOF
     ```

   There are optional configuration variables for the `credentials-fetcher` daemon that you can set in `/etc/ecs/ecs.config`\. We recommend that you set the variables in the user data in the YAML block or `heredoc` like the examples above to prevent issues with partial configuration that can happen with editing a file multiple times\. For more information about the ECS agent configuration, see [Amazon ECS Container Agent](https://github.com/aws/amazon-ecs-agent/blob/master/README.md#environment-variables) on GitHub\.
   + If you chose to use *domainless gMSA* for initial credentials, then you must add the variable `CREDENTIALS_FETCHER_SECRET_NAME_FOR_DOMAINLESS_GMSA` with the name of the secret in Secrets Manager as the value\.
   + Optionally, you can use the variable `CREDENTIALS_FETCHER_HOST` if you change the `credentials-fetcher` daemon configuration to move the socket to another location\.

**Setting up permissions and secrets**  
Do the following steps once for each application and each task definition\. We recommend that you use the best practice of granting the least privilege and narrow the permissions used in the policy so that each task can only read the secrets that it needs\.

1. <a name="linux-gmsa-domainless-secret"></a>

**\(Optional\) domainless gMSA secret**

   If you chose to use the domainless method where the instance isn't joined to the domain, then follow this step\.

   You must add the following permissions as an inline policy to the Amazon EC2 instance IAM role to give the `credentials-fetcher` daemon access to the Secrets Manager secret\. Replace the `MySecret` example with the Amazon Resource Name \(ARN\) of your secret in the `Resource` list\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "secretsmanager:GetSecretValue"
               ],
               "Resource": [
                   "arn:aws:ssm:region:111122223333:secret:MySecret"
               ]
           }
       ]
   }
   ```
**Note**  
If you chose to use your own KMS key to encrypt your secret, then you must add the necessary permissions to this role and add this role to the AWS KMS key policy\.

1. 

**Decide if you are using SSM Parameter Store or S3 to store the credspec**

   Amazon ECS supports the following ways to reference the file path in the `dockerSecurityOptions` field of the task definition\.

   For more information about the credspec, see [Credential specification file](#linux-gmsa-credentialspec)\.
   + <a name="linux-gmsa-credspec-s3"></a>

**Amazon S3 Bucket**

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
   + <a name="linux-gmsa-credspec-ssm"></a>

**SSM Parameter Store parameter**

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

## Credential specification file<a name="linux-gmsa-credentialspec"></a>

Amazon ECS uses an Active Directory credential specification file \(*CredSpec*\) that contains the gMSA metadata used to propagate the gMSA account context to the Linux container\. You generate the CredSpec and reference it in the `dockerSecurityOptions` field in your task definition\. The CredSpec file does not contain any secrets\.

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
<a name="linux-gmsa-credentialspec-create"></a>
**Creating a CredSpec**  
You create a CredSpec by using the CredentialSpec PowerShell module on a Windows computer that is joined to the domain\. Follow the steps in [Create a credential spec](https://learn.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec) on the Microsoft Learn website\.