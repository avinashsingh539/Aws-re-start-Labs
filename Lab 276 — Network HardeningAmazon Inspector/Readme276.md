# Lab 276 — Network Hardening: Amazon Inspector

## 📌 Overview

This lab demonstrates how to use **Amazon Inspector** to identify software vulnerabilities in AWS Lambda functions and perform remediation. It covers activating Inspector, reviewing security findings, updating a vulnerable Python dependency, and verifying remediation through subsequent scans.

## 🎯 Objectives

- Activate Amazon Inspector and enable vulnerability scanning.
- Identify and investigate software vulnerabilities.
- Understand Common Vulnerabilities and Exposures (CVE).
- Remediate a vulnerable Python package in an AWS Lambda function.
- Verify findings and review Lambda resource coverage.

## 🛠️ AWS Services and Tools

- **Amazon Inspector** — Automated vulnerability assessment.
- **AWS Lambda** — Serverless function containing the vulnerable dependency.
- **NVD (National Vulnerability Database)** — Reference for CVE details and remediation information.
- **Python requirements.txt** — File used to manage application dependencies.

## 🚀 Implementation Steps

### 1. Activate Amazon Inspector

1. Open the AWS Management Console.
2. Search for **Amazon Inspector**.
3. Select **Activate Inspector**.
4. Wait for the Lambda environment coverage to reach 100%, as required by the lab.

### 2. Review Security Findings

1. Navigate to **Findings → All findings**.
2. Review the reported medium-severity findings.
3. Locate the finding:

   `CVE-2023-32681 - requests`

4. Open the finding and follow its NVD link.
5. Review the vulnerability details and recommended remediation.

### 3. Identify the Vulnerable Dependency

The affected Lambda function is named `get-request`.

Its `requirements.txt` initially contains:

```text
requests==2.20.0
```

This line pins the application to a specific version of the Python `requests` package.

### 4. Update the Dependency

1. Open the **AWS Lambda** console.
2. Select the `get-request` function.
3. Open `requirements.txt` in the function's code editor.
4. Replace the existing dependency declaration with:

```text
requests
```

5. Click **Deploy** to apply the code change.

**Note:** Removing the version pin allows dependency resolution to select a version according to the deployment environment and dependency installation process. It does not, by itself, guarantee that the deployed package is patched. Verify the actual resolved version and Inspector findings.

### 5. Verify Remediation

1. Return to **Amazon Inspector → Findings → All findings**.
2. Change the finding status filter from **Active** to **Closed**.
3. Look for `CVE-2023-32681 - requests`.
4. Allow a few minutes for rescanning if the finding has not updated.

A closed finding indicates that Inspector no longer considers that finding active; verify the scan results before concluding that the vulnerability has been resolved.

### 6. Verify Lambda Resource Coverage

1. In Amazon Inspector, open **Resources coverage → Lambda functions**.
2. Locate the `get-request` function.
3. Verify that the **Last scanned** timestamp has updated.

### 7. End the Lab

After completing the verification steps, return to the lab environment and click **End Lab**.

## 🔍 Key Concepts Learned

| Concept | Description |
|---|---|
| Vulnerability assessment | Identifying known security weaknesses in software components. |
| CVE | A standardized identifier for a publicly disclosed cybersecurity vulnerability. |
| Amazon Inspector | Helps discover and track software vulnerabilities in supported AWS resources. |
| Dependency management | Managing external libraries required by an application. |
| Vulnerability remediation | Updating or replacing vulnerable components to address security findings. |
| Security findings | Records of detected issues, their severity, and their current status. |
| Resource coverage | Shows whether supported resources are covered by Inspector scanning. |

## ✅ Expected Outcome

- Amazon Inspector is activated.
- Security findings are reviewed.
- The vulnerable dependency declaration in `get-request` is updated and deployed.
- The finding is checked for a transition from **Active** to **Closed**.
- The Lambda function's last-scanned timestamp is verified.

## 🏁 Conclusion

This lab demonstrates a basic vulnerability-management workflow in AWS: detect a security issue, investigate the affected dependency, apply a code-level change, and verify the result through security scanning.

It highlights the importance of dependency management, continuous vulnerability assessment, and verification after remediation.

---

**Lab:** 276 — Network Hardening  
**Primary Service:** Amazon Inspector  
**Related Service:** AWS Lambda  
**Focus:** Vulnerability Assessment and Remediation
