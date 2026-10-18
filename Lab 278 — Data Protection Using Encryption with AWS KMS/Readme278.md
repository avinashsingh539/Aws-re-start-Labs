# Lab 278 — Data Protection Using Encryption with AWS KMS

## 📌 Overview

This lab demonstrates how to protect sensitive data using **AWS Key Management Service (AWS KMS)** and the AWS Encryption Command Line Interface (CLI). It covers creating a symmetric encryption key, configuring AWS credentials on an EC2 file server, encrypting plaintext files into ciphertext, and decrypting them to recover the original content.

## 🎯 Objectives

- Create a symmetric encryption key using AWS KMS.
- Configure AWS CLI credentials on an EC2 instance.
- Install and configure the AWS Encryption CLI.
- Encrypt sensitive plaintext files.
- Decrypt ciphertext and verify the original content.

## 🛠️ AWS Services and Tools

- **AWS KMS:** Creates and manages cryptographic keys.
- **Amazon EC2:** Hosts the File Server instance.
- **AWS Systems Manager Session Manager:** Provides browser-based access to the EC2 instance.
- **AWS Encryption CLI:** Performs file encryption and decryption.
- **AWS CLI:** Configures credentials and AWS access.

## 🏗️ Lab Environment

The lab uses a preconfigured EC2 instance named `File Server`. An IAM role is attached to the instance to enable management through AWS Systems Manager Session Manager.

A symmetric AWS KMS key is created to encrypt and decrypt sample files containing mock sensitive information.

## 🚀 Implementation Steps

### Task 1: Create an AWS KMS Key

1. Open the AWS Management Console and navigate to **Key Management Service (KMS)**.
2. Choose **Create a key**.
3. Select `Symmetric` as the key type.
4. Configure the following details:

| Setting | Value |
|---|---|
| Key type | `Symmetric` |
| Alias | `MyKMSKey` |
| Description | `Key used to encrypt and decrypt data files.` |
| Key administrator | `voclabs` |
| Key usage permission | `voclabs` |

5. Review the configuration and choose **Finish**.
6. Open the newly created key and copy its Amazon Resource Name (ARN).

The key ARN is used by the AWS Encryption CLI to identify the KMS key during encryption and decryption.

### Task 2: Configure the File Server

#### Step 1: Connect to the EC2 instance

1. Navigate to **EC2 → Instances**.
2. Select `File Server`.
3. Choose **Connect → Session Manager → Connect**.

#### Step 2: Configure AWS CLI

Run:

```bash
cd ~
aws configure
```

Enter the temporary values specified in the lab:

- AWS Access Key ID: `1`
- AWS Secret Access Key: `1`
- Default region: The region shown on the Vocareum AWS Details page
- Default output format: Press Enter

#### Step 3: Update AWS credentials

1. Open **AWS Details** in Vocareum.
2. Choose **Show** next to AWS CLI.
3. Copy the complete `[default]` credentials configuration, including the session token.
4. Open the credentials file:

```bash
vi ~/.aws/credentials
```

5. Press `i` and replace the temporary contents with the copied configuration.
6. Press `Esc`, enter `:wq`, and press Enter.

Verify the file:

```bash
cat ~/.aws/credentials
```

**Security note:** AWS credentials and session tokens are sensitive. Never publish them in a GitHub repository or include them in screenshots.

#### Step 4: Install the AWS Encryption CLI

Run:

```bash
pip3 install aws-encryption-sdk-cli
export PATH=$PATH:/home/ssm-user/.local/bin
```

This installs the encryption utility and adds its executable directory to the current shell's PATH.

### Task 3: Encrypt and Decrypt Data

#### Step 1: Create sample files

Run:

```bash
touch secret1.txt secret2.txt secret3.txt
echo 'TOP SECRET 1!!!' > secret1.txt
cat secret1.txt
mkdir output
```

The `secret1.txt` file contains the sample plaintext that will be encrypted.

#### Step 2: Set the KMS key ARN

