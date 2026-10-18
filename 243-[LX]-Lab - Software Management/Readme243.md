# 243-[LX]-Lab - Software Management


## 📌 Overview

This lab focuses on Linux package management, updating system software, rolling back package transactions, installing the AWS Command Line Interface (AWS CLI), and interacting with Amazon EC2 through CLI commands.

## 🎯 Objectives

- Check for available Linux package updates.
- Apply security updates and upgrade installed packages.
- Inspect package transaction history using YUM.
- Roll back a package installation transaction.
- Install and verify AWS CLI v2.
- Configure AWS CLI credentials and query EC2 instance attributes.

## 🛠️ Technologies Used

- **Amazon EC2** — Cloud-based Linux instance
- **Amazon Linux / Red Hat-based Linux** — Operating system environment
- **YUM** — Package management
- **AWS CLI v2** — Command-line interface for AWS services
- **SSH** — Remote server access
- **Nano** — Terminal-based text editor

## 🚀 Tasks Performed

### 1. Connect to the EC2 Instance

Connected to the Linux instance using SSH and the lab-provided key.

```bash
ssh -i labsuser.pem ec2-user@<PUBLIC_IP>
```

### 2. Update the Linux Machine

Checked the current directory and navigated to the `companyA` directory when necessary.

Checked for available package updates:

```bash
sudo yum -y check-update
```

Applied security updates:

```bash
sudo yum update --security
```

Upgraded installed packages:

```bash
sudo yum -y upgrade
```

Installed the Apache HTTP Server package to create a package transaction for the rollback exercise:

```bash
sudo yum install httpd -y
```

**Key Learning:** Package managers simplify software maintenance, security updates, and dependency management on Linux systems.

### 3. Roll Back a Package Transaction

Reviewed the package transaction history:

```bash
sudo yum history list
```

Inspected the relevant transaction:

```bash
sudo yum history info <TRANSACTION_ID>
```

Undid the transaction associated with installing Apache:

```bash
sudo yum -y history undo <TRANSACTION_ID>
```

**Key Learning:** YUM transaction history helps administrators inspect previous package operations and reverse supported changes when necessary.

### 4. Install and Verify AWS CLI v2

Checked the installed Python and pip versions:

```bash
python3 --version
pip3 --version
```

Downloaded the AWS CLI v2 installer:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

Extracted the installer and installed AWS CLI:

```bash
unzip awscliv2.zip
sudo ./aws/install
```

Verified the installation:

```bash
aws --version
```

Opened the AWS CLI help documentation:

```bash
aws help
```

**Key Learning:** AWS CLI enables users to manage and inspect AWS resources directly from a terminal.

### 5. Configure AWS CLI and Query EC2

Started the configuration process:

```bash
aws configure
```

Configured the default region as `us-west-2` and the output format as `json`. Added the temporary lab credentials to the local AWS credentials file.

```bash
nano ~/.aws/credentials
```

The credentials file used the following structure:

```ini
[default]
aws_access_key_id=<ACCESS_KEY_ID>
aws_secret_access_key=<SECRET_ACCESS_KEY>
aws_session_token=<SESSION_TOKEN>
```

Retrieved the Command Host instance ID from the EC2 console and queried its instance type:

```bash
aws ec2 describe-instance-attribute \
  --instance-id <INSTANCE_ID> \
  --attribute instanceType
```

The expected response is JSON containing the instance ID and its instance-type attribute.

**Security Note:** AWS credentials and session tokens must remain private. Never commit credentials to GitHub or include real credentials in documentation.

## 📚 Key Takeaways

- Used YUM to check, update, and upgrade Linux packages.
- Inspected package transaction history and practiced rolling back an installation.
- Installed and verified AWS CLI v2.
- Configured AWS CLI to use lab-provided credentials and a default region.
- Queried EC2 instance attributes using an AWS CLI command.
- Learned the importance of secure credential handling in cloud environments.

## ✅ Conclusion

Completed the Software Management lab workflow covering Linux package updates, package transaction rollback, AWS CLI installation, CLI configuration, and EC2 instance inspection. These skills support Linux administration, software maintenance, and command-line cloud resource management.

---

**Lab:** 243-[LX]-Lab — Software Management  
**Platform:** AWS Academy / Vocareum