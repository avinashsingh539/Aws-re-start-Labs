# Lab 277 — Systems Hardening with AWS Systems Manager Patch Manager

## 📌 Overview

This lab demonstrates how to manage operating system security updates for Linux and Windows EC2 instances using **AWS Systems Manager Patch Manager**. It covers patching Linux instances with a default patch baseline, creating a custom Windows patch baseline, associating patch groups, executing patch operations, and verifying patch compliance.

## 🎯 Objectives

- Patch Linux EC2 instances using the default patch baseline.
- Create a custom patch baseline for Windows Server 2019.
- Configure patch approval rules for security updates.
- Use patch groups to target Windows instances.
- Verify patch compliance across all six EC2 instances.

## 🛠️ AWS Services Used

- **Amazon EC2:** Hosts the Linux and Windows instances.
- **AWS Systems Manager:** Provides centralized management of managed instances.
- **Patch Manager:** Scans instances and installs approved patches.
- **Fleet Manager:** Displays managed instance information.
- **Run Command:** Executes patch operations on target instances.
- **Patch compliance reporting:** Displays patch compliance status and installation details.

## 🏗️ Lab Environment

The lab environment contains six preconfigured EC2 instances:

| Operating system | Instances | Patch group |
|---|---|---|
| Linux | `Linux-1`, `Linux-2`, `Linux-3` | `LinuxProd` |
| Windows | `Windows-1`, `Windows-2`, `Windows-3` | `WindowsProd` |

The instances are configured with IAM permissions that allow AWS Systems Manager to manage them.

## 🚀 Implementation Steps

### Task 1: Patch Linux Instances Using the Default Baseline

1. Open the AWS Management Console and navigate to **Systems Manager → Fleet Manager**.
2. Select `Linux-1` and choose **Node actions → View details** to inspect its operating system and IAM role.
3. Navigate to **Systems Manager → Patch Manager**.
4. Choose **Patch now**.
5. Configure the patch operation as follows:

| Setting | Value |
|---|---|
| Patching operation | `Scan and install` |
| Reboot option | `Reboot if needed` |
| Instance selection | `Patch only the target instances I specify` |
| Target selection | `Specify instance tags` |
| Tag key | `Patch Group` |
| Tag value | `LinuxProd` |

6. Click **Add → Patch now**.
7. Monitor the execution until the patch operation completes for all three Linux instances.

The default Amazon Linux 2 patch baseline is used to determine which patches are applicable and approved for installation.

### Task 2: Create a Custom Windows Patch Baseline

Navigate to **Systems Manager → Patch Manager → Patch baselines → Create patch baseline**.

Configure the baseline details:

| Setting | Value |
|---|---|
| Name | `WindowsServerSecurityUpdates` |
| Description | `Windows security baseline patch` |
| Operating system | `Windows` |
| Default patch baseline | Unchecked |

#### Configure approval rules

Create the following two rules:

| Configuration | Rule 1 | Rule 2 |
|---|---|---|
| Product | `WindowsServer2019` | `WindowsServer2019` |
| Severity | `Critical` | `Important` |
| Classification | `SecurityUpdates` | `SecurityUpdates` |
| Auto-approval delay | 3 days | 3 days |
| Compliance reporting | `Critical` | `High` |

For each rule, select `WindowsServer2019` and deselect `All` under Products.

Choose **Add rule** to configure the second rule, then select **Create patch baseline**.

#### Associate the patch group

1. Select `WindowsServerSecurityUpdates` from the patch baselines list.
2. Choose **Actions → Modify patch groups**.
3. Enter `WindowsProd`.
4. Click **Add → Close**.

This associates the custom baseline with the Windows production patch group.

### Task 3: Tag and Patch Windows Instances

#### Step 1: Apply instance tags

Navigate to **EC2 → Instances**.

For each of `Windows-1`, `Windows-2`, and `Windows-3`:

1. Select the instance and open the **Tags** tab.
2. Choose **Manage tags → Add new tag**.
3. Enter the following values:

| Tag field | Value |
|---|---|
| Key | `Patch Group` |
| Value | `WindowsProd` |

4. Click **Save**.

#### Step 2: Execute the Windows patch operation

Navigate to **Systems Manager → Patch Manager → Patch now**.

Configure the operation:

| Setting | Value |
|---|---|
| Patching operation | `Scan and install` |
| Reboot option | `Reboot if needed` |
| Instance selection | `Patch only the target instances I specify` |
| Target selection | `Specify instance tags` |
| Tag key | `Patch Group` |
| Tag value | `WindowsProd` |

Click **Add → Patch now**.

#### Step 3: Inspect patch execution output

1. Open the generated **Execution ID**.
2. Locate a managed instance with an `InProgress` status.
3. Select its **Output** link.
4. Expand the Output panel.
5. Inspect the execution details and look for `PatchGroup: WindowsProd`.

Patch Manager uses Systems Manager Run Command and the `RunPatchBaseline` document to evaluate and apply patches to the selected instances.

### Task 4: Verify Patch Compliance

Navigate to **Systems Manager → Patch Manager → Dashboard**.

1. Review the **Compliance summary**.
2. Verify that the expected result is `Compliant: 6`.
3. Open the **Compliance reporting** tab.
4. Confirm that all three Linux and all three Windows instances show a `Compliant` status.
5. Review the node patching details, including:
   - Critical noncompliant count
   - Security noncompliant count
   - Other noncompliant count
   - Last operation date
   - Baseline ID
6. Select a Windows node ID and open its **Patch** tab.
7. Inspect the patches applied to the instance and their installation times.

## 🔑 Key Concepts Learned

| Concept | Description |
|---|---|
| Patch management | Process of scanning, approving, and installing software updates. |
| Patch baseline | Defines the patches approved for installation according to configured rules. |
| Default baseline | AWS-provided baseline used without creating a custom approval configuration. |
| Custom baseline | User-defined baseline that provides greater control over patch approval. |
| Patch groups | Logical groups of instances associated with a patch baseline using tags. |
| Instance tagging | Enables targeting instances by key-value pairs rather than selecting each instance individually. |
| Run Command | Executes commands and Systems Manager documents on managed instances. |
| Patch compliance | Indicates whether instances meet the configured patch requirements. |
| Auto-approval delay | Specifies how long to wait before patches matching a rule are automatically approved. |

## ✅ Expected Results

After successful completion:

- Three Linux instances are patched using the default patch baseline.
- The custom `WindowsServerSecurityUpdates` baseline is created.
- Critical and Important Windows security update approval rules are configured.
- The `WindowsProd` patch group is associated with the custom baseline.
- Three Windows instances are tagged and patched.
- All six instances appear as compliant in Patch Manager's compliance reporting.

## 🏁 Conclusion

This lab demonstrates how AWS Systems Manager Patch Manager simplifies operating system patching across Linux and Windows EC2 instances. Default and custom patch baselines, tag-based patch groups, automated execution, and compliance reporting provide a structured approach to maintaining system security and reducing the risks associated with outdated software.

---

**Lab:** 277 — Systems Hardening  
**Primary AWS Service:** AWS Systems Manager Patch Manager  
**Related Services:** Amazon EC2, Fleet Manager, Run Command  
**Focus:** OS Patching, Custom Baselines, Patch Groups, and Compliance Verification
