# 245-[LX]-Lab - Managing Log Files

## 📌 Overview

This lab focuses on reviewing Linux authentication logs and examining the most recent login information for system users on an Amazon EC2 Linux instance. It introduces essential log inspection commands used in Linux administration, security monitoring, and troubleshooting.

## 🎯 Objectives

- Connect to an Amazon Linux EC2 instance using SSH.
- Inspect secure log files for authentication events and failures.
- Review the last login information of system users.
- Understand how Linux logs support security investigations and auditing.

## 🛠️ Technologies Used

- **Amazon EC2** — Cloud-based Linux environment
- **Amazon Linux** — Operating system
- **SSH** — Secure remote access
- **`less`** — Terminal-based log file viewer
- **`lastlog`** — Utility for reviewing user login information
- **Linux authentication logs** — Records of authentication-related events

## 🚀 Tasks Performed

### 1. Connect to the EC2 Instance

Connected to the Linux instance using SSH and the lab-provided key.

```bash
ssh -i labsuser.pem ec2-user@<PUBLIC_IP>
```

The public IP and SSH key were obtained from the Vocareum lab environment.

### 2. Review Secure Log Files

Verified the current working directory:

```bash
pwd
```

Navigated to the `companyA` directory if required:

```bash
cd companyA
```

Opened the sample secure log file:

```bash
sudo less /tmp/log/secure
```

Reviewed the available authentication-related entries, including failed authentication attempts, source IP addresses, and port information where present.

Pressed `q` to exit the log viewer.

**Key Learning:** Secure logs help administrators investigate authentication failures and review suspicious access attempts.

> **Note:** This lab uses `/tmp/log/secure` as a sample log file. The instructions identify `/var/log/secure` as the usual location on relevant Linux distributions.

### 3. Review User Login Information

Executed the following command:

```bash
sudo lastlog
```

Reviewed the most recent login information displayed for system accounts, including accounts that had never logged in.

**Key Learning:** The `lastlog` utility helps administrators review account login activity and identify accounts that may require further investigation.

## 🔐 Practical Security Applications

Log inspection is useful for:

- **Authentication monitoring:** Investigating repeated failed login attempts.
- **Access auditing:** Reviewing the most recent login information for user accounts.
- **Incident investigation:** Examining available source IP and port information.
- **Account review:** Identifying accounts with no recorded login activity for further assessment.
- **Troubleshooting:** Using authentication records to investigate access problems.

A failed login or an account with no recorded login does not automatically indicate malicious activity. Findings should be evaluated in context.

## 📚 Key Takeaways

- Used SSH to access a Linux instance hosted on Amazon EC2.
- Inspected authentication-related entries with `less`.
- Reviewed user login records using `lastlog`.
- Learned how log files support Linux security monitoring, auditing, and troubleshooting.
- Understood the importance of investigating log entries in context.

## ✅ Conclusion

Completed the Managing Log Files lab by reviewing a sample secure log file and examining user login information on an Amazon EC2 Linux instance. The lab provided practical experience with Linux log inspection and demonstrated how authentication records can support security monitoring and system administration.

---

**Lab:** 245-[LX]-Lab — Managing Log Files  
**Platform:** AWS Academy / Vocareum