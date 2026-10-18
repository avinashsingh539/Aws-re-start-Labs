# 233-[LX]-Lab - Working with the File System

## 📌 Overview

This lab provides hands-on experience with Linux file system management using the command line. It focuses on creating directories and files, navigating directory structures, copying files and folders, moving resources, and deleting files and directories.

The lab was performed on an Amazon Linux EC2 Command Host accessed through SSH in the AWS Academy/Vocareum environment.

## 🎯 Objectives

- Create a directory structure using Linux commands.
- Create empty files using `touch`.
- Navigate directories using `cd`.
- List files and directories using `ls`.
- Copy directories recursively using `cp -r`.
- Move and reorganize directories using `mv`.
- Delete files using `rm`.
- Remove empty directories using `rmdir`.
- Inspect a complete directory tree using `ls -laR`.

## 🛠️ Environment and Tools

| Component | Description |
|---|---|
| Operating system | Amazon Linux |
| Cloud platform | Amazon EC2 |
| Lab environment | AWS Academy / Vocareum |
| Remote connection | SSH |
| User | `ec2-user` |
| Interface | Linux terminal |

## 1. Connect to the EC2 Command Host

From Windows PowerShell, connect to the current lab instance using SSH:

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the current Public IP displayed in the Vocareum lab credentials.

**Security note:** Never commit private keys or expose credentials in a public GitHub repository.

## 2. Understand Basic Navigation Commands

Check the current working directory:

```bash
pwd
```

Navigate to the user's home directory:

```bash
cd /home/ec2-user
```

Create the main directory:

```bash
mkdir CompanyA
```

Enter the directory:

```bash
cd CompanyA
```

Create three subdirectories:

```bash
mkdir Finance HR Management
```

List the contents:

```bash
ls
```

Expected output:

```text
Finance  HR  Management
```

### Concepts learned

- `pwd` displays the current working directory.
- `cd` changes the current directory.
- `mkdir` creates directories.
- `ls` lists directory contents.

## 3. Create Files in the HR Directory

Navigate to the HR directory:

```bash
cd HR
```

Create two empty CSV files:

```bash
touch Assessments.csv TrialPeriod.csv
```

Verify the files:

```bash
ls
```

Expected output:

```text
Assessments.csv  TrialPeriod.csv
```

The `touch` command creates an empty file if it does not exist. If the file already exists, it updates its modification timestamp.

## 4. Create Files in the Finance Directory

Navigate from HR to Finance using a relative path:

```bash
cd ../Finance
```

Create the required files:

```bash
touch Salary.csv ProfitAndLossStatements.csv
```

Verify:

```bash
ls
```

Expected output:

```text
Salary.csv  ProfitAndLossStatements.csv
```

The path `../Finance` means to move up one directory level and then enter the `Finance` directory.

## 5. Create Files in the Management Directory

Return to the `CompanyA` directory:

```bash
cd ..
```

Create the files using paths relative to the current directory:

```bash
touch Management/Managers.csv Management/Schedule.csv
```

Verify:

```bash
ls Management
```

Expected output:

```text
Managers.csv  Schedule.csv
```

This demonstrates that a file can be created inside a subdirectory without first navigating into that directory.

## 6. Inspect the Initial Directory Structure

Run:

```bash
ls -laR
```

This command recursively lists the directory contents, including hidden entries and detailed file information.

The initial directory structure is:

```text
CompanyA/
├── Finance/
│   ├── ProfitAndLossStatements.csv
│   └── Salary.csv
├── HR/
│   ├── Assessments.csv
│   └── TrialPeriod.csv
└── Management/
    ├── Managers.csv
    └── Schedule.csv
```

## 7. Copy the Finance Directory into HR

Ensure you are in the `CompanyA` directory:

```bash
cd /home/ec2-user/CompanyA
pwd
```

Copy Finance and all its contents into HR:

```bash
cp -r Finance HR
```

Verify the copied files:

```bash
ls HR/Finance
```

Expected output:

```text
ProfitAndLossStatements.csv  Salary.csv
```

### How `cp -r` works

- `cp` copies files and directories.
- `-r` enables recursive copying of directory contents.
- `Finance` is the source directory.
- `HR` is the destination directory.

The original Finance directory remains in place until it is explicitly removed.

## 8. Delete the Original Finance Directory

First, attempt to remove the directory:

```bash
rmdir Finance
```

The command fails because the directory is not empty. This is expected behavior.

