# Deregister a container instance<a name="deregister_container_instance"></a>

When you are finished with a container instance, you can deregister it from your cluster\.

Following deregistration, the container instance is no longer able to accept new tasks\. If you have tasks running on the container instance when you deregister it, these tasks remain running until you terminate the instance or the tasks stop through some other means\. 

However, these tasks are orphaned \(no longer monitored or accounted for by Amazon ECS\)\. If an orphaned task on your container instance is part of an Amazon ECS service, then the service scheduler starts another copy of that task, on a different container instance, if possible\. Any containers in orphaned service tasks that are registered with a Classic Load Balancer or an Application Load Balancer target group are deregistered\. They begin connection draining according to the settings on the load balancer or target group\.

**Note**
Any running tasks running on `awsvpc` network mode will have their underlying Elastic Network Interfaces deleted.

If you intend to use the container instance for some other purpose after deregistration, you should stop all of the tasks running on the container instance before deregistration\. This stops any orphaned tasks from consuming resources\.

**Important**  
Because each container instance has unique state information, they should not be deregistered from one cluster and re\-registered into another\. To relocate container instance resources, we recommend that you terminate container instances from one cluster and launch new container instances with the latest Amazon ECS\-optimized Amazon Linux 2 AMI in the new cluster\. For more information, see [Terminate Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances* and [Launching an Amazon ECS Container Instance](launch_container_instance.md)\.

Deregistering a container instance removes the instance from a cluster, but it does not terminate the EC2 instance\. If you are finished using the instance, be sure to terminate it in the Amazon EC2 console to stop billing\. For more information, see [Terminate Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
If you terminate a running container instance with a connected Amazon ECS container agent, the agent automatically deregisters the instance from your cluster\. Stopped container instances or instances with disconnected agents are not automatically deregistered when terminated\.

**To deregister a container instance**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. From the navigation bar, choose the Region in which your container instance is registered\.

1. In the navigation pane, choose **Clusters** and select the cluster that hosts your container instance\.

1. On the **Cluster : *name*** page, choose **ECS Instances**\.  
![\[ECS Instances Tab\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/images/ECS_Instances_tab.png)

1. Select the container instance ID to deregister\.

1. On the **Container Instance : *id*** page, choose **Deregister**\.

1. Review the deregistration message, and choose **Yes, Deregister**\.

1. If you are finished with the container instance, terminate the underlying Amazon EC2 instance\. For more information, see [Terminate Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
If your instance is maintained by an Auto Scaling group or AWS CloudFormation stack, terminate the instance by updating the Auto Scaling group or AWS CloudFormation stack\. Otherwise, the Auto Scaling group re\-creates the instance after you terminate it\.
