# Cluster query language<a name="cluster-query-language"></a>

Cluster queries are expressions that enable you to group objects\. For example, you can group container instances by attributes such as Availability Zone, instance type, or custom metadata\. For more information, see [Attributes](task-placement-constraints.md#attributes)\.

After you have defined a group of container instances, you can customize Amazon ECS to place tasks on container instances based on group\. For more information, see [Running tasks](ecs_run_task.md) and [Creating a service](create-service.md)\. You can also apply a group filter when listing container instances\. For more information, see [Filtering by attribute](task-placement-constraints.md#filter-attribute)\.

## Expression syntax<a name="expression-syntax"></a>

Expressions have the following syntax:

```
subject operator [argument]
```

**Subject**  
The attribute or field to be evaluated\.

`agentConnected`  
Select container instances by their Amazon ECS container agent connection status\. You can use this filter to search for instances with container agents that are disconnected\.  
Valid operators: equals \(==\), not\_equals \(\!=\), in, not\_in \(\!in\), matches \(=\~\), not\_matches \(\!\~\)

`agentVersion`  
Select container instances by their Amazon ECS container agent version\. You can use this filter to find instances that are running outdated versions of the Amazon ECS container agent\.  
Valid operators: equals \(==\), not\_equals \(\!=\), greater\_than \(>\), greater\_than\_equal \(>=\), less\_than \(<\), less\_than\_equal \(<=\)

`attribute:attribute-name`  
Select container instances by attribute\. For more information, see [Attributes](task-placement-constraints.md#attributes)\.

`ec2InstanceId`  
Select container instances by their Amazon EC2 instance ID\.  
Valid operators: equals \(==\), not\_equals \(\!=\), in, not\_in \(\!in\), matches \(=\~\), not\_matches \(\!\~\)

`registeredAt`  
Select container instances by their container instance registration date\. You can use this filter to find newly registered instances or instances that are very old\.  
Valid operators: equals \(==\), not\_equals \(\!=\), greater\_than \(>\), greater\_than\_equal \(>=\), less\_than \(<\), less\_than\_equal \(<=\)  
Valid date formats: 2018\-06\-18T22:28:28\+00:00, 2018\-06\-18T22:28:28Z, 2018\-06\-18T22:28:28, 2018\-06\-18

`runningTasksCount`  
Select container instances by number of running tasks\. You can use this filter to find instances that are empty or near empty \(few tasks running on them\)\.  
Valid operators: equals \(==\), not\_equals \(\!=\), greater\_than \(>\), greater\_than\_equal \(>=\), less\_than \(<\), less\_than\_equal \(<=\)

`task:group`  
Select container instances by task group\. For more information, see [Task groups](task-placement-constraints.md#task-groups)\.

**Operator**  
The comparison operator\. The following operators are supported\.


| Operator | Description | 
| --- | --- | 
|  ==, equals  |  String equality  | 
|  \!=, not\_equals  |  String inequality  | 
|  >, greater\_than  |  Greater than  | 
|  >=, greater\_than\_equal  |  Greater than or equal to  | 
|  <, less\_than  |  Less than  | 
|  <=, less\_than\_equal  |  Less than or equal to  | 
|  exists  |  Subject exists  | 
|  \!exists, not\_exists  |  Subject does not exist  | 
|  in  |  Value in argument list  | 
|  \!in, not\_in  |  Value not in argument list  | 
|  =\~, matches  |  Pattern match  | 
|  \!\~, not\_matches  |  Pattern mismatch  | 

**Note**  
A single expression can't contain parentheses\. However, parentheses can be used to specify precedence in compound expressions\.

**Argument**  
For many operators, the argument is a literal value\.

The `in` and `not_in` operators expect an argument list as the argument\. You specify an argument list as follows:

```
[argument1, argument2, ..., argumentN]
```

The matches and not\_matches operators expect an argument that conforms to the Java regular expression syntax\. For more information, see [java\.util\.regex\.Pattern](http://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html)\.

**Compound expressions**

You can combine expressions using the following Boolean operators:
+ &&, and
+ \|\|, or
+ \!, not

You can specify precedence using parentheses:

```
(expression1 or expression2) and expression3
```

## Example expressions<a name="expression-examples"></a>

The following are example expressions\.

**Example: String Equality**  
The following expression selects instances with the specified instance type\.

```
attribute:ecs.instance-type == t2.small
```

**Example: Argument List**  
The following expression selects instances in the us\-east\-1a or us\-east\-1b Availability Zone\.

```
attribute:ecs.availability-zone in [us-east-1a, us-east-1b]
```

**Example: Compound Expression**  
The following expression selects G2 instances that are not in the us\-east\-1d Availability Zone\.

```
attribute:ecs.instance-type =~ g2.* and attribute:ecs.availability-zone != us-east-1d
```

**Example: Task Affinity**  
The following expression selects instances that are hosting tasks in the `service:production` group\.

```
task:group == service:production
```

**Example: Task Anti\-Affinity**  
The following expression selects instances that are not hosting tasks in the database group\.

```
not(task:group == database)
```

**Example: Running task count**  
The following expression selects instances that are only running one task\.

```
runningTasksCount == 1
```

**Example: Amazon ECS container agent version**  
The following expression selects instances that are running a container agent version below 1\.14\.5\.

```
agentVersion < 1.14.5
```

**Example: Instance registration time**  
The following expression selects instances that were registered before February 13, 2018\.

```
registeredAt < 2018-02-13
```

**Example: Amazon EC2 instance ID**  
The following expression selects instances with the following Amazon EC2 instance IDs\.

```
ec2InstanceId in ['i-abcd1234', 'i-wxyx7890']
```