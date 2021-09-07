# Amazon ECS\-optimized AMI versions<a name="ecs-windows-ami-versions"></a>

This topic lists the current and previous versions of the Amazon ECS\-optimized AMIs and their corresponding versions of the Amazon ECS container agent, Docker, and the `ecs-init` package\.

The Amazon ECS\-optimized AMI metadata, including the AMI ID, for each variant can be retrieved programmatically\. For more information, see [Retrieving Amazon ECS\-Optimized AMI metadata](retrieve-ecs-optimized_windows_AMI.md)\.

## Windows Amazon ECS\-optimized AMIs versions<a name="ecs-ami-versions-windows"></a>

The following tabs display a list of Windows Amazon ECS\-optimized AMIs versions\.

**Note**  
For details on referencing the Systems Manager Parameter Store parameter in an AWS CloudFormation template, see [Using the latest recommended Amazon ECS\-optimized AMI in an AWS CloudFormation template](retrieve-ecs-optimized_AMI.md#ecs-optimized-ami-parameter-examples-5)\.

**Important**  
To ensure that customers have the latest security updates by default, Amazon ECS maintains at least the last three Windows Amazon ECS\-optimized AMIs\. After releasing new Windows Amazon ECS\-optimized AMIs, Amazon ECS makes the Windows Amazon ECS\-optimized AMIs that are older private\. If there is a private AMI that you need access to, let us know by filing a ticket with Cloud Support\.

------
#### [ Windows Server 2019 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2019 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2019 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.03\.11**  |  `1.50.2`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.02\.10**  |  `1.50.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.01\.13**  |  `1.49.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.11\.18**  |  `1.48.0`  |  `19.03.13`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.11\.06**  |  `1.47.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.10\.14**  |  `1.45.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2019\.05\.10**  |  `1.27.0`  |  `18.09.4`  |  Private  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 2019 Full AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Full-ECS_Optimized
```

------
#### [ Windows Server 2019 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2019 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2019 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.03\.11**  |  `1.50.2`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.02\.10**  |  `1.50.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.01\.13**  |  `1.49.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.11\.18**  |  `1.48.0`  |  `19.03.13`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.11\.06**  |  `1.47.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.10\.14**  |  `1.45.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.09\.09**  |  `1.44.3`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 2019 Full AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2019-English-Core-ECS_Optimized
```

------
#### [ Windows Server 2016 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2016 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2016 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.03\.11**  |  `1.50.2`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.02\.10**  |  `1.50.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.01\.13**  |  `1.49.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.11\.18**  |  `1.48.0`  |  `19.03.13`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.11\.06**  |  `1.47.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.10\.14**  |  `1.45.0`  |  `19.03.12`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.09\.09**  |  `1.44.3`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.07\.15**  |  `1.41.1`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.06\.11**  |  `1.40.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.05\.14**  |  `1.39.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2020\.01\.15**  |  `1.35.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.12\.16**  |  `1.34.0`  |  `19.03.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.11\.25**  |  `1.34.0`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.11\.13**  |  `1.32.1`  |  `19.03.4`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.10\.09**  |  `1.32.0`  |  `19.03.2`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.09\.11**  |  `1.30.0`  |  `19.03.1`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.08\.16**  |  `1.29.1`  |  `19.03.1`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.07\.19**  |  `1.29.0`  |  `18.09.8`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2019\.03\.07**  |  `1.26.0`  |  `18.03.1`  |  Private  | 

Use the following AWS CLI Amazon ECS\-optimized Windows Server 2016 Full AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2016-English-Full-ECS_Optimized
```

------
#### [ Windows Server 2004 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2004 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2004 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Public  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.03\.11**  |  `1.50.2`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.02\.10**  |  `1.50.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2021\.01\.13**  |  `1.49.0`  |  `19.03.14`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2020\.11\.18**  |  `1.48.0`  |  `19.03.13`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2020\.11\.06**  |  `1.47.0`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2020\.10\.14**  |  `1.45.0`  |  `19.03.12`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2020\.09\.09**  |  `1.44.3`  |  `19.03.11`  |  Private  | 
|  **Windows\_Server\-2004\-English\-Core\-ECS\_Optimized\-2020\.08\.12**  |  `1.43.0`  |  `19.03.11`  |  Private  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 2004 Core AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2004-English-Core-ECS_Optimized
```

------
#### [ Windows Server 20H2 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 20H2 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 20H2 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Public  | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Public  | 
|  **Windows\_Server\-20H2\-English\-Core\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Public  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 20H2 Core AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-20H2-English-Core-ECS_Optimized
```

------