# Scaling a Cluster<a name="scale_cluster"></a>

If you have a cluster that contains Amazon EC2 container instances, the following will help you scale the number of Amazon EC2 instances in your cluster\.

**Note**  
Clusters with Fargate tasks can be scaled using Service Auto Scaling\. For more information, see [Service Auto Scaling](service-auto-scaling.md)\.

If your cluster was created with the console first\-run experience after November 24th, 2015, then the Auto Scaling group associated with the AWS CloudFormation stack created for your cluster can be scaled up or down to add or remove container instances\. You can perform this scaling operation from within the Amazon ECS console\.

If your cluster was not created with the console first\-run experience after November 24th, 2015, then you cannot scale your cluster from the Amazon ECS console\. However, you can still modify existing Auto Scaling groups associated with your cluster in the Auto Scaling console\. If you do not have an Auto Scaling group associated with your cluster, you can create one from an existing container instance\. For more information, see [Creating an Auto Scaling Group Using an EC2 Instance](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-asg-from-instance.html) in the *Amazon EC2 Auto Scaling User Guide*\. You can also manually launch or terminate container instances from the Amazon EC2 console; for more information see [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

**To scale a cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the region in which your cluster exists\.

1. In the navigation pane, choose **Clusters**\.

1. Select the cluster to scale\.

1. On the **Cluster : *name*** page, choose **ECS Instances**\.  
![\[ECS scale cluster\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/scale_cluster.png)

   If a **Scale ECS Instances** button appears, then you can scale your cluster in the next step\. If not, you must manually adjust your Auto Scaling group to scale up or down your instances, or you can manually launch or terminate your container instances in the Amazon EC2 console\.

1. Choose **Scale ECS Instances**\.

1. For **Desired number of instances**, enter the number of instances to which to scale your cluster to and choose **Scale**\.
**Note**  
If you reduce the number of container instances in your cluster, randomly selected container instances are terminated until the desired count is achieved, and any tasks that are running on terminated instances are stopped\.