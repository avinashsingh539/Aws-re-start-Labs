# 251-[LX]-Lab — Bash Shell Script

## Objective

Create and execute a Bash shell script on an Amazon Linux EC2 instance to automate the backup of the `CompanyA` directory. The script generates a date-based, compressed `.tar.gz` archive containing the directory's files and subdirectories.

**Estimated Duration:** 10 minutes

## Prerequisites

- AWS Academy Learner Lab / Vocareum access
- An Amazon Linux EC2 instance
- Windows PowerShell or Windows Terminal
- SSH access using the lab-provided `.pem` key
- Basic knowledge of Linux commands and Bash scripting

## Step 1: Start the Lab

1. Open the AWS Academy Learner Lab / Vocareum environment.
2. Click **Start Lab**.
3. Wait until the lab status displays **Ready**.
4. Click **AWS** to open the AWS Management Console.

## Step 2: Download the PEM File and Copy the Public IP

1. Return to the lab instructions page.
2. Open **Details → Show** to display the lab credentials.
3. Click **Download PEM** to download the SSH private key, usually named `labsuser.pem`.
4. Copy the value of **PublicIP** from the same panel.
5. Close the Details panel.

**Important:** If you have a PEM file from a previous lab, identify which key belongs to the current lab before connecting. Remove or rename the old file only if you no longer need it. An old PEM file does not automatically create a conflict; using the wrong key in the SSH command can cause authentication errors.

Never upload your PEM file to GitHub or share it publicly.

## Step 3: Connect to the EC2 Instance Using SSH

Open PowerShell or Windows Terminal on your computer.

Navigate to the Downloads directory:

```powershell
cd "$HOME\Downloads"
```

Connect to the EC2 instance:

```powershell
ssh -i ".\labsuser.pem" ec2-user@<PUBLIC-IP>
```

Replace `<PUBLIC-IP>` with the public IP copied from the lab's Details panel. If your downloaded PEM file has a different name, update the filename accordingly.

If prompted to confirm the host's authenticity, type:

```text
yes
```

Once connected successfully, you should see a terminal prompt for the `ec2-user` account.

## Step 4: Verify the Home Directory

Check the current working directory:

```bash
pwd
```

Expected output:

```text
/home/ec2-user
```

List the files and directories:

```bash
ls -la
```

## Step 5: Check the Source and Backup Directories

Verify that the `CompanyA` directory exists:

```bash
ls -ld /home/ec2-user/CompanyA
```

Check whether the backup directory exists:

```bash
ls -ld /home/ec2-user/backups
```

If the backup directory does not exist, create it:

```bash
mkdir -p /home/ec2-user/backups
```

List the contents of `CompanyA` and its subdirectories:

```bash
ls -R /home/ec2-user/CompanyA
```

The source directory contains the files that the script will back up.

## Step 6: Create the Bash Script

Create a file named `backup.sh`:

```bash
touch backup.sh
```

Make the script executable:

```bash
chmod 755 backup.sh
```

Open the file using the `vi` editor:

```bash
vi backup.sh
```

Press `i` to enter insert mode and add the following code:

```bash
#!/bin/bash

DAY="$(date +%Y_%m_%d)"
BACKUP="/home/$USER/backups/$DAY-backup-CompanyA.tar.gz"

tar -csvpzf "$BACKUP" "/home/$USER/CompanyA"
```

### Explanation of the Script

| Command or variable | Purpose |
|---|---|
| `#!/bin/bash` | Specifies Bash as the script interpreter. |
| `DAY="$(date +%Y_%m_%d)"` | Stores the current date in `YYYY_MM_DD` format. |
| `BACKUP=...` | Defines the destination and filename of the backup archive. |
| `$USER` | Refers to the current username, which is `ec2-user` in this lab. |
| `tar -csvpzf` | Creates a compressed archive and displays the files being processed. |
| `"$BACKUP"` | Specifies the output archive path. |
| `"/home/$USER/CompanyA"` | Specifies the directory to back up. |

Save the file and exit the editor:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

## Step 7: Verify the Script and Permissions

Display the script to confirm that it was saved correctly:

```bash
cat backup.sh
```

Check the file permissions:

```bash
ls -l backup.sh
```

The permissions should resemble:

```text
-rwxr-xr-x
```

The `755` permission allows the owner to read, write, and execute the file while other users can read and execute it.

## Step 8: Execute the Backup Script

Run the script from the home directory:

```bash
./backup.sh
```

The terminal should display the directories and files being added to the archive. Output may include:

```text
tar: Removing leading `/' from member names
/home/ec2-user/CompanyA/
/home/ec2-user/CompanyA/Management/
/home/ec2-user/CompanyA/Employees/
/home/ec2-user/CompanyA/Finance/
```

The exact output depends on the contents of the `CompanyA` directory.

## Step 9: Verify the Backup Archive

List the generated backup files:

```bash
ls -lh /home/ec2-user/backups/
```

The archive filename should follow this format:

```text
YYYY_MM_DD-backup-CompanyA.tar.gz
```

For example:

```text
2026_10_18-backup-CompanyA.tar.gz
```

The actual date will depend on when the script is executed.

Inspect the archive contents:

```bash
tar -tzf /home/ec2-user/backups/*.tar.gz
```

The command should list the archived `CompanyA` directory and its files.

**Expected result:** A compressed backup archive exists in `/home/ec2-user/backups/` and contains the files from `CompanyA`.

## Key Concepts Learned

- **Bash scripting:** Automating repetitive tasks using shell scripts.
- **Shebang:** Specifying the interpreter used to execute a script.
- **Variables:** Storing and reusing values such as dates and file paths.
- **Date formatting:** Generating filenames using the current date.
- **File permissions:** Using `chmod` to make a script executable.
- **Archive management:** Using `tar` and gzip compression to create backups.
- **SSH:** Connecting securely to a remote Linux instance.
- **Verification:** Checking file permissions, archive existence, and archive contents.

## Result

Successfully created and executed a Bash script named `backup.sh` on an Amazon Linux EC2 instance to automate the creation of a compressed backup archive of the `CompanyA` directory.

## Further Learning

The backup script can be extended to run automatically every day using `cron`. Additional improvements could include logging, error handling, backup retention, and transferring archives to Amazon S3.

## Cleanup

After verifying the backup:

1. Return to the AWS Academy lab instructions.
2. Click **End Lab**.
3. Confirm the action when prompted.

Keep your PEM file private and never commit AWS credentials or private keys to your repository.