# Amazon ECS clusters in Local Zones, Wavelength Zones, and AWS Outposts<a name="cluster-regions-zones"></a>

Amazon ECS supports workloads that take advantage of Local Zones, Wavelength Zones and AWS Outposts when low latency or local data processing requirements are needed\.
+ Local Zones are an extension of an AWS Region that provide you the ability to place resources in multiple locations closer to your end users\.
+ Wavelength Zones allow developers to build applications that deliver ultra\-low latencies to 5G devices and end users\. Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks\.
+ AWS Outposts brings native AWS services, infrastructure, and operating models to virtually any data center, co\-location space, or on\-premises facility\.

**Important**  
Amazon ECS on AWS Fargate workloads are not supported in Local Zones, Wavelength Zones, or on AWS Outposts at this time\.

We describe each of these in more detail in the following section\.

## Local Zones<a name="clusters-local-zones"></a>

A *Local Zone* is an extension of an AWS Region in geographic proximity to your users\. Local Zones have their own connections to the internet and support AWS Direct Connect, so resources created in a Local Zone can serve local users with low\-latency communications\. For more information, see [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)\.

A Local Zone is represented by a Region code followed by an identifier that indicates the location, for example, `us-west-2-lax-1a`\.

To use a Local Zone, you must opt\-in to the zone\. Once you have opted in, you must create a Amazon VPC and subnet in the Local Zone\. Then you will be ready to launch your Amazon EC2 instances, Amazon FSx file servers, and Application Load Balancers in them to use for your Amazon ECS clusters and tasks\. For more information, see [Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-local-zones) in the *Amazon EC2 User Guide for Linux Instances*\.

## Wavelength Zones<a name="clusters-wavelength-zones"></a>

*AWS Wavelength* allows developers to build applications that deliver ultra\-low latencies to mobile devices and end users\. Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks\. Developers can extend a Amazon Virtual Private Cloud to one or more Wavelength Zones, and then use AWS resources like Amazon EC2 instances to run applications that require ultra\-low latency and a connection to AWS services in the Region\.

A Wavelength Zone is an isolated zone in the carrier location where the Wavelength infrastructure is deployed\. Wavelength Zones are tied to a Region\. A Wavelength Zone is a logical extension of a Region, and is managed by the control plane in the Region\.

A Wavelength Zone is represented by a Region code followed by an identifier that indicates the Wavelength Zone, for example, `us-east-1-wl1-bos-wlz-1`\.

To use a Wavelength Zone, you must opt\-in to the zone\. Once you have opted in, you must create a Amazon VPC and subnet in the Local Zone\. Then you will be ready to launch your Amazon EC2 instances in them to use for your Amazon ECS clusters and tasks\. For more information, see [Get started with AWS Wavelength](https://docs.aws.amazon.com/wavelength/latest/developerguide/get-started-wavelength.html) in the *AWS Wavelength Developer Guide*\.

Wavelength Zones are not available in every Region\. For information about the Regions that support Wavelength Zones, see [Available Wavelength Zones](https://docs.aws.amazon.com/wavelength/latest/developerguide/wavelength-quotas.html) in the *AWS Wavelength Developer Guide*\.

## AWS Outposts<a name="clusters-outposts"></a>

AWS Outposts enables native AWS services, infrastructure, and operating models in on\-premises facilities\. In AWS Outposts environments, you can use the same AWS APIs, tools, and infrastructure that you use in the AWS Cloud\. Amazon ECS on AWS Outposts is ideal for low\-latency workloads that need to be run in close proximity to on\-premises data and applications\. For more information about AWS Outposts, see [Amazon Elastic Container Service on AWS Outposts](ecs-on-outposts.md)\.