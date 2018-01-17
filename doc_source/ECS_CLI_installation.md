# Installing the Amazon ECS CLI<a name="ECS_CLI_installation"></a>

Follow these instructions to install the Amazon ECS CLI on your macOS, Linux, or Windows system\.

**To install the Amazon ECS CLI**

1. Download the binary\.

   + For macOS:

     ```
     sudo curl -o /usr/local/bin/ecs-cli https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-darwin-amd64-latest
     ```

   + For Linux systems:

     ```
     sudo curl -o /usr/local/bin/ecs-cli https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-linux-amd64-latest
     ```

   + For Windows systems:

     Open Windows PowerShell and run the following commands:

     ```
     PS C:\> New-Item ‘C:\Program Files\Amazon\ECSCLI’ -type directory
     PS C:\> Invoke-WebRequest -OutFile ‘C:\Program Files\Amazon\ECSCLI\ecs-cli.exe’ https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-windows-amd64-latest.exe
     ```
**Note**  
If you encounter permission issues, ensure you are running PowerShell as Administrator\.

1. \(Optional\) Verify the downloaded binary with the MD5 sum provided\.

   + For macOS \(compare the two output strings to verify that they match\):

     ```
     curl -s https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-darwin-amd64-latest.md5 && md5 -q /usr/local/bin/ecs-cli
     ```

   + For Linux systems \(look for an `OK` in the output string\):

     ```
     echo "$(curl -s https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-linux-amd64-latest.md5) /usr/local/bin/ecs-cli" | md5sum -c -
     ```

   + For Windows systems:

     Open Windows PowerShell and find the md5 hash of the executable you downloaded:

     ```
     PS C:\> Get-FileHash ecs-cli.exe -Algorithm MD5
     ```

     Compare that with this md5 hash:

     ```
     PS C:\> Invoke-WebRequest -OutFile md5.txt https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-windows-amd64-latest.md5
     PS C:\> Get-Content md5.txt
     ```

1. Apply execute permissions to the binary\.

   + For macOS and Linux systems:

     ```
     sudo chmod +x /usr/local/bin/ecs-cli
     ```

   + For Windows systems:

     Edit the environment variables and add `C:\Program Files\Amazon\ECSCLI` to the `PATH` variable field, separated from existing entries by using a semicolon\. For example:

     ```
     C:\existing\path;C:\Program Files\Amazon\ECSCLI
     ```

     Restart PowerShell \(or the command prompt\) so the changes will go into effect\.
**Note**  
Once the `PATH` variable is set, the ECS CLI can be used from either Windows PowerShell or the command prompt\.

1. Verify that the CLI is working properly\.

   ```
   ecs-cli --version
   ```

1. Proceed to [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)\.
**Important**  
You must configure the ECS CLI with your AWS credentials, an AWS region, and an Amazon ECS cluster name before you can use it\.