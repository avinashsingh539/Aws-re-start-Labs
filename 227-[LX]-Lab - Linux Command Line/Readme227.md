## 227-[LX]-Lab - Linux Command Line

## Overview

**Platform:** AWS Academy Vocareum  
**Operating System:** Amazon Linux 2  
**Connection:** SSH using Windows PowerShell

### Objectives
- Connect to an Amazon Linux EC2 instance.
- Execute Linux commands to inspect system information.
- Explore date, time-zone, and calendar commands.
- View, search, and repeat previously executed commands.

## Task 1 — Connect to the EC2 Instance

Connect to the lab instance from Windows PowerShell using the downloaded PEM key.

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@<PUBLIC_IP>
```

Replace `<PUBLIC_IP>` with the public IP address displayed in Vocareum.

**Result:** Successfully connected to the Amazon Linux 2 instance.

## Task 2 — Execute Linux Commands

### 1. Identify the Current User

```bash
whoami
```

Displays the username of the current user.

**Result:** `ec2-user`

### 2. Display the Hostname

```bash
hostname -s
```

Displays the short hostname of the EC2 instance.

### 3. Check System Uptime

```bash
uptime -p
```

Displays how long the system has been running in a human-readable format.

### 4. Display Login Information

```bash
who -H -a
```

Displays login and session information, including available user and system details.

### 5. Display Date and Time in New York

```bash
TZ=America/New_York date
```

Displays the date and time using the New York time zone.

### 6. Display Date and Time in Los Angeles

```bash
TZ=America/Los_Angeles date
```

Displays the date and time using the Los Angeles time zone.

### 7. Display the Julian Calendar

```bash
cal -j
```

Displays the current month's calendar using Julian day numbers.

### 8. Display the Calendar Starting on Monday

```bash
cal -m
```

Displays the calendar with Monday as the first day of the week.

### 9. Check User and Group IDs

```bash
id ec2-user
```

Displays the user ID (UID), primary group ID (GID), and supplementary group information for `ec2-user`.

**Result:** All Task 2 commands executed successfully.

## Task 3 — Command History and Repetition

### 1. View Command History

```bash
history
```

Displays previously executed commands in the current Bash session.

### 2. Search Previous Commands

Use Bash reverse history search:

1. Press `Ctrl + R`.
2. Type `TZ` to search for a previous time-zone command.
3. Inspect the recalled command and edit it if necessary.
4. Press `Enter` to execute the selected command.

### 3. Display the Current Date and Time

```bash
date
```

Displays the system's current date and time.

**Observed output:**

```text
Sun Oct 18 05:54:32 UTC 2026
```

### 4. Repeat the Previous Command

```bash
!!
```

Repeats the most recently executed command.

**Observed output:**

```text
date
Sun Oct 18 05:54:37 UTC 2026
```

**Result:** Command history was displayed, `date` executed successfully, and `!!` repeated the previous command successfully.

## Key Learnings

- Connected to an EC2 instance using SSH.
- Identified the current user and system hostname.
- Inspected system uptime and login information.
- Displayed dates in different time zones.
- Used calendar commands and inspected user/group IDs.
- Retrieved previous commands using Bash history.
- Repeated commands using `!!`.

## Lab Completion

**Status:** Successfully completed the Linux command-line tasks in the AWS Academy lab.

**Environment:** Amazon Linux 2 on AWS EC2.