# Related services<a name="welcome-related"></a>

Amazon ECS can be used along with the following AWS services:

**AWS Identity and Access Management**  
AWS Identity and Access Management \(IAM\) is an access management service that helps you securely control access to AWS resources\. You can use IAM to control who's authenticated \(signed in\) and authorized \(has permissions\) to view or perform specific actions on resources\. In Amazon ECS, you can use IAM to control access at the container instance level using IAM roles\. You can also use it to control access at the task level using IAM task roles\. For more information, see [Identity and access management for Amazon Elastic Container Service](security-iam.md)\. 

**Amazon EC2 Auto Scaling**  
Auto Scaling is a service that sets up automatic scaling for your tasks\. The scaling is based on user\-defined policies, health status checks, and schedules\. You can use Auto Scaling alongside a Fargate task within a service to scale in response to a number of metrics\. Or, alternatively, you can use it with an EC2 task to scale the container instances within your cluster\. For more information, see [Service auto scaling](service-auto-scaling.md)\.

**Elastic Load Balancing**  
The Elastic Load Balancing service automatically distributes incoming application traffic across the tasks in your Amazon ECS service\. You can use it to achieve greater levels of fault tolerance in your applications\. At the same time, you can use it to also provide the amount of load\-balancing capacity that's required to distribute application traffic\. You can use Elastic Load Balancing to create an endpoint that balances traffic across services in a cluster\. For more information, see [Service load balancing](service-load-balancing.md)\.

**Amazon Elastic Container Registry**  
Amazon ECR is a managed AWS Docker registry service that's secure, scalable, and reliable\. Amazon ECR supports private Docker repositories with resource\-based permissions using IAM so that specific users or tasks can access repositories and images\. Developers can use the Docker CLI to push, pull, and manage images\. For more information, see the *[Amazon Elastic Container Registry User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)*\.

**AWS CloudFormation**  
AWS CloudFormation gives developers and systems administrators an easy way to create and manage a collection of related AWS resources\. More specifically, it makes provisioning and updating resources more predictable\. You can define clusters, task definitions, and services as entities in an AWS CloudFormation script\. For more information, see [AWS CloudFormation Template Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/)\.