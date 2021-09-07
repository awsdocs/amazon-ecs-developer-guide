# Fargate task recycling<a name="task-recycle"></a>

When AWS determines that a security or infrastructure update is needed for an Amazon ECS task hosted on AWS Fargate, it will apply the necessary patches for the task\. Most of these patches will be transparent and the task won't need to be stopped, but on occasion it is necessary for the task to be recycled\. Beginning with Fargate platform version `1.3.0`, any Fargate tasks launched as part of a service may be stopped and a new one started by the Amazon ECS service scheduler in order to provide the best possible security and availability for the task\. The service scheduler will ensure that the desired task count for your service will be maintained\.

For information about the signals that ae sent to stop a take, see [Load balancer container draining](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/load-balancer-connection-draining.html) in the *Amazon ECS Best Practices Guide*\.

To prepare for this process, we recommending testing your application behavior by simulating this scenario\. You can do this by stopping an individual task in your service to test for resiliency\.

**Note**  
Amazon ECS doesn't send a notification prior to the recycling event\.