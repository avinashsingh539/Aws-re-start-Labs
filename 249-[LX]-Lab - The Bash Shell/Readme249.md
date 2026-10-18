# 249-[LX]-Lab - The Bash Shell

## 📌 Overview
This lab explores Bash shell features on an Amazon Linux EC2 instance, focusing on creating aliases to simplify repetitive commands and modifying the `PATH` environment variable to execute scripts from different directories.

## 🎯 Objectives
- Create and use a Bash alias to back up a directory.
- Understand how the `PATH` environment variable works.
- Add a custom directory to `PATH` to execute a script without specifying its full path.

## 🛠️ Environment
- **Cloud Platform:** Amazon Web Services (AWS)
- **Service:** Amazon EC2
- **Operating System:** Amazon Linux
- **Shell:** Bash
- **Connection:** SSH

## 🧪 Tasks and Implementation

### Task 1: Connect to the EC2 Instance
Connect to the Amazon Linux instance using SSH and the lab-provided public IP address and private key.

```bash
ssh -i "labsuser.pem" ec2-user@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the current public IP provided by the lab environment.

### Task 2: Create a Backup Alias

Create a Bash alias named `backup` to simplify the process of creating compressed archives.

```bash
alias backup='tar -cvzf '
```

Use the alias to create a compressed backup of the `CompanyA` directory:

```bash
backup backup_companyA.tar.gz CompanyA
```

Verify the backup archive:

```bash
ls
```

**Key concepts:**
- `alias` creates a shortcut for a command.
- `tar` archives files and directories.
- `-c` creates an archive.
- `-v` displays processed files.
- `-z` compresses the archive using gzip.
- `-f` specifies the archive filename.

### Task 3: Explore and Modify the PATH Variable

Navigate to the directory containing the `hello.sh` script:

```bash
cd /home/ec2-user/CompanyA/bin
```

Execute the script using its relative path:

```bash
./hello.sh
```

Expected output:

```text
hello ec2-user
```

Move to the parent directory and execute the script using its relative path:

```bash
cd ..
./bin/hello.sh
```

Try executing the script directly:

```bash
hello.sh
```

Initially, Bash may report `command not found` because the script's directory is not included in `PATH`.

Display the existing `PATH`:

```bash
echo $PATH
```

Add the script directory to the current shell's `PATH`:

```bash
PATH=$PATH:/home/ec2-user/CompanyA/bin
```

Now execute:

```bash
hello.sh
```

Expected output:

```text
hello ec2-user
```

## 📚 Key Learnings

- **Bash aliases:** Provide convenient shortcuts for frequently used commands.
- **Archiving and compression:** `tar` can package directories into compressed backup files.
- **PATH variable:** Contains directories that Bash searches when resolving commands.
- **Command execution:** Scripts can be executed using their paths or by command name when their directory is included in `PATH`.
- **Temporary environment changes:** Updating `PATH` as shown affects the current shell session. The change does not automatically persist across new sessions.

## ✅ Conclusion

This lab demonstrates how Bash aliases can simplify repetitive backup operations and how modifying `PATH` enables scripts to be launched by name without navigating to their directory. These skills are useful for Linux administration, automation, and command-line productivity.

---
**Lab:** 249-[LX]-Lab — The Bash Shell  
**Platform:** AWS Academy / Vocareum  
**Category:** Linux Fundamentals