Remove the original files:

```bash
rm Finance/ProfitAndLossStatements.csv Finance/Salary.csv
```

Verify that Finance is empty:

```bash
ls Finance
```

Remove the now-empty directory:

```bash
rmdir Finance
```

Verify the remaining directories:

```bash
ls
```

Expected output:

```text
HR  Management
```

### Difference between `rm` and `rmdir`

- `rm` removes files.
- `rmdir` removes empty directories only.
- `rm -r` can recursively delete a directory and its contents, so it must be used carefully.

## 9. Move Management into HR

Move the Management directory:

```bash
mv Management HR
```

Verify the moved files:

```bash
ls HR/Management
```

Expected output:

```text
Managers.csv  Schedule.csv
```

The `mv` command moves files and directories. When the destination is a directory, the source is placed inside it.

## 10. Create Employees and Move HR Files

Enter the HR directory:

```bash
cd HR
```

Create the Employees directory:

```bash
mkdir Employees
```

Move both CSV files into Employees:

```bash
mv Assessments.csv TrialPeriod.csv Employees
```

Verify the resulting structure:

```bash
ls
ls Employees
```

Expected output for `ls`:

```text
Employees  Finance  Management
```

Expected output for `ls Employees`:

```text
Assessments.csv  TrialPeriod.csv
```

## 11. Verify the Final Directory Structure

Return to the main directory:

```bash
cd /home/ec2-user/CompanyA
```

Display the complete structure:

```bash
ls -laR
```

The final directory structure is:

```text
CompanyA/
└── HR/
    ├── Employees/
    │   ├── Assessments.csv
    │   └── TrialPeriod.csv
    ├── Finance/
    │   ├── ProfitAndLossStatements.csv
    │   └── Salary.csv
    └── Management/
        ├── Managers.csv
        └── Schedule.csv
```

All six CSV files are preserved, but their locations have changed according to the lab requirements.

## 12. Linux Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `pwd` | Display the current working directory |
| `cd directory` | Navigate into a directory |
| `cd ..` | Move to the parent directory |
| `mkdir directory` | Create a directory |
| `touch file.csv` | Create an empty file |
| `ls` | List directory contents |
| `ls -laR` | Recursively list detailed contents, including hidden entries |
| `cp source destination` | Copy a file |
| `cp -r source destination` | Copy a directory recursively |
| `mv source destination` | Move or rename a file or directory |
| `rm file` | Delete a file |
| `rmdir directory` | Delete an empty directory |
| `rm -r directory` | Recursively remove a directory and its contents |

## 13. Important Concepts

### Absolute vs. relative paths

**Absolute path:**

```bash
cd /home/ec2-user/CompanyA
```

An absolute path starts from the root directory `/`.

**Relative path:**

```bash
cd ../Finance
```

A relative path is interpreted from the current working directory.

### Recursive operations

The `-r` option enables operations on directories and their contents, such as copying with `cp -r` or deleting with `rm -r`.

### File system organization

Directories provide a hierarchical way to organize files by purpose, department, or application component. In this exercise, Finance, HR, and Management resources were reorganized beneath HR.

### Data safety

Before deleting files or directories:

- Verify the current location with `pwd`.
- Inspect the target with `ls`.
- Confirm the source and destination paths.
- Prefer explicit file paths when practicing destructive commands.

## ✅ Lab Completion Checklist

- [x] Connected to the Amazon Linux Command Host using SSH.
- [x] Created the initial `CompanyA` directory structure.
- [x] Created all six CSV files.
- [x] Copied Finance into HR using `cp -r`.
- [x] Removed the original Finance files and directory.
- [x] Moved Management into HR using `mv`.
- [x] Created Employees under HR.
- [x] Moved Assessments.csv and TrialPeriod.csv into Employees.
- [x] Verified the final structure using `ls -laR`.

## 🏁 Conclusion

This lab strengthened my understanding of Linux file system operations and directory management. I practiced navigating with absolute and relative paths, creating files and directories, recursively copying directory contents, moving resources, and safely removing files and empty directories.

These operations are fundamental when managing Linux servers, organizing application files, maintaining project directories, and working with cloud-based environments such as Amazon EC2.

## 📚 References

- [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types)
- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Training and Certification](https://aws.amazon.com/training/)

---

**Topics:** Linux · File System · Bash Commands · Amazon EC2 · AWS Academy · Directory Management · File Operations
