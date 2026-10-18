# AWS EC2 Lab 01 — Introduction to Amazon EC2

A step-by-step guide to completing the **Introduction to Amazon EC2** lab using AWS CLI on Windows PowerShell, without performing the main lab tasks through the AWS Management Console.

## Table of Contents

1. [Lab Overview](#1-lab-overview)
2. [Prerequisites](#2-prerequisites)
3. [Configure AWS CLI](#3-configure-aws-cli)
4. [Find the Lab VPC and Subnet](#4-find-the-lab-vpc-and-subnet)
5. [Create the Security Group](#5-create-the-security-group)
6. [Find the Amazon Linux AMI](#6-find-the-amazon-linux-ami)
7. [Prepare Apache User Data](#7-prepare-apache-user-data)
8. [Launch the EC2 Instance](#8-launch-the-ec2-instance)
9. [Verify the Instance](#9-verify-the-instance)
10. [Configure HTTP Access](#10-configure-http-access)
11. [Test the Web Server](#11-test-the-web-server)
12. [Stop and Resize the Instance](#12-stop-and-resize-the-instance)
13. [Increase the EBS Volume](#13-increase-the-ebs-volume)
14. [Start and Verify the Instance](#14-start-and-verify-the-instance)
15. [Test Termination Protection](#15-test-termination-protection)
16. [Terminate the Instance](#16-terminate-the-instance)
17. [Submit the Lab](#17-submit-the-lab)
18. [Troubleshooting](#18-troubleshooting)

---

## 1. Lab Overview

In this lab, you will launch and manage an Amazon EC2 virtual server, configure a web server, and perform instance maintenance operations.

| Configuration          | Required value              |
| ---------------------- | --------------------------- |
| Instance name          | `Web Server`                |
| Operating system       | Amazon Linux 2023           |
| Initial instance type  | `t3.micro`                  |
| Updated instance type  | `t3.small`                  |
| VPC                    | `Lab VPC`                   |
| Security group         | `Web Server security group` |
| Initial root volume    | 8 GiB                       |
| Updated root volume    | 10 GiB                      |
| Web server             | Apache HTTP Server          |
| HTTP port              | TCP 80                      |
| Termination protection | Enabled initially           |

**Important:** This guide is designed for the AWS Vocareum lab environment. Start the lab first and use its temporary AWS credentials. Resource IDs will differ between sessions.

## 2. Prerequisites

Install or verify:

- Windows PowerShell
- AWS CLI v2
- An active AWS Vocareum lab session

Check the AWS CLI installation:

```powershell
aws --version
```

You should see output beginning with `aws-cli/2`.

## 3. Configure AWS CLI

Configure the temporary credentials provided by your active Vocareum lab using your preferred secure method.

If using the interactive configuration command:

```powershell
aws configure
```

Enter the access key and secret access key provided by the lab. Use the appropriate region, typically `us-west-2` for this lab. If Vocareum provides a session token, configure it as well using the method supported by your lab.



Set the region:

```powershell
$Region = "us-west-2"
```

Verify authentication:

```powershell
aws sts get-caller-identity
```

Verify the configured region:

```powershell
aws configure get region
```

Continue only if AWS authentication succeeds.

## 4. Find the Lab VPC and Subnet

Find the VPC named `Lab VPC`:

```powershell
aws ec2 describe-vpcs `
  --region $Region `
  --filters "Name=tag:Name,Values=Lab VPC" `
  --query "Vpcs[*].[VpcId,State,CidrBlock]" `
  --output table
```

Copy the VPC ID from the output and set it below.

Replace `vpc-REPLACE_ME` with your actual VPC ID.

```powershell
$VpcId = "vpc-REPLACE_ME"
```

For example, an ID may look like `vpc-0123456789abcdef0`. Use the ID returned by your own lab, not this example.

List the subnets belonging to the VPC:

```powershell
aws ec2 describe-subnets `
  --region $Region `
  --filters "Name=vpc-id,Values=$VpcId" `
  --query "Subnets[*].[SubnetId,AvailabilityZone,MapPublicIpOnLaunch]" `
  --output table
```

Choose an appropriate public subnet from the output. For this lab, select a subnet that supports public IP assignment, where `MapPublicIpOnLaunch` is `True`.

Set the selected subnet ID:

```powershell
$SubnetId = "subnet-REPLACE_ME"
```

## 5. Create the Security Group

Create the security group with the required name and description.

```powershell
$SG = aws ec2 create-security-group `
  --region $Region `
  --group-name "Web Server security group" `
  --description "Security group for my web server" `
  --vpc-id $VpcId `
  --query "GroupId" `
  --output text
```

Display the generated security group ID:

```powershell
$SG
```

The result should begin with `sg-`.

The new security group initially has no inbound rules, as required by the lab.

## 6. Find the Amazon Linux AMI

Retrieve the latest Amazon Linux 2023 x86-64 AMI ID through AWS Systems Manager Parameter Store:

```powershell
$AMI = aws ssm get-parameter `
  --region $Region `
  --name "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64" `
  --query "Parameter.Value" `
  --output text
```

Display the AMI ID:

```powershell
$AMI
```

The output should begin with `ami-`.

## 7. Prepare Apache User Data

The following script installs and starts Apache and creates the webpage required by the lab.

**Do not execute these Linux commands directly in PowerShell.** They must be saved as User data and executed on the Amazon Linux instance.

Create the User data file:

```powershell
@'
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
'@ | Set-Content -Path "$HOME\userdata.txt" -Encoding ascii
```

The file is saved as:

```text
%USERPROFILE%\userdata.txt
```

## 8. Launch the EC2 Instance

Select the public subnet you identified earlier and launch the instance.

```powershell
$Launch = aws ec2 run-instances `
  --region $Region `
  --image-id $AMI `
  --instance-type t3.micro `
  --subnet-id $SubnetId `
  --security-group-ids $SG `
  --user-data "file://$HOME\userdata.txt" `
  --disable-api-termination `
  --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]" `
  --query "Instances[0].InstanceId" `
  --output text
```

Display the instance ID:

```powershell
$Launch
```

The instance ID should begin with `i-`.

Set the exact required instance name:

```powershell
aws ec2 create-tags `
  --region $Region `
  --resources $Launch `
  --tags "Key=Name,Value=Web Server"
```

This launches the instance with:

- Amazon Linux 2023
- Instance type `t3.micro`
- The selected Lab VPC subnet
- Your newly created security group
- Apache installation through User data
- API termination protection enabled

## 9. Verify the Instance

Check the instance state, type, public IP address, and termination-protection attribute reported by the instance description:

```powershell
aws ec2 describe-instances `
  --region $Region `
  --instance-ids $Launch `
  --query "Reservations[0].Instances[0].[InstanceId,State.Name,InstanceType,PublicIpAddress]" `
  --output table
```

Wait for the instance to reach the running state:

```powershell
aws ec2 wait instance-running `
  --region $Region `
  --instance-ids $Launch
```

Wait for both EC2 status checks to pass:

```powershell
aws ec2 wait instance-status-ok `
  --region $Region `
  --instance-ids $Launch
```

Verify the status checks:

```powershell
aws ec2 describe-instance-status `
  --region $Region `
  --instance-ids $Launch `
  --include-all-instances `
  --query "InstanceStatuses[*].[InstanceState.Name,SystemStatus.Status,InstanceStatus.Status]" `
  --output table
```

Expected result after initialization:

- Instance state: `running`
- System status: `ok`
- Instance status: `ok`

## 10. Configure HTTP Access

The security group initially has no inbound rules. Add an inbound rule for HTTP on TCP port 80:

```powershell
aws ec2 authorize-security-group-ingress `
  --region $Region `
  --group-id $SG `
  --protocol tcp `
  --port 80 `
  --cidr 0.0.0.0/0
```

The successful response includes:

```text
"Return": true
```

This rule makes the web server publicly accessible over HTTP. It is appropriate for this lab exercise; production servers should use more restrictive network access where possible.

## 11. Test the Web Server

Retrieve the current public IP address:

```powershell
$PublicIP = aws ec2 describe-instances `
  --region $Region `
  --instance-ids $Launch `
  --query "Reservations[0].Instances[0].PublicIpAddress" `
  --output text
```

Wait briefly for Apache to finish initializing:

```powershell
Start-Sleep -Seconds 30
```

Test the webpage:

```powershell
Invoke-WebRequest `
  -Uri "http://$PublicIP" `
  -TimeoutSec 30 `
  -UseBasicParsing
```

Expected response:

```text
StatusCode        : 200
StatusDescription : OK
Content           : <html><h1>Hello From Your Web Server!</h1></html>
```

The HTTP status code `200` confirms that the web server is responding successfully.

## 12. Stop and Resize the Instance

Stop the instance:

```powershell
aws ec2 stop-instances `
  --region $Region `
  --instance-ids $Launch
```

Wait until the instance has stopped:

```powershell
aws ec2 wait instance-stopped `
  --region $Region `
  --instance-ids $Launch
```

Change the instance type to `t3.small`:

```powershell
aws ec2 modify-instance-attribute `
  --region $Region `
  --instance-id $Launch `
  --instance-type Value=t3.small
```

Verify the instance type:

```powershell
aws ec2 describe-instances `
  --region $Region `
  --instance-ids $Launch `
  --query "Reservations[0].Instances[0].[InstanceId,State.Name,InstanceType]" `
  --output table
```

Expected instance type: `t3.small`.

The instance should remain stopped until the storage modification is complete.

## 13. Increase the EBS Volume

Find the volume ID attached to the root device:

```powershell
aws ec2 describe-instances `
  --region $Region `
  --instance-ids $Launch `
  --query "Reservations[0].Instances[0].BlockDeviceMappings[*].[DeviceName,Ebs.VolumeId]" `
  --output table
```

Identify the root device, typically `/dev/xvda`, and copy its EBS volume ID.

Set the volume ID:

```powershell
$VolumeId = "vol-REPLACE_ME"
```

Replace the placeholder with the actual `vol-...` ID from your output.

Increase the volume size to 10 GiB:

```powershell
aws ec2 modify-volume `
  --region $Region `
  --volume-id $VolumeId `
  --size 10
```

Verify the volume:

```powershell
aws ec2 describe-volumes `
  --region $Region `
  --volume-ids $VolumeId `
  --query "Volumes[*].[VolumeId,Size,State]" `
  --output table
```

Expected size: `10`.

The volume state may initially be `modifying`. Check again until the modification reaches an appropriate completed state before proceeding.

## 14. Start and Verify the Instance

Start the instance:

```powershell
aws ec2 start-instances `
  --region $Region `
  --instance-ids $Launch
```

Wait for it to become running:

```powershell
aws ec2 wait instance-running `
  --region $Region `
  --instance-ids $Launch
```

Wait for the status checks:

```powershell
aws ec2 wait instance-status-ok `
  --region $Region `
  --instance-ids $Launch
```

Verify the final instance type:

```powershell
aws ec2 describe-instances `
  --region $Region `
  --instance-ids $Launch `
  --query "Reservations[0].Instances[0].[InstanceId,State.Name,InstanceType]" `
  --output table
```

Expected result:

```text
running
t3.small
```

## 15. Test Termination Protection

Check the instance's termination-protection setting:

```powershell
aws ec2 describe-instance-attribute `
  --region $Region `
  --instance-id $Launch `
  --attribute disableApiTermination
```

Expected value:

```text
"Value": true
```

Attempt to terminate the instance while protection is enabled:

```powershell
aws ec2 terminate-instances `
  --region $Region `
  --instance-ids $Launch
```

**Expected result:** AWS rejects the operation with an `OperationNotPermitted` error stating that termination protection must be disabled.

This error is expected and demonstrates that termination protection is functioning correctly.

## 16. Terminate the Instance

Disable termination protection:

```powershell
aws ec2 modify-instance-attribute `
  --region $Region `
  --instance-id $Launch `
  --no-disable-api-termination
```

Verify the setting:

```powershell
aws ec2 describe-instance-attribute `
  --region $Region `
  --instance-id $Launch `
  --attribute disableApiTermination
```

Confirm that the value is now `false`.

Terminate the instance:

```powershell
aws ec2 terminate-instances `
  --region $Region `
  --instance-ids $Launch
```

Wait until termination completes:

```powershell
aws ec2 wait instance-terminated `
  --region $Region `
  --instance-ids $Launch
```

**Warning:** Termination is destructive. Confirm that you have finished all required lab checks before executing these commands.

## 17. Submit the Lab

Return to the Vocareum lab page and submit the assessment if required.

Verify that the lab reports completion and awards the expected marks.

---

## 18. Troubleshooting

### `yum` or `systemctl` is not recognized

These are Linux commands. Put them in the EC2 User data script instead of running them directly in Windows PowerShell.

### No subnets are returned

Check that `$VpcId` contains the actual VPC ID rather than a placeholder. Then list subnets using the VPC ID filter.

### `InvalidParameterValue` for a volume ID

Make sure `$VolumeId` contains the real ID returned by `describe-instances`, not `vol-REPLACE_ME`.

### `Unknown options` while changing the instance type

Use this PowerShell-compatible command:

```powershell
aws ec2 modify-instance-attribute `
  --region $Region `
  --instance-id $Launch `
  --instance-type Value=t3.small
```

### HTTP request times out

Check that:

1. The instance is running.
2. Both EC2 status checks pass.
3. The security group has an inbound TCP port 80 rule.
4. The instance has a public IP address.
5. Apache has finished initializing.

### `ExpiredToken` or authentication errors

Vocareum credentials are temporary. Refresh the credentials using the active lab session and retry the relevant command.

### Resource name already exists

If the security group creation fails because the name already exists in the VPC, inspect existing resources before creating anything else. Avoid repeatedly launching instances or creating duplicate resources.

---

## Completion Checklist

- [ ] AWS CLI authentication verified.
- [ ] Correct Lab VPC and public subnet selected.
- [ ] Security group created with no initial inbound rules.
- [ ] Amazon Linux 2023 EC2 instance launched.
- [ ] Instance named `Web Server`.
- [ ] Termination protection enabled.
- [ ] Apache User data executed successfully.
- [ ] HTTP inbound rule added on port 80.
- [ ] Webpage returned HTTP 200.
- [ ] Instance type changed to `t3.small`.
- [ ] Root EBS volume increased to 10 GiB.
- [ ] Instance restarted and verified.
- [ ] Termination protection tested and disabled.
- [ ] Instance terminated.
- [ ] Vocareum lab submitted successfully.

**Lab result:** Record your own score after completing the assessment.

**Note:** AWS CLI commands and resource IDs can vary across lab versions. Always follow the active lab instructions if they differ from this guide.
