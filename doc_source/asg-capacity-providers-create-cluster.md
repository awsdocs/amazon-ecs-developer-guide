# Creating a cluster with an Auto Scaling group capacity provider<a name="asg-capacity-providers-create-cluster"></a>

When a new Amazon ECS cluster is created, you can specify one or more capacity providers to associate with the cluster\. The associated capacity providers determine the infrastructure to run your tasks on\.

For AWS Management Console steps, see [Creating a cluster using the classic console](create_cluster.md)\.

## To create a cluster with an Auto Scaling group capacity provider \(AWS CLI\)<a name="create-cluster-cli"></a>

Use the following command to create a new cluster and associate one or more capacity providers with it\.
+ [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/ecs/create-cluster.html) \(AWS CLI\)

  ```
  aws ecs create-cluster \
       --cluster-name ASGCluster \
       --capacity-providers CapacityProviderA CapacityProviderB \
       --default-capacity-provider-strategy capacityProvider=CapacityProviderA,weight=1,base=1 capacityProvider=CapacityProviderB,weight=1 \
       --region us-west-2
  ```

  If you prefer to use a JSON input file with the `create-cluster` command, use the following command to generate a CLI skeleton\.

  ```
  aws ecs create-cluster --generate-cli-skeleton
  ```