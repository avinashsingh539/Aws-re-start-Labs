# 239-[LX]-Lab - Managing Processes

## Overview
Practiced Linux process monitoring, process logging, and scheduled task automation on an Amazon Linux EC2 instance.

## Objectives
- Generate a process listing and save it to a CSV file.
- Monitor active processes using `top`.
- Create and verify a scheduled cron job.
- Use Linux pipes and `sed` for text processing.

## Tasks and Commands

### 1. Connect to EC2
Connected to the Amazon Linux EC2 instance using SSH.

### 2. Create a Process Log
```bash
cd /home/ec2-user/companyA
sudo ps -aux | grep -v root | sudo tee SharedFolders/processes.csv
cat SharedFolders/processes.csv
```

**Result:** Generated a process listing, filtered lines containing `root`, and saved the output to `SharedFolders/processes.csv`.

### 3. Monitor Processes Using `top`
```bash
top
```

- Observed running, sleeping, stopped, and zombie tasks.
- Pressed `q` to exit.
- Checked usage and version information:

```bash
top -hv
```

**Result:** Practiced real-time process monitoring and system resource inspection.

### 4. Schedule a Cron Job
Opened the root user's crontab:

```bash
sudo crontab -e
```

Configured the following entries:

```bash
SHELL=/bin/bash
PATH=/usr/bin:/bin:/usr/local/bin
MAILTO=root
0 * * * * ls -la $(find .) | sed -e 's/..csv/#####.csv/g' > /home/ec2-user/companyA/SharedFolders/filteredAudit.csv
```

Verified the scheduled task:

```bash
sudo crontab -l
```

**Result:** Configured a cron job to run hourly and redirect its output to `SharedFolders/filteredAudit.csv`.

## Concepts Learned
- **`ps`** — Displays information about running processes.
- **`grep -v`** — Excludes lines matching a pattern.
- **`tee`** — Displays output and writes it to a file.
- **`top`** — Monitors processes and system resource usage.
- **`cron` / `crontab`** — Schedules recurring tasks.
- **`sed`** — Performs text transformations.
- **Pipe (`|`)** — Passes output between commands.

## Technologies Used
- Amazon Linux EC2
- Linux command line
- SSH
- Bash utilities
- Cron scheduling

## Conclusion
Completed the lab exercises covering process listing, real-time process monitoring, file-based logging, and scheduled task configuration using Linux utilities.
