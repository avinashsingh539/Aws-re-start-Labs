 
# Lab 168 — [JAWS] Activity: Install and Configure the AWS CLI

## 📌 Overview

This lab demonstrates how to install and configure the **AWS Command Line Interface (AWS CLI)** on a Red Hat Linux EC2 instance and use it to interact with AWS Identity and Access Management (IAM).

The activity covers connecting to an EC2 instance through SSH, installing AWS CLI, configuring AWS credentials, verifying IAM access, and retrieving a customer-managed IAM policy document using CLI commands.

## 🎯 Objectives

- Connect to a Red Hat Linux EC2 instance using SSH.
- Install and verify AWS CLI v2.
- Configure AWS CLI credentials and default settings.
- Access IAM resources through the AWS CLI.
- Retrieve an IAM policy document in JSON format.

## 🛠️ AWS Services and Tools

| Service / Tool | Purpose |
|---|---|
| Amazon EC2 | Hosts the Red Hat Linux instance |
| AWS CLI v2 | Interacts with AWS services through terminal commands |
| AWS IAM | Manages identities, permissions, and policies |
| PuTTY | Establishes an SSH connection from Windows |
| SSH | Provides secure remote terminal access |
| `curl` and `unzip` | Download and extract the AWS CLI installer |

## 🏗️ Architecture

```text
Windows 11 Laptop
       |
       | SSH using PuTTY and .ppk key
       v
Red Hat Linux EC2 Instance
       |
       | Install AWS CLI v2
       | Configure AWS credentials
       v
AWS Command Line Interface
       |
       | AWS API requests
       v
AWS IAM
       |
       +--> List IAM users
       |
       +--> List customer-managed policies
       |
       +--> Retrieve policy document
                    |
                    v
             lab_policy.json
```

## 🚀 Implementation Steps

### Task 1: Connect to the Red Hat EC2 Instance

1. Start the lab in Vocareum and wait until the status becomes **Ready**.
2. Open the AWS Management Console.
3. Navigate to **Details → Show** in the lab instructions.
4. Download the `labsuser.ppk` private key and copy the instance's public IP address.
5. Open PuTTY and configure the connection:

   - **Host Name:** `ec2-user@<PUBLIC_IP>`
   - **Port:** `22`
   - **Connection Type:** SSH
   - **Private Key:** Select `labsuser.ppk` under SSH authentication credentials.

6. Click **Open** and accept the host-key prompt when appropriate.

After a successful connection, the Red Hat instance's terminal becomes available.

> Replace `<PUBLIC_IP>` with the actual public IP address provided by the lab. Do not publish private keys or credentials.

### Task 2: Install AWS CLI v2

**Step 1 — Download the installer**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

**Step 2 — Extract the installer**

```bash
unzip -u awscliv2.zip
```

**Step 3 — Install AWS CLI**

```bash
sudo ./aws/install
```

**Step 4 — Verify the installation**

```bash
aws --version
```

Example output:

```text
aws-cli/2.x.x Python/3.x.x Linux/...
```

The exact version and runtime details may differ.

**Step 5 — Explore the CLI help page**

```bash
aws help
```

Press `q` to exit the help page.

### Task 3: Inspect IAM Configuration

In the AWS Management Console:

1. Search for **IAM**.
2. Navigate to **Users → awsstudent**.
3. Open the **Permissions** tab.
4. Expand `lab_policy` and inspect its JSON document.
5. Open **Security credentials** to locate the access key ID.

The policy defines the AWS actions and resources accessible to the IAM identity. Some IAM console permission warnings may appear because of the restricted lab environment.

### Task 4: Configure AWS CLI

In the Red Hat EC2 terminal, execute:

```bash
aws configure
```

Provide the temporary lab credentials and configuration values when prompted.

| Configuration | Value |
|---|---|
| AWS Access Key ID | Vocareum `AccessKey` |
| AWS Secret Access Key | Vocareum `SecretKey` |
| Default region | `us-west-2` |
| Default output format | `json` |

The AWS CLI stores these settings in the user's AWS configuration and credentials files.

- `~/.aws/credentials` — stores access key credentials.
- `~/.aws/config` — stores configuration such as the default region and output format.

**Security note:** Never commit these files, secret access keys, or temporary session credentials to a GitHub repository.

### Task 5: Verify IAM Access

Execute the following command from the Red Hat terminal:

```bash
aws iam list-users
```

The expected response is JSON containing the IAM users visible to the configured identity, including `awsstudent` when permitted.

This command verifies that the AWS CLI can authenticate and make an IAM API request.

### Activity 1 Challenge: Retrieve the IAM Policy Document

The objective is to download the `lab_policy` document as a JSON file using the AWS CLI rather than the AWS Management Console.

**Step 1 — List customer-managed IAM policies**

```bash
aws iam list-policies --scope Local
```

Locate the `lab_policy` entry and note its:

- `Arn`
- `DefaultVersionId`

**Step 2 — Retrieve and save the policy document**

Replace the placeholders with the actual values from the command output.

```bash
aws iam get-policy-version \
  --policy-arn <LAB_POLICY_ARN> \
  --version-id <DEFAULT_VERSION_ID> \
  --query 'PolicyVersion.Document' \
  --output json > lab_policy.json
```

The command retrieves the specified policy version, extracts its policy document, and redirects the JSON output into `lab_policy.json`.

**Step 3 — Verify the output**

```bash
cat lab_policy.json
```

Check that the file exists:

```bash
ls -l lab_policy.json
```

The file should contain the IAM policy document in JSON format.

> If the command returns `AccessDenied`, verify the lab permissions and the ARN and version ID. Do not modify IAM permissions to bypass the restriction.

## ✅ Expected Results

Upon successful completion:

- AWS CLI v2 is installed on the Red Hat EC2 instance.
- The CLI is configured with the temporary credentials and region provided by the lab.
- The `aws iam list-users` command returns IAM user information.
- The customer-managed policy can be retrieved when the necessary permissions are available.
- The policy document is saved as `lab_policy.json`.

These are the expected outcomes; confirm each against the actual lab execution before marking it complete.

## 🧠 Key Learnings

1. **AWS CLI:** Provides command-line access to AWS services and APIs.
2. **SSH:** Enables secure remote access to a Linux server.
3. **IAM:** Controls identities, permissions, and access to AWS resources.
4. **AWS credentials:** Authenticate CLI requests and are separate from the SSH private key used to connect to EC2.
5. **JSON output:** Makes AWS responses easy to inspect and process.
6. **Output redirection:** The `>` operator saves command output to a file.
7. **Least privilege:** AWS identities should receive only the permissions required for their tasks.

## 🧹 Cleanup

After completing the activities:

1. Confirm the required commands and policy retrieval have been attempted.
2. Return to Vocareum.
3. Select **End Lab → Yes** to terminate the lab resources.

## 📚 References

- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)
- [Install or Update AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [IAM CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/iam/index.html)
- [Connect to a Linux EC2 Instance from Windows](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-from-windows.html)
- [Troubleshoot AWS CLI Errors](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-troubleshooting.html)

---

**Lab:** 168 — [JAWS] Activity: Install and Configure the AWS CLI  
**Platform:** AWS Academy / Vocareum  
**Environment:** Red Hat Linux EC2, AWS CLI v2, IAM  
**Primary Outcome:** Install and configure AWS CLI on a remote EC2 instance and retrieve IAM information through CLI commands.
