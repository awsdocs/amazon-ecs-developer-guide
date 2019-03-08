# Amazon ECS IAM Policies, Roles, and Permissions<a name="IAM_policies"></a>

By default, IAM users don't have permission to create or modify Amazon ECS resources, or perform tasks using the Amazon ECS API\. This means that they also can't do so using the Amazon ECS console or the AWS CLI\. To allow IAM users to create or modify resources and perform tasks, you must create IAM policies\. Policies grant IAM users permissions to use specific resources and API actions\. Then, attach those policies to the IAM users or groups that require those permissions\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\. For more general information about IAM policies, see [Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. For more information about managing and creating custom IAM policies, see [Managing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html)\.

Likewise, Amazon ECS container instances make calls to the Amazon ECS and Amazon EC2 APIs on your behalf, so they need to authenticate with your credentials\. This authentication is accomplished by creating an IAM role for your container instances and associating that role with your container instances when you launch them\. For more information, see [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)\. If you use an Elastic Load Balancing load balancer with your Amazon ECS services, calls to the Amazon EC2 and Elastic Load Balancing APIs are made on your behalf to register and deregister container instances with your load balancers\. For more information, see [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md)\. For more general information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html) in the *IAM User Guide*\.

**Getting Started**

An IAM policy must grant or deny permission to use one or more Amazon ECS actions\. It must also specify the resources that can be used with the action, which can be all resources, or in some cases, specific resources\. The policy can also include conditions that you apply to the resource\. 

Amazon ECS partially supports resource\-level permissions\. This means that for some Amazon ECS API actions, you cannot specify which resource a user is allowed to work with for that action; instead, you have to allow users to work with all resources for that action\. 

**Topics**
+ [Policy Structure](iam-policy-structure.md)
+ [Supported Resource\-Level Permissions for Amazon ECS API Actions](ecs-supported-iam-actions-resources.md)
+ [Creating Amazon ECS IAM Policies](ECS_IAM_user_policies.md)
+ [Managed Policies and Trust Relationships](managed_policies.md)
+ [Amazon ECS Container Instance IAM Role](instance_IAM_role.md)
+ [Amazon ECS Task Execution IAM Role](task_execution_IAM_role.md)
+ [Using Service\-Linked Roles for Amazon ECS](using-service-linked-roles.md)
+ [Amazon ECS Service Scheduler IAM Role](service_IAM_role.md)
+ [Amazon ECS CodeDeploy IAM Role](codedeploy_IAM_role.md)
+ [Amazon ECS Service Auto Scaling IAM Role](autoscale_IAM_role.md)
+ [**Amazon ECS Task Role**](task_IAM_role.md)
+ [CloudWatch Events IAM Role](CWE_IAM_role.md)
+ [IAM Roles for Tasks](task-iam-roles.md)
+ [Amazon ECS IAM Policy Examples](IAMPolicyExamples.md)