# Amazon ECS clusters in Local Zones, Wavelength Zones, and AWS Outposts<a name="cluster-regions-zones"></a>

Amazon ECS supports workloads that use Local Zones, Wavelength Zones, and AWS Outposts for when low latency or local data processing is a requirement\.
+ You can use Local Zones are an extension of an AWS Region to place resources in multiple locations closer to your end users\.
+ You can use Wavelength Zones to build applications that deliver ultra\-low latencies to 5G devices and end users\. Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks\.
+ AWS Outposts brings native AWS services, infrastructure, and operating models to virtually any data center, co\-location space, or on\-premises facility\.

**Important**  
Amazon ECS on AWS Fargate workloads aren't supported in Local Zones, Wavelength Zones, or on AWS Outposts at this time\.

For information about the differences between Local Zones, Wavelength Zones, and AWS Outposts , see [How should I think about when to use AWS Wavelength, AWS Local Zones, or AWS Outposts for applications requiring low latency or local data processing](http://aws.amazon.com/wavelength/faqs/) in the AWS Wavelength FAQs\.

## Local Zones<a name="clusters-local-zones"></a>

A *Local Zone* is an extension of an AWS Region in close geographic proximity to your users\. Local Zones have their own connections to the internet and support AWS Direct Connect\. Resources that are created in a Local Zone can serve local users with low\-latency communications\. For more information, see [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)\.

A Local Zone is represented by a Region code followed by an identifier that indicates the location \(for example, `us-west-2-lax-1a`\)\.

To use a Local Zone, you must opt in to the zone\. After you opt in, you must create an Amazon VPC and subnet in the Local Zone\. Then, you can launch your Amazon EC2 instances, Amazon FSx file servers, and Application Load Balancers in the Local Zone to use for your Amazon ECS clusters and tasks\. For more information, see [Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-local-zones) in the *Amazon EC2 User Guide for Linux Instances*\.

## Wavelength Zones<a name="clusters-wavelength-zones"></a>

You can use *AWS Wavelength* to build applications that deliver ultra\-low latency to mobile devices and end users\. Wavelength deploys standard AWS compute and storage services to the edge of telecommunication carriers' 5G networks\. You can extend an Amazon Virtual Private Cloud to one or more Wavelength Zones\. Then, you can use AWS resources such as Amazon EC2 instances to run applications that require ultra\-low latency and a connection to AWS services in the Region\.

A Wavelength Zone is an isolated Zone in the carrier location where the Wavelength infrastructure is deployed\. Wavelength Zones are tied to an AWS Region\. A Wavelength Zone is a logical extension of a Region, and is managed by the control plane in the Region\.

A Wavelength Zone is represented by a Region code followed by an identifier that indicates the Wavelength Zone \(for example, `us-east-1-wl1-bos-wlz-1`\)\.

To use a Wavelength Zone, you must opt in to the Zone\. After you opt in, you must create an Amazon VPC and subnet in the Wavelength Zone\. Then, you can launch your Amazon EC2 instances in the Zone to use for your Amazon ECS clusters and tasks\. For more information, see [Get started with AWS Wavelength](https://docs.aws.amazon.com/wavelength/latest/developerguide/get-started-wavelength.html) in the *AWS Wavelength Developer Guide*\.

Wavelength Zones aren't available in all AWS Regions\. For information about the Regions that support Wavelength Zones, see [Available Wavelength Zones](https://docs.aws.amazon.com/wavelength/latest/developerguide/wavelength-quotas.html) in the *AWS Wavelength Developer Guide*\.

## AWS Outposts<a name="clusters-outposts"></a>

AWS Outposts enables native AWS services, infrastructure, and operating models in on\-premises facilities\. In AWS Outposts environments, you can use the same AWS APIs, tools, and infrastructure that you use in the AWS Cloud\. Amazon ECS on AWS Outposts is suitable for low\-latency workloads that require to be run in close proximity to on\-premises data and applications\. For more information about AWS Outposts, see [Amazon Elastic Container Service on AWS Outposts](ecs-on-outposts.md)\.