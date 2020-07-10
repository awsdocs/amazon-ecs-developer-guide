# Step 4: Configuring Your Service to Use Service Discovery<a name="service-configure-servicediscovery"></a>

Your Amazon ECS service can optionally enable service discovery integration, which allows your service to be discoverable via DNS\. For more information, see [Service Discovery](service-discovery.md)\.

If you are not configuring your service to use a service discovery, you can move on to the next section, [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)\.

**To configure service discovery**

1. If you have not done so already, follow the basic service configuration procedures in [Step 1: Configuring Basic Service Parameters](basic-service-params.md)\.

1. On the **Configure network** page, select **Enable service discovery integration**\.

1. For **Namespace**, select an existing Amazon Route 53 namespace, if you have one, otherwise select **create new private namespace**\.

1. If creating a new namespace, for **Namespace name** enter a descriptive name for your namespace\. This is the name used for the Amazon Route 53 hosted zone\.

1. For **Configure service discovery service**, select to either create a new service discovery service or select an existing one\.

1. If creating a new service discovery service, for **Service discovery name** enter a descriptive name for your service discovery service\. This is used as the prefix for the DNS records to be created\.

1. Select **Enable ECS task health propagation** if you want health checks enabled for your service discovery service\.

1. For **DNS record type**, select the DNS record type to create for your service\. Amazon ECS service discovery only supports A and SRV records, depending on the network mode that your task definition specifies\. For more information about these record types, see [Supported DNS Record Types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html) in the *Amazon Route 53 Developer Guide*\.
   + If the task definition that your service task specifies uses the `bridge` or `host` network mode, only type SRV records are supported\. Choose a container name and port combination to associate with the record\.
   + If the task definition that your service task specifies uses the `awsvpc` network mode, select either the A or SRV record type\. If the type A DNS record is selected, skip to the next step\. If the type SRV is selected, specify either the port that the service can be found on or a container name and port combination to associate with the record\.

1. For **TTL**, enter the resource record cache time to live \(TTL\), in seconds\. This value determines how long a record set is cached by DNS resolvers and by web browsers\.

1. Choose **Next step** to proceed and navigate to [Step 5: Configuring your service to use Service Auto Scaling](service-configure-auto-scaling.md)\.