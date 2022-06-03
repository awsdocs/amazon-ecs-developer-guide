# Amazon EC2 Windows task definition considerations<a name="windows_task_definitions"></a>

The following parameters aren't supported for Amazon EC2 Windows task definitions:
+ `containerDefinitions`
  + `disableNetworking`
  + `dnsServers`
  + `dnsSearchDomains`
  + `dockerSecurityOptions`
  + `extraHosts`
  + `links`
  + `linuxParameters`
  + `privileged`
  + `readonlyRootFilesystem`
  + `user`
  + `ulimits`
+ `volumes`
  + `dockerVolumeConfiguration`
+ `cpu`

  We recommend specifying container\-level CPU for Windows containers\.
+ `memory`

  We recommend specifying container\-level memory for Windows containers\.
+ `proxyConfiguration`
+ `ipcMode`
+ `pidMode`

## Additional configuration for Windows IAM roles for tasks<a name="windowsa-iam-task-role"></a>

The IAM roles for tasks with Windows features requires additional configuration, but much of this configuration is similar to confiuring IAM roles for tasks on Linux container instances\. For more information see [Additional configuration for Windows IAM roles for tasks](windows_task_IAM_roles.md)\.