Replace the placeholder with the ARN copied from AWS KMS:

```bash
keyArn=<YOUR_KMS_KEY_ARN>
```

#### Step 3: Encrypt the plaintext

Run:

```bash
aws-encryption-cli --encrypt \
  --input secret1.txt \
  --wrapping-keys key=$keyArn \
  --metadata-output ~/metadata \
  --encryption-context purpose=test \
  --commitment-policy require-encrypt-require-decrypt \
  --output ~/output/.
```

**Explanation of important parameters:**

| Parameter | Purpose |
|---|---|
| `--encrypt` | Selects the encryption operation. |
| `--input` | Specifies the plaintext file. |
| `--wrapping-keys` | Identifies the AWS KMS key used to protect the data key. |
| `--metadata-output` | Specifies where encryption metadata is written. |
| `--encryption-context` | Supplies additional contextual information associated with encryption. |
| `--commitment-policy` | Requires key commitment for encryption and decryption. |
| `--output` | Specifies the destination for the encrypted file. |

Check the command's exit status:

```bash
echo $?
```

A value of `0` indicates success.

List the encrypted output:

```bash
ls output
```

Expected filename:

```text
secret1.txt.encrypted
```

View the encrypted file:

```bash
cd output
cat secret1.txt.encrypted
```

The output is ciphertext, which is not directly readable as the original plaintext.

#### Step 4: Decrypt the ciphertext

From inside the `output` directory, run:

```bash
aws-encryption-cli --decrypt \
  --input secret1.txt.encrypted \
  --wrapping-keys key=$keyArn \
  --commitment-policy require-encrypt-require-decrypt \
  --encryption-context purpose=test \
  --metadata-output ~/metadata \
  --max-encrypted-data-keys 1 \
  --buffer \
  --output .
```

List the output files:

```bash
ls
```

Display the decrypted content:

```bash
cat secret1.txt.encrypted.decrypted
```

Expected output:

```text
TOP SECRET 1!!!
```

The decrypted file should contain the same content as the original `secret1.txt` file.

**Important:** Use the same KMS key and matching encryption context during decryption. In this lab, the encryption context is `purpose=test`.

## 🔑 Key Concepts Learned

| Concept | Description |
|---|---|
| Encryption | Converts readable plaintext into ciphertext to protect confidentiality. |
| Decryption | Converts ciphertext back into readable plaintext. |
| Symmetric encryption | Uses the same underlying cryptographic key for encryption and decryption. |
| AWS KMS | Creates and manages cryptographic keys and controls their use. |
| Key ARN | Uniquely identifies an AWS resource, including a KMS key. |
| Encryption context | Additional non-secret contextual information associated with an encryption operation. |
| Ciphertext | The encrypted representation of the original data. |
| Plaintext | The original, readable data before encryption. |
| Key commitment | A security property that binds ciphertext to the data key used to encrypt it. |

## ✅ Expected Results

After completing the lab:

- The symmetric KMS key `MyKMSKey` is created.
- AWS CLI credentials are configured on the File Server.
- The AWS Encryption CLI is installed.
- `secret1.txt` is encrypted into `secret1.txt.encrypted`.
- The encrypted output is verified.
- The ciphertext is decrypted into `secret1.txt.encrypted.decrypted`.
- The recovered content matches the original plaintext.

## 🏁 Conclusion

This lab demonstrates how AWS KMS and the AWS Encryption CLI can be used together to protect sensitive files. By creating a symmetric key, encrypting plaintext into ciphertext, and decrypting it when authorized, the lab illustrates a practical approach to data confidentiality and cryptographic key management in AWS.

---

**Lab:** 278 — Data Protection  
**Primary AWS Service:** AWS Key Management Service (KMS)  
**Related Services and Tools:** Amazon EC2, AWS Systems Manager Session Manager, AWS CLI, AWS Encryption CLI  
**Focus:** Encryption, Decryption, Key Management, and Data Confidentiality
