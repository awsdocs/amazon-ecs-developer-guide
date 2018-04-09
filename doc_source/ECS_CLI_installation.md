# Installing the Amazon ECS CLI<a name="ECS_CLI_installation"></a>

Follow these instructions to install the Amazon ECS CLI on your macOS, Linux, or Windows system\.

## Step 1: Download the Amazon ECS CLI<a name="ECS_CLI_install"></a>

Download the ECS CLI binary\.
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
If you encounter permissions issues, ensure that you are running PowerShell as an administrator\.

## Step 2: \(Optional\) Verify the Amazon ECS CLI<a name="ECS_CLI_install"></a>

To verify the validity of the Amazon ECS CLI file, you can either use the provided MD5 sum or the PGP signatures\. Both methods are described in the following sections\.

### Verify Using the MD5 Sum<a name="ECS_CLI_verify_md5"></a>

Verify the downloaded binary with the MD5 sum provided\.
+ For macOS \(compare the two output strings to verify that they match\):

  ```
  curl -s https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-darwin-amd64-latest.md5 && md5 -q /usr/local/bin/ecs-cli
  ```
+ For Linux systems \(look for an `OK` in the output string\):

  ```
  echo "$(curl -s https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-linux-amd64-latest.md5) /usr/local/bin/ecs-cli" | md5sum -c -
  ```
+ For Windows systems:

  Open Windows PowerShell and find the md5 hash of the executable that you downloaded:

  ```
  PS C:\> Get-FileHash ecs-cli.exe -Algorithm MD5
  ```

  Compare that with this md5 hash:

  ```
  PS C:\> Invoke-WebRequest -OutFile md5.txt https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-windows-amd64-latest.md5
  PS C:\> Get-Content md5.txt
  ```

### Verify Using the PGP Signature<a name="ECS_CLI_verify_pgp"></a>

The ECS CLI executables are cryptographically signed using PGP signatures\. You can use the following steps to verify the signatures using the GnuPG tool\.

1. Download and install GnuPG\. For more information about GNUpg, see the [GnuPG website](https://www.gnupg.org)\.
   + For macOS, we recommend using Homebrew\. Install Homebrew using the instructions from their website\. For more information, see [Homebrew](https://brew.sh/)\. After Homebrew is installed, use the following command from your macOS terminal:

     ```
     brew install gnupg
     ```
   + For Linux systems, install `gpg` using the package manager on your flavor of Linux\.
   + For Windows systems, download and use the Windows simple installer from the GnuPG website\. For more information, see [GnuPG Download](https://www.gnupg.org/download/index.html)\.

1. Retrieve the Amazon ECS PGP public key\. You can use a command to do this or manually create the key and then import it\.

   1. Option 1: Retrieve the key with the following command\.

      ```
      gpg --keyserver hkp://keys.gnupg.net --recv BCE9D9A42D51784
      ```

   1. Option 2: Create a file with the following contents of the Amazon ECS PGP public key and then import it:

      ```
      -----BEGIN PGP PUBLIC KEY BLOCK-----
      Version: GnuPG v2.0.22 (GNU/Linux)
      
      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
      =LqgN
      -----END PGP PUBLIC KEY BLOCK-----
      ```

      The details of the Amazon ECS PGP public key for reference:

      ```
      Key ID: 0x2D51784F
      Type: RSA
      Size: 4096/4096
      Expires: Never
      User ID: Amazon ECS
      Key fingerprint: F34C 3DDA E729 26B0 79BE AEC6 BCE9 D9A4 2D51 784F
      ```

      Import the Amazon ECS PGP public key with the following command\.

      ```
      gpg --import <public_key_filename>
      ```

1. Download the ECS CLI signatures\. ECS CLI signatures are ascii detached PGP signatures stored in files with the extension `.asc`\. The signatures file has the same name as its corresponding executable, with `.asc` appended\.
   + For macOS systems:

     ```
     curl -o ecs-cli.asc https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-darwin-amd64-latest.asc
     ```
   + For Linux systems:

     ```
     curl -o ecs-cli.asc https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-linux-amd64-latest.asc
     ```
   + For Windows systems:

     ```
     PS C:\> Invoke-WebRequest -OutFile ecs-cli.asc https://s3.amazonaws.com/amazon-ecs-cli/ecs-cli-windows-amd64-latest.exe.asc
     ```

1. Verify the signature\.
   + For macOS and Linux systems:

     ```
     gpg --verify ecs-cli.asc /usr/local/bin/ecs-cli
     ```
   + For Windows systems:

     ```
     gpg --verify ecs-cli.asc 'C:\Program Files\Amazon\ECSCLI\ecs-cli.exe'
     ```

   Expected output:

   ```
   gpg: Signature made Tue Apr  3 13:29:30 2018 PDT
   gpg:                using RSA key DE3CBD61ADAF8B8E
   gpg: Good signature from "Amazon ECS <ecs-security@amazon.com>" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: F34C 3DDA E729 26B0 79BE  AEC6 BCE9 D9A4 2D51 784F
        Subkey fingerprint: EB3D F841 E2C9 212A 2BD4  2232 DE3C BD61 ADAF 8B8E
   ```
**Note**  
The warning in the output is expected and is not problematic; it occurs because there is not a chain of trust between your personal PGP key \(if you have one\) and the Amazon ECS PGP key\. For more information, see [Web of trust](https://en.wikipedia.org/wiki/Web_of_trust)\.

## Step 3: Apply Execute Permissions to the Binary<a name="ECS_CLI_install_execute"></a>

Apply execute permissions to the binary\.
+ For macOS and Linux systems:

  ```
  sudo chmod +x /usr/local/bin/ecs-cli
  ```
+ For Windows systems:

  Edit the environment variables and add `C:\Program Files\Amazon\ECSCLI` to the `PATH` variable field, separated from existing entries by using a semicolon\. For example:

  ```
  C:\existing\path;C:\Program Files\Amazon\ECSCLI
  ```

  Restart PowerShell \(or the command prompt\) so the changes go into effect\.
**Note**  
Once the `PATH` variable is set, the ECS CLI can be used from either Windows PowerShell or the command prompt\.

## Step 4: Complete the Installation<a name="ECS_CLI_install_verify"></a>

Verify that the CLI is working properly\.

```
ecs-cli --version
```

Proceed to [Configuring the Amazon ECS CLI](ECS_CLI_Configuration.md)\.

**Important**  
You must configure the ECS CLI with your AWS credentials, an AWS region, and an Amazon ECS cluster name before you can use it\.