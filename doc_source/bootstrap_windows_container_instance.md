# Bootstrapping Windows container instances with Amazon EC2 user data<a name="bootstrap_windows_container_instance"></a>

When you launch an Amazon ECS container instance, you have the option of passing user data to the instance\. The data can be used to perform common automated configuration tasks and even run scripts when the instance boots\. For Amazon ECS, the most common use cases for user data are to pass configuration information to the Docker daemon and the Amazon ECS container agent\. 

You can pass multiple types of user data to Amazon EC2, including cloud boothooks, shell scripts, and `cloud-init` directives\. For more information about these and other format types, see the [Cloud\-Init documentation](https://cloudinit.readthedocs.io/en/latest/topics/format.html)\. 

You can pass this user data when using the Amazon EC2 launch wizard\. For more information, see [Launching an Amazon ECS Linux container instance](launch_container_instance.md)\.

**Topics**
+ [Default Windows user data](#windows-default-userdata)
+ [Windows agent installation user data](#agent-service-userdata)
+ [Windows IAM roles for tasks](#windows-iam-roles-tasks)

## Default Windows user data<a name="windows-default-userdata"></a>

This example user data script shows the default user data that your Windows container instances receive if you use the [cluster creation wizard](create_cluster.md)\. The below script does the following:
+ Sets the cluster name to `windows`\.
+ Sets the IAM roles for tasks\.
+ Sets `json-file` and `awslogs` as the available logging drivers\.

In addition, the following options are available when you use the `awsvpc` network mode\.
+ `EnableTaskENI`: This flag turns on task networking and is required when you use the `awsvpc` network mode\.
+ `AwsvpcBlockIMDS`: This optional flag blocks IMDS access for the task containers running in `awsvpc` network mode\.
+ `AwsvpcAdditionalLocalRoutes`: This optional flag allows you to have additional routes\.

  Replace `ip-address` with the IP Address for the additional routes, for example 172\.31\.42\.23/32\.

You can use this script for your own container instances \(provided that they are launched from the Amazon ECS\-optimized Windows Server AMI\), but be sure to replace the `-Cluster windows` line to specify your own cluster name \(if you are not using a cluster called `windows`\)\.

```
<powershell>
Initialize-ECSAgent -Cluster windows -EnableTaskIAMRole -LoggingDrivers '["json-file","awslogs"]' -EnableTaskENI -AwsvpcBlockIMDS -AwsvpcAdditionalLocalRoutes
'["ip-address"]'
</powershell>
```

## Windows agent installation user data<a name="agent-service-userdata"></a>

This example user data script installs the Amazon ECS container agent on an instance launched with a **Windows\_Server\-2016\-English\-Full\-Containers** AMI\. It has been adapted from the agent installation instructions on the [Amazon ECS Container Agent GitHub repository](https://github.com/aws/amazon-ecs-agent) README page\.

**Note**  
This script is shared for example purposes\. It is much easier to get started with Windows containers by using the Amazon ECS\-optimized Windows Server AMI\. For more information, see [Creating a cluster using the classic console](create_cluster.md)\.

You can use this script for your own container instances \(provided that they are launched with a version of the **Windows\_Server\-2016\-English\-Full\-Containers** AMI\)\. Be sure to replace the `windows` line to specify your own cluster name \(if you are not using a cluster called `windows`\)\.

```
<powershell>
# Set up directories the agent uses
New-Item -Type directory -Path ${env:ProgramFiles}\Amazon\ECS -Force
New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS -Force
New-Item -Type directory -Path ${env:ProgramData}\Amazon\ECS\data -Force
# Set up configuration
$ecsExeDir = "${env:ProgramFiles}\Amazon\ECS"
[Environment]::SetEnvironmentVariable("ECS_CLUSTER", "windows", "Machine")
[Environment]::SetEnvironmentVariable("ECS_LOGFILE", "${env:ProgramData}\Amazon\ECS\log\ecs-agent.log", "Machine")
[Environment]::SetEnvironmentVariable("ECS_DATADIR", "${env:ProgramData}\Amazon\ECS\data", "Machine")
# Download the agent
$agentVersion = "latest"
$agentZipUri = "https://s3.amazonaws.com/amazon-ecs-agent/ecs-agent-windows-$agentVersion.zip"
$zipFile = "${env:TEMP}\ecs-agent.zip"
Invoke-RestMethod -OutFile $zipFile -Uri $agentZipUri
# Put the executables in the executable directory.
Expand-Archive -Path $zipFile -DestinationPath $ecsExeDir -Force
Set-Location ${ecsExeDir}
# Set $EnableTaskIAMRoles to $true to enable task IAM roles
# Note that enabling IAM roles will make port 80 unavailable for tasks.
[bool]$EnableTaskIAMRoles = $false
if (${EnableTaskIAMRoles}) {
  $HostSetupScript = Invoke-WebRequest https://raw.githubusercontent.com/aws/amazon-ecs-agent/master/misc/windows-deploy/hostsetup.ps1
  Invoke-Expression $($HostSetupScript.Content)
}
# Install the agent service
New-Service -Name "AmazonECS" `
        -BinaryPathName "$ecsExeDir\amazon-ecs-agent.exe -windows-service" `
        -DisplayName "Amazon ECS" `
        -Description "Amazon ECS service runs the Amazon ECS agent" `
        -DependsOn Docker `
        -StartupType Manual
sc.exe failure AmazonECS reset=300 actions=restart/5000/restart/30000/restart/60000
sc.exe failureflag AmazonECS 1
Start-Service AmazonECS
</powershell>
```

## Windows IAM roles for tasks<a name="windows-iam-roles-tasks"></a>

See the following Windows examples regarding bootstrapping IAM task roles\.
+ [Additional configuration for Windows IAM roles for tasks](windows_task_IAM_roles.md)
+ [IAM roles for task container bootstrap script](windows_task_IAM_roles.md#windows_task_IAM_roles_bootstrap)