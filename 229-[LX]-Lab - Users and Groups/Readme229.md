# 229-[LX]-Lab — Users and Groups

## Overview

**Platform:** AWS Academy Vocareum  
**Operating System:** Amazon Linux 2  
**AWS Service:** Amazon EC2  
**Connection:** SSH using Windows PowerShell

### Objectives

- Create Linux user accounts and set passwords.
- Create groups and assign users based on their job roles.
- Understand Linux file permissions.
- Test administrator privileges using `sudo`.
- Inspect security logs for unauthorized operations.

---

## Task 1 — Connect to the EC2 Instance

### Step 1: Start the lab

1. Open the lab in AWS Academy Vocareum.
2. Click **Start Lab** and wait until the status displays `Lab status: ready`.
3. Open **Details → Show**.

### Step 2: Download the SSH key and obtain the Public IP

Inside the **Details → Show** panel:

- Click **Download PEM** to download `labsuser.pem`.
- Copy the **PublicIP** value displayed in the panel.
- Close the panel when finished.

**Important:** The Public IP is available in the Vocareum **Details** panel. Always use the IP provided for your current lab session.

### Step 3: Connect using Windows PowerShell

Run the following command, replacing `<PUBLIC_IP>` with the IP address copied from Vocareum.

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@<PUBLIC_IP>
```

If prompted to trust the host on the first connection, enter `yes` and press Enter.

**Expected result:** A prompt similar to:

```text
[ec2-user@ip-10-0-... ~]$
```

### Step 4: Verify the current directory

```bash
pwd
```

Expected output:

```text
/home/ec2-user
```

---

## Task 2 — Create Users

The lab requires 10 user accounts with specific usernames and job roles.

### User Details

| User ID | Full Name | Job Role |
|---|---|---|
| `arosalez` | Alejandro Rosalez | Sales Manager |
| `eowusu` | Efua Owusu | Shipping |
| `jdoe` | Jane Doe | Shipping |
| `ljuan` | Li Juan | HR Manager |
| `mmajor` | Mary Major | Finance Manager |
| `mjackson` | Mateo Jackson | CEO |
| `nwolf` | Nikki Wolf | Sales Representative |
| `psantos` | Paulo Santos | Shipping |
| `smartinez` | Sofia Martinez | HR Specialist |
| `ssarkar` | Saanvi Sarkar | Finance Specialist |

### Default Password for All Lab Users

The lab specifies the following starting password for all 10 accounts:

```text
P@ssword1234!
```

**Important:** When running `sudo passwd <username>`, enter the password twice when prompted. Linux does not display password characters while you type.

This password is intended for this lab exercise only. Do not reuse it for real accounts or production systems.

### Step 1: Create the first user

Create Alejandro Rosalez's account:

```bash
sudo useradd arosalez
```

Set the password:

```bash
sudo passwd arosalez
```

Enter `P@ssword1234!` twice when prompted.

### Step 2: Create the remaining nine users

Run the following commands in your SSH terminal:

```bash
sudo useradd eowusu
sudo passwd eowusu

sudo useradd jdoe
sudo passwd jdoe

sudo useradd ljuan
sudo passwd ljuan

sudo useradd mmajor
sudo passwd mmajor

sudo useradd mjackson
sudo passwd mjackson

sudo useradd nwolf
sudo passwd nwolf

sudo useradd psantos
sudo passwd psantos

sudo useradd smartinez
sudo passwd smartinez

