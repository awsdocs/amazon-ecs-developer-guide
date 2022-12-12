# Interconnecting services<a name="interconnecting-services"></a>

Applications that run in Amazon ECS tasks often need to receive connections from the internet or to connect to other applications that run in Amazon ECS services\. If you need external connections from the internet, we recommend using Elastic Load Balancing\. For more information about integrated load balancing, see [Service load balancing](service-load-balancing.md)\.

## Choosing an interconnection method<a name="connection-options"></a>

If you need an application to connect to other applications that run in Amazon ECS services, Amazon ECS provides three ways to do this without a load balancer:
+ *Amazon ECS Service Connect*

  Amazon ECS Service Connect provides management of service\-to\-service communication as Amazon ECS configuration\. It does this by building both service discovery and a service mesh in Amazon ECS\. This provides  the complete configuration inside each Amazon ECS service that you manage by service deployments, a unified way to refer to your services within namespaces that doesn't depend on the Amazon VPC DNS configuration, and standardized metrics and logs to monitor all of your applications on Amazon ECS\. Amazon ECS Service Connect only interconnects Amazon ECS services\.

   You must configure any cross\-VPC connectivity that you want to use with Amazon ECS Service Connect\. There's no additional Amazon VPC or network infrastructure configuration required for service\-to\-service communication when using Service Connect beyond the cross\-VPC connectivity\. Service Connect configures each task for your applications to discover services\. Service Connect configures DNS names for your services in the task itself, and doesn't require nor create DNS records in your hosted zones\.

  For more information, see [Service Connect ](service-connect.md)\.
+ *Amazon ECS service discovery*

  Amazon ECS service discovery integrates services with AWS Cloud Map namespaces to add entries \(specifically, AWS Cloud Map service instances\) to the namespace for each task in the Amazon ECS service\. To connect, an app resolves these entries as DNS hostname records or uses the AWS Cloud Map API to get the IP address of the tasks\.

  Amazon ECS service discovery can be used with any applications, including UDP connections\. Service discovery doesn't affect the connecting protocol or traffic route\.

  For more information, see [Service discovery](service-discovery.md)
+ *AWS App Mesh*

  AWS App Mesh is a service mesh that you can use to monitor and control services\. App Mesh standardizes how your services communicate and can help ensure high availability\. You can use App Mesh to monitor your applications and network traffic for each of your services and applications that run outside of Amazon ECS\.

  App Mesh is configured in a new task definition revision and isn't applied to Amazon ECS services\. Add the `proxyConfiguration` field to the task definition and a container with the proxy inside of it\. You can configure the proxy container for advanced features\.

  App Mesh has advanced traffic routing for release controls that are unaffected by the Amazon ECS service deployment methods\.

  For more information, see [Use App Mesh with Amazon ECS](gs-app-mesh.md)\.

## Network mode compatibility table<a name="interconnect-network-mode-compatibility-table"></a>

The following table covers the compatibility between these options and the task network modes\. In the table, "client" refers to the application that's making the connections from inside an Amazon ECS task\.


****  

| Interconnection Options | Bridged | `awsvpc` | Host | 
| --- | --- | --- | --- | 
| Service discovery | yes, but requires clients be aware of SRV records in DNS without hostPort\. | yes | yes, but requires clients be aware of SRV records in DNS without hostPort\. | 
| Service Connect  | yes | yes | no | 
| App Mesh | no | yes | no | 