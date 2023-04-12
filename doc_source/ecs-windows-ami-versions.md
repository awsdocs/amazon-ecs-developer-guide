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
#### [ Windows Server 2022 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2022 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2022 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2023\.03\.21**  |  `1.69.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2023\.02\.21**  |  `1.68.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2023\.01\.11**  |  `1.68.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.12\.14**  |  `1.67.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.11\.09**  |  `1.65.1`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.10\.12**  |  `1.64.0`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.09\.22**  |  `1.63.1`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.09\.14**  |  `1.62.2`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.08\.15**  |  `1.62.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.07\.13**  |  `1.61.3`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.06\.15**  |  `1.61.2`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2022\.01\.18**  |  `1.57.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2021\.12\.16**  |  `1.57.1`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2021\.11\.11**  |  `1.57.0`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Full\-ECS\_Optimized\-2021\.009\.23**  |  `1.55.3`  |  `20.10.7`  |  Private  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 2022 Full AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2022-English-Full-ECS_Optimized
```

------
#### [ Windows Server 2022 Core AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2022 Core AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2022 Core AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2023\.03\.21**  |  `1.69.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2023\.02\.21**  |  `1.68.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2023\.01\.11**  |  `1.68.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.12\.14**  |  `1.67.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.11\.09**  |  `1.65.1`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.10\.12**  |  `1.64.0`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.09\.22**  |  `1.63.1`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.09\.14**  |  `1.62.2`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.08\.15**  |  `1.62.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.07\.13**  |  `1.61.3`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2022\.06\.15**  |  `1.61.2`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2021\.12\.16**  |  `1.57.1`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2021\.11\.11**  |  `1.57.0`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2022\-English\-Core\-ECS\_Optimized\-2021\.009\.23**  |  `1.55.3`  |  `20.10.7`  |  Private  | 

Use the following AWS CLI command to retrieve the current Amazon ECS\-optimized Windows Server 2022 Full AMI\.

```
aws ssm get-parameters --names /aws/service/ami-windows-latest/Windows_Server-2022-English-Core-ECS_Optimized
```

------
#### [ Windows Server 2019 Full AMI versions ]

The table below lists the current and previous versions of the Amazon ECS\-optimized Windows Server 2019 Full AMI and their corresponding versions of the Amazon ECS container agent and Docker\.


|  Amazon ECS\-optimized Windows Server 2019 Full AMI  |  Amazon ECS container agent version  |  Docker version  |  Visibility  | 
| --- | --- | --- | --- | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2023\.03\.21**  |  `1.69.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2023\.02\.21**  |  `1.68.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2023\.01\.11**  |  `1.68.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.12\.14**  |  `1.67.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.11\.09**  |  `1.65.1`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.10\.12**  |  `1.64.0`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.09\.22**  |  `1.63.1`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.09\.14**  |  `1.62.2`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.08\.15**  |  `1.62.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.07\.13**  |  `1.61.3`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.06\.15**  |  `1.61.2`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2022\.01\.18**  |  `1.57.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.12\.16**  |  `1.57.1`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.11\.11**  |  `1.57.0`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.009\.23**  |  `1.55.3`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Full\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Private  | 
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
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2023\.03\.21**  |  `1.69.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2023\.02\.21**  |  `1.68.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2023\.01\.11**  |  `1.68.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.12\.14**  |  `1.67.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.11\.09**  |  `1.65.1`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.10\.12**  |  `1.64.0`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.09\.22**  |  `1.63.1`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.09\.14**  |  `1.62.2`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.08\.15**  |  `1.62.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.07\.13**  |  `1.61.3`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.06\.15**  |  `1.61.2`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2022\.01\.18**  |  `1.57.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.12\.16**  |  `1.57.1`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.11\.11**  |  `1.57.0`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.09\.23**  |  `1.55.3`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Private  | 
|  **Windows\_Server\-2019\-English\-Core\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Private  | 
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
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2023\.03\.21**  |  `1.69.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2023\.02\.21**  |  `1.68.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2023\.01\.11**  |  `1.68.0`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.12\.14**  |  `1.67.2`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.11\.09**  |  `1.65.1`  |  `20.10.21 (Docker CE)`  |  Public  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.10\.12**  |  `1.64.0`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.09\.22**  |  `1.63.1`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.09\.14**  |  `1.62.2`  |  `20.10.17 (Docker CE)`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.08\.15**  |  `1.62.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.07\.13**  |  `1.61.3`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.06\.15**  |  `1.61.2`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2022\.01\.18**  |  `1.57.1`  |  `20.10.9`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.12\.16**  |  `1.57.1`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.11\.11**  |  `1.57.0`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.09\.23**  |  `1.55.3`  |  `20.10.7`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.08\.12**  |  `1.55.0`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.07\.13**  |  `1.54.02`  |  `20.10.6`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.07\.08**  |  `1.54.0`  |  `20.10.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.06\.11**  |  `1.53.0`  |  `20.10.5`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.05\.21**  |  `1.52.2`  |  `20.10.4`  |  Private  | 
|  **Windows\_Server\-2016\-English\-Full\-ECS\_Optimized\-2021\.04\.14**  |  `1.51.0`  |  `20.10.0`  |  Private  | 
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