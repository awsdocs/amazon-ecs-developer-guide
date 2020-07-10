# Registering multiple target groups with a service<a name="register-multiple-targetgroups"></a>

Your Amazon ECS service can serve traffic from multiple load balancers and expose multiple load balanced ports when you specify multiple target groups in a service definition\.

To create a service specifying multiple target groups, you must create the service using the Amazon ECS API, SDK, AWS CLI, or an AWS CloudFormation template\. After the service is created, you can view the service and the target groups registered to it with the AWS Management Console\. It is not possible to update the load balancing configuration of an existing service\.

Multiple target groups can be specified in a service definition using the following format\. For the full syntax of a service definition, see [Service definition template](service_definition_parameters.md#sd-template)\.

```
"loadBalancers":[
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_1/1234567890123456",
      "containerName":"container_name",
      "containerPort":container_port
   },
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_2/6543210987654321",
      "containerName":"container_name",
      "containerPort":container_port
   }
]
```

## Multiple target group considerations<a name="multiple-targetgroups-considerations"></a>

The following should be considered when you specify multiple target groups in a service definition\.
+ For services that use an Application Load Balancer or Network Load Balancer, you cannot attach more than five target groups to a service\.
+ Specifying multiple target groups in a service definition is only supported under the following conditions:
  + The service must use either an Application Load Balancer or Network Load Balancer\.
  + The service must use the rolling update \(`ECS`\) deployment controller type\.
+ Specifying multiple target groups is supported for services containing tasks using both the Fargate and EC2 launch types\.
+ When creating a service that specifies multiple target groups, the Amazon ECS service\-linked role must be created\. The role is created by omitting the `role` parameter in API requests, or the `Role` property in AWS CloudFormation\. For more information, see [Service\-Linked Role for Amazon ECS](using-service-linked-roles.md)\.

## Example service definitions<a name="multiple-targetgroups-examples"></a>

Following are a few example use cases for specifying multiple target groups in a service definition\. For the full syntax of a service definition, see [Service definition template](service_definition_parameters.md#sd-template)\.

### Example: Having separate load balancers for internal and external traffic<a name="multiple-targetgroups-example1"></a>

In the following use case, a service uses two separate load balancers, one for internal traffic and a second for internet\-facing traffic, for the same container and port\.

```
"loadBalancers":[
   //Internal ELB
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_1/1234567890123456",
      "containerName":"nginx",
      "containerPort":8080
   },
   //Internet-facing ELB
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_2/6543210987654321",
      "containerName":"nginx",
      "containerPort":8080
   }
]
```

### Example: Exposing multiple ports from the same container<a name="multiple-targetgroups-example1"></a>

In the following use case, a service uses one load balancer but exposes multiple ports from the same container\. For example, a Jenkins container might expose port 8080 for the Jenkins web interface and port 50000 for the API\.

```
"loadBalancers":[
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_1/1234567890123456",
      "containerName":"jenkins",
      "containerPort":8080
   },
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_2/6543210987654321",
      "containerName":"jenkins",
      "containerPort":50000
   }
]
```

### Example: Exposing ports from multiple containers<a name="multiple-targetgroups-example3"></a>

In the following use case, a service uses one load balancer and two target groups to expose ports from separate containers\.

```
"loadBalancers":[
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_1/1234567890123456",
      "containerName":"webserver",
      "containerPort":80
   },
   {  
      "targetGroupArn":"arn:aws:elasticloadbalancing:region:123456789012:targetgroup/target_group_name_2/6543210987654321",
      "containerName":"database",
      "containerPort":3306
   }
]
```