sudo useradd ssarkar
sudo passwd ssarkar
```

For **every** `passwd` command, enter the default lab password twice.

### Step 3: Verify user creation

```bash
sudo cat /etc/passwd | cut -d: -f1
```

Check that all 10 usernames appear in the output.

- `cat /etc/passwd` displays account information.
- `cut -d: -f1` extracts the username field from each line.

**Expected result:** All 10 accounts are present.

---

## Task 3 — Create Groups and Assign Users

The lab requires these seven groups:

- `Sales`
- `HR`
- `Finance`
- `Personnel`
- `CEO`
- `Shipping`
- `Managers`

### Step 1: Create all seven groups

```bash
sudo groupadd Sales
sudo groupadd HR
sudo groupadd Finance
sudo groupadd Personnel
sudo groupadd CEO
sudo groupadd Shipping
sudo groupadd Managers
```

### Step 2: Add users to the Sales group

```bash
sudo usermod -a -G Sales arosalez
sudo usermod -a -G Sales nwolf
```

### Step 3: Add users to the HR group

```bash
sudo usermod -a -G HR ljuan
sudo usermod -a -G HR smartinez
```

### Step 4: Add users to the Finance group

```bash
sudo usermod -a -G Finance mmajor
sudo usermod -a -G Finance ssarkar
```

### Step 5: Add users to the Shipping group

```bash
sudo usermod -a -G Shipping eowusu
sudo usermod -a -G Shipping jdoe
sudo usermod -a -G Shipping psantos
```

### Step 6: Add users to the Managers group

```bash
sudo usermod -a -G Managers arosalez
sudo usermod -a -G Managers ljuan
sudo usermod -a -G Managers mmajor
```

### Step 7: Add the CEO to the CEO group

```bash
sudo usermod -a -G CEO mjackson
```

### Step 8: Add `ec2-user` to all seven groups

The lab requires the original `ec2-user` account to be added to every group.

```bash
sudo usermod -a -G Sales,HR,Finance,Personnel,CEO,Shipping,Managers ec2-user
```

**Understanding the command:**

- `usermod` modifies an existing user account.
- `-a` appends group memberships without removing existing supplementary groups.
- `-G` specifies supplementary groups.

### Step 9: Verify group creation and membership

Display the group information:

```bash
sudo cat /etc/group
```

Check the group memberships of `ec2-user`:

```bash
id ec2-user
```

Verify that the seven groups exist and the required usernames appear in their respective memberships.

**Expected result:** All seven groups exist, and the users have been assigned to the appropriate groups.

---

## Task 4 — Log In Using a New User

This task demonstrates file permissions, `sudo` restrictions, and security logging.

### Step 1: Switch to `arosalez`

```bash
su arosalez
```

When prompted, enter the default lab password:

```text
P@ssword1234!
```

The terminal prompt should now identify `arosalez`.

### Step 2: Check the current directory

```bash
pwd
```

Expected output:

```text
/home/ec2-user
```

You are logged in as `arosalez`, but the current directory is still `ec2-user`'s home directory.

### Step 3: Test normal file creation

```bash
touch myFile.txt
```

**Expected result:** A `Permission denied` error.

The `arosalez` user cannot create a file in `ec2-user`'s home directory because the user does not have the required write permissions.

### Step 4: Test administrator privileges

```bash
sudo touch myFile.txt
```

Enter the password if prompted.

**Expected result:** A message similar to:

```text
arosalez is not in the sudoers file. This incident will be reported.
```

This demonstrates that a normal user does not automatically have permission to execute administrator commands.

### Step 5: Return to `ec2-user`

```bash
exit
```

This exits the `arosalez` shell and returns you to the previous `ec2-user` shell.

### Step 6: Inspect the security log

```bash
sudo cat /var/log/secure
```

Inspect the output and look for the failed `sudo` attempt made by `arosalez`.

The log can record information such as the username, attempted command, working directory, and authorization failure.

**Expected result:** The security log contains a record of the unauthorized `sudo` operation.

---

## Important Commands Reference

| Command | Purpose |
|---|---|
| `pwd` | Display the current working directory |
| `useradd` | Create a user account |
| `passwd` | Set or change a user's password |
| `groupadd` | Create a group |
| `usermod -a -G` | Add a user to supplementary groups |
| `cat /etc/passwd` | Display user account records |
| `cat /etc/group` | Display group records |
| `cut -d: -f1` | Extract the first colon-separated field |
| `id username` | Display user and group IDs |
| `su username` | Switch to another user |
| `touch filename` | Create an empty file if permitted |
| `sudo command` | Attempt to execute a command with elevated privileges |
| `cat /var/log/secure` | Inspect security-related log entries |
| `exit` | Exit the current shell |

---

## Key Learnings

- Connected to an Amazon Linux EC2 instance through SSH.
- Created 10 Linux users and configured their starting passwords.
- Created seven groups and assigned users according to their roles.
- Used supplementary group membership to organize user access.
- Tested file permissions and administrator privileges.
- Inspected security logs to understand how unauthorized operations are recorded.

## Lab Completion

**Status:** Successfully completed.

### End the Lab

1. Return to the Vocareum lab page.
2. Click **End Lab**.
3. Confirm by clicking **Yes**.
4. Wait for the lab termination message.

**Note:** This lab uses a temporary AWS Academy environment. Obtain the current instance's Public IP and SSH key from Vocareum whenever you start the lab again.
