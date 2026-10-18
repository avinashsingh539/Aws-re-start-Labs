 
# Lab 279 — Introduction to AWS Identity and Access Management (IAM)

## 📌 Overview

This lab explores **AWS Identity and Access Management (IAM)**, a service that securely controls access to AWS resources. IAM allows administrators to manage users, organize users into groups, and assign permissions using policies.

The lab demonstrates how to implement a password policy, inspect existing IAM policies, assign users to groups according to their job responsibilities, and verify how permissions control access to AWS services.

## 🎯 Objectives

- Create and apply an account-level password policy.
- Explore IAM users and user groups.
- Inspect AWS-managed and customer inline policies.
- Assign IAM users to groups based on job responsibilities.
- Locate and use the IAM user sign-in URL.
- Test how IAM permissions affect access to Amazon EC2 and Amazon S3.

## 🛠️ AWS Services Used

- **AWS Identity and Access Management (IAM)** — Manages identities, authentication, and permissions.
- **Amazon EC2** — Used to test read-only and administrative permissions for virtual machine instances.
- **Amazon S3** — Used to test permissions for listing buckets and viewing objects.

## 🔐 Task 1: Create an Account Password Policy

Navigated to **IAM → Account settings → Change password policy** and configured stronger password requirements.

### Configuration

| Setting | Configuration |
|---|---|
| Minimum password length | 10 characters |
| Password complexity | Enabled all required character-type options |
| Password expiration | 90 days |
| Password reuse prevention | Last 5 passwords |
| Administrator reset on expiration | Not enabled |

**Result:** Applied a stronger account-level password policy to improve password security for IAM users.

## 👥 Task 2: Explore IAM Users and User Groups

Inspected the pre-created IAM users and groups and reviewed their associated permissions.

### IAM Users

| User | Initial permissions |
|---|---|
| `user-1` | No permissions assigned directly |
| `user-2` | No permissions assigned directly |
| `user-3` | No permissions assigned directly |

### IAM User Groups and Policies

| Group | Attached policy | Purpose |
|---|---|---|
| `S3-Support` | `AmazonS3ReadOnlyAccess` | Read-only access to Amazon S3 |
| `EC2-Support` | `AmazonEC2ReadOnlyAccess` | Read-only access to Amazon EC2 and related resources covered by the policy |
| `EC2-Admin` | `EC2-Admin-Policy` (inline policy) | View EC2 information and start or stop EC2 instances |

### Types of IAM Policies Explored

- **AWS-managed policy:** A reusable policy maintained by AWS, such as `AmazonS3ReadOnlyAccess`.
- **Customer inline policy:** A policy embedded directly into a particular IAM identity, such as the `EC2-Admin-Policy` used in this lab.

### Basic IAM Policy Structure

An IAM policy contains statements that define permissions.

| Element | Meaning |
|---|---|
| `Effect` | Specifies whether an action is `Allow` or `Deny` |
| `Action` | Defines the AWS API actions covered by the statement |
| `Resource` | Specifies the AWS resources to which the statement applies |

These elements determine which operations an identity can perform and on which resources.

## 🔗 Task 3: Assign Users to Groups

Added the pre-created IAM users to groups according to the business scenario.

| IAM user | Assigned group | Intended permissions |
|---|---|---|
| `user-1` | `S3-Support` | Read-only Amazon S3 access |
| `user-2` | `EC2-Support` | Read-only Amazon EC2 access |
| `user-3` | `EC2-Admin` | View, start, and stop EC2 instances |

### Key Concept: Group-Based Permissions

IAM users inherit permissions from the policies attached to their groups. This approach simplifies permission management because administrators can assign users to appropriate groups instead of configuring the same permissions individually for every user.

## 🧪 Task 4: Test IAM User Permissions

Used the IAM user sign-in URL to sign in as each user in a private browser window and test their access.

### Permission Testing Results

| Test | Expected result | Reason |
|---|---|---|
| `user-1` lists S3 buckets and views bucket contents | Allowed | Member of `S3-Support` |
| `user-1` accesses EC2 instances | Denied | No applicable EC2 permissions |
| `user-2` views EC2 instances | Allowed | Member of `EC2-Support` |
| `user-2` attempts to stop an EC2 instance | Denied | Read-only EC2 permissions |
| `user-2` lists S3 buckets | Denied | No applicable S3 permissions |
| `user-3` views EC2 instances | Allowed | Member of `EC2-Admin` |
| `user-3` stops an EC2 instance | Allowed | Inline policy permits the operation |

*These are the expected outcomes described by the lab. Actual results should be confirmed in the AWS console.*

### Understanding Least Privilege

The lab demonstrates the **principle of least privilege**: each user should receive only the permissions required to perform their assigned responsibilities.

- Support personnel can inspect resources without modifying them.
- Administrators receive additional permissions for operational tasks.
- Access to one AWS service does not automatically grant access to another.

## 📚 Key Concepts Learned

### 1. IAM Users
Identities used by people or applications that need AWS access. Users can have credentials and receive permissions through applicable policies.

### 2. IAM Groups
Collections of IAM users that share common permissions through attached policies.

### 3. IAM Policies
Permission documents that specify allowed or denied actions on resources.

### 4. Authentication vs. Authorization
- **Authentication:** Verifies the identity attempting to sign in.
- **Authorization:** Determines what that identity is permitted to do.

### 5. Read-Only vs. Administrative Access
Read-only policies permit viewing supported resources but do not grant modification permissions. Administrative permissions may allow actions such as starting or stopping EC2 instances, subject to the policy's scope.

### 6. Principle of Least Privilege
Grant only the minimum permissions necessary for each user's role to reduce accidental changes and unauthorized access.

## ✅ Conclusion

This lab provided practical experience with AWS IAM user management, group-based access control, password security, and policy evaluation.

By assigning users to `S3-Support`, `EC2-Support`, and `EC2-Admin`, the lab demonstrated how IAM policies enforce different levels of access to AWS resources. Testing each user showed how permissions allow authorized operations while restricting unauthorized actions.

**Main takeaway:** Secure AWS environments require strong authentication, carefully designed policies, and permissions aligned with each user's responsibilities.

---

*Lab: 279 — Introduction to Identity and Access Management (IAM)*  
*Platform: AWS Academy / Vocareum*
