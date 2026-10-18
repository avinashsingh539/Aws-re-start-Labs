# 225-[LX]-Lab — Introduction to Amazon Linux AMI

## Overview

This lab introduces Amazon Linux running on an Amazon EC2 instance. It focuses on connecting to a Linux Command Host through SSH and learning how to use Linux manual pages for command documentation and search.

## Objectives

- Connect to an Amazon Linux EC2 instance using SSH.
- Explore Linux manual pages using the `man` command.
- Understand manual-page sections and command options.
- Practice navigating and searching documentation.

## Environment

| Component | Details |
|---|---|
| Cloud Platform | AWS Academy / Vocareum |
| AWS Service | Amazon EC2 |
| Operating System | Amazon Linux 2 |
| Region | `us-west-2` |
| Client | Windows PowerShell |
| Connection | OpenSSH over SSH |

## Procedure

### 1. Verify AWS Credentials

Verify that the AWS CLI is authenticated:

```powershell
aws sts get-caller-identity
```

### 2. Connect to the EC2 Command Host

Download the SSH private key in PEM format from **Vocareum → AWS Details → SSH Key → Download PEM**.

Connect using PowerShell:

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the Command Host's current public IP address.

A successful connection displays the Amazon Linux banner and a shell prompt similar to:

```text
[ec2-user@ip-10-0-10-223 ~]$
```

### 3. Explore Linux Manual Pages

Open the manual for the `man` command:

```bash
man man
```

Open the manual for the `ls` command:

```bash
man ls
```

The `ls` manual documents how to list files and directories and explains its supported options.

### 4. Navigate and Search Manual Pages

While viewing a manual page, use the following controls:

| Key | Function |
|---|---|
| `↑` / `↓` | Navigate through the page |
| `Space` | Move to the next screen |
| `/DESCRIPTION` | Search for the specified term |
| `n` | Find the next match |
| `q` | Exit the manual page |

Inspect available sections such as `NAME`, `SYNOPSIS`, `DESCRIPTION`, `OPTIONS`, and `SEE ALSO`. The sections present depend on the manual page.

## Key Learnings

- Connected to an EC2 Linux instance using SSH from Windows PowerShell.
- Learned the role of PEM private keys in SSH authentication.
- Used `man` to access Linux command documentation.
- Practiced searching and navigating manual pages.
- Explored command syntax, descriptions, and options.

## Conclusion

Successfully connected to the Amazon Linux 2 Command Host and explored Linux manual pages. The lab provided practical experience with SSH-based remote access and built-in Linux documentation tools.

**Status:** Completed