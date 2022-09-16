# Using the AWS Copilot command line interface<a name="AWS_Copilot"></a>

The AWS Copilot command line interface \(CLI\) commands simplify building, releasing, and operating production\-ready containerized applications on Amazon ECS from a local development environment\. The AWS Copilot CLI aligns with developer workflows that support modern application best practices: from using infrastructure as code to creating a CI/CD pipeline provisioned on behalf of a user\. Use the AWS Copilot CLI as part of your everyday development and testing cycle as an alternative to the AWS Management Console\.

AWS Copilot currently supports Linux, macOS, and Windows systems\. For more information about the latest version of the AWS Copilot CLI, see [Releases](https://github.com/aws/copilot-cli/releases)\.

**Note**  
The source code for the AWS Copilot CLI is available on [GitHub](https://github.com/aws/copilot-cli)\. The latest CLI documentation is available on the AWS Copilot [website](https://aws.github.io/copilot-cli/)\. We recommend that you submit issues and pull requests for changes that you would like to have included\. However, Amazon Web Services doesn't currently support running modified copies of AWS Copilot code\. Report issues with AWS Copilot by connecting with us on [Gitter](https://gitter.im/aws/copilot-cli) or [GitHub](https://github.com/aws/copilot-cli) where you can open issues, provide feedback, and report bugs\.

## Installing the AWS Copilot CLI<a name="copilot-install"></a>

The AWS Copilot CLI can be installed on Linux or macOS systems either by using Homebrew or by manually downloading the binary\. Use the following steps with your preferred installation method\.

### Installing the AWS Copilot CLI using Homebrew<a name="copilot-install-homebrew"></a>

The following command is used to install the AWS Copilot CLI on your macOS or Linux system using Homebrew\. Before installation, you should have Homebrew installed\. For more information, see [Homebrew](https://brew.sh/)\.

```
brew install aws/tap/copilot-cli
```

### Manually installing the AWS Copilot CLI<a name="copilot-install-manual"></a>

As an alternative to Homebrew, you can manually install the AWS Copilot CLI on your macOS or Linux system\. Use the following command for your operating system to download the binary, apply execute permissions to it, and then verify it works by listing the help menu\.

------
#### [ macOS ]

For macOS:

```
sudo curl -Lo /usr/local/bin/copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-darwin \
   && sudo chmod +x /usr/local/bin/copilot \
   && copilot --help
```

------
#### [ Linux ]

For Linux x86 \(64\-bit\) systems:

```
sudo curl -Lo /usr/local/bin/copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux \
   && sudo chmod +x /usr/local/bin/copilot \
   && copilot --help
```

For Linux ARM systems:

```
sudo curl -Lo /usr/local/bin/copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux-arm64 \
   && sudo chmod +x /usr/local/bin/copilot \
   && copilot --help
```

------
#### [ Windows ]

Using Powershell, run the following command:

```
PS C:\> New-Item -Path 'C:\copilot' -ItemType directory; `
  Invoke-WebRequest -OutFile 'C:\copilot\copilot.exe' https://github.com/aws/copilot-cli/releases/latest/download/copilot-windows.exe
```

------

### \(Optional\) Verify the AWS Copilot CLI using PGP signatures<a name="ECS_Copilot_validate"></a>

The AWS Copilot CLI executables are cryptographically signed using PGP signatures\. The PGP signatures can be used to verify the validity of the AWS Copilot CLI executable\. Use the following steps to verify the signatures using the GnuPG tool\.

1. Download the AWS Copilot CLI signatures\. The signatures are ASCII detached PGP signatures stored in files with the extension `.asc`\. The signatures file has the same name as its corresponding executable, with `.asc` appended\.

------
#### [ macOS ]

   For macOS systems, run the following command\.

   ```
   sudo curl -Lo copilot.asc https://github.com/aws/copilot-cli/releases/latest/download/copilot-darwin.asc
   ```

------
#### [ Linux ]

   For Linux x86 \(64\-bit\) systems, run the following command\.

   ```
   sudo curl -Lo copilot.asc https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux.asc
   ```

   For Linux ARM systems, run the following command\.

   ```
   sudo curl -Lo copilot.asc https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux-arm64.asc
   ```

------
#### [ Windows ]

   Using Powershell, run the following command\.

   ```
   PS C:\> Invoke-WebRequest -OutFile 'C:\copilot\copilot.asc' https://github.com/aws/copilot-cli/releases/latest/download/copilot-windows.exe.asc
   ```

------

1. Verify the signature with the following command\.
   + For macOS and Linux systems:

     ```
     gpg --verify copilot.asc /usr/local/bin/copilot
     ```

1. For Windows installations, run the following command to add AWS Copilot directory to the path\.

   ```
   e $Env:PATH += ";<path to Copilot executable files>"
   ```

## Next steps<a name="copilot-nextsteps"></a>

After installation, learn how to deploy an Amazon ECS application using AWS Copilot\. For more information, see [Getting started with Amazon ECS using AWS Copilot](getting-started-aws-copilot-cli.md)\.