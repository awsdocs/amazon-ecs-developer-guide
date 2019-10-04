# Registering Multiple Target Groups with a Service<a name="register-multiple-targetgroups"></a>

Your Amazon ECS service can serve traffic from multiple load balancers and expose multiple load balanced ports when you specify multiple target groups in a service definition\.

Currently, if you want to create a service specifying multiple target groups, you must create the service using the Amazon ECS API, SDK, AWS CLI, or an AWS CloudFormation template\. After the service is created, you can view the service and the target groups registered to it with the AWS Management Console\.

Multiple target groups can be specified in a service definition using the following format\.

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

## Multiple Target Group Considerations<a name="multiple-targetgroups-considerations"></a>

The following should be considered when you specify multiple target groups in a service definition:
+ Multiple target groups are only supported when you use the Application Load Balancer or Network Load Balancer load balancer types\.
+ Multiple target groups are only supported when the service uses the rolling update \(`ECS`\) deployment controller type\. If you are using the CodeDeploy or an external deployment controller, multiple target groups are not supported\.
+ Multiple target groups are supported for services containing tasks using both the Fargate and EC2 launch types\.
+ When creating a service that specifies multiple target groups, the Amazon ECS service\-linked role must be created\. The role is created by omitting the `role` parameter in API requests, or the `Role` property in AWS CloudFormation\. For more information, see [Using Service\-Linked Roles for Amazon ECS](using-service-linked-roles.md)\.

## Example Service Definitions<a name="multiple-targetgroups-examples"></a>

Following are a few example use cases for specifying multiple target groups in a service definition\.

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