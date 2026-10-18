# 235-[LX]-Lab - Working with Files

## Overview
Linux file management and backup operations using Amazon Linux EC2.

**Platform:** AWS Academy / Vocareum  
**Environment:** Amazon Linux EC2

## Objectives
- Create a compressed backup using `tar`.
- Record backup details in a log file.
- Move the archive to another directory.
- Verify file operations.

## Commands Executed

### 1. Inspect Directory Structure
```bash
cd /home/ec2-user
pwd
ls -R CompanyA
```

### 2. Create Backup
```bash
tar -cvpzf backup.CompanyA.tar.gz CompanyA
ls -lh backup.CompanyA.tar.gz
```

### 3. Log Backup Details
```bash
cd /home/ec2-user/CompanyA
touch SharedFolders/backups.csv
date '+%d %b %Y, %H:%M, backup.CompanyA.tar.gz' | tee SharedFolders/backups.csv
cat SharedFolders/backups.csv
```

### 4. Move Backup Archive
```bash
mv ../backup.CompanyA.tar.gz IA/
ls . IA
```

## Key Commands

| Command | Purpose |
|---|---|
| `tar` | Create archives |
| `date` | Display date and time |
| `tee` | Write output to a file |
| `cat` | Display file contents |
| `mv` | Move files |
| `ls` | List files and directories |

## Result
- Created `backup.CompanyA.tar.gz`.
- Recorded backup details in `SharedFolders/backups.csv`.
- Moved the archive into `CompanyA/IA/`.
- Verified the resulting directory contents.

**Status:** Lab completed successfully.