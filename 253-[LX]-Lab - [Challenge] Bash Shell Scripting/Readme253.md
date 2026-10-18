# 253-[LX]-Lab — [Challenge] Bash Shell Scripting Exercise

## Objective

Create a Bash shell script that automatically generates 25 empty files per execution. Each file uses a name consisting of a chosen prefix followed by a number. Every subsequent execution must continue numbering from the highest existing file number without hard-coding the starting number.

**Estimated Duration:** 5 minutes

## Prerequisites

- AWS Academy Learner Lab / Vocareum access
- An Amazon Linux EC2 instance
- Windows PowerShell or Windows Terminal
- SSH access using the lab-provided `.pem` private key
- Basic knowledge of Linux commands and Bash scripting

## Step 1: Start the AWS Academy Lab

1. Open the AWS Academy Learner Lab / Vocareum environment.
2. Click **Start Lab**.
3. Wait until the status displays **Ready**.
4. Click **AWS** to open the AWS Management Console in a new browser tab.

## Step 2: Download the PEM File and Copy the Public IP

1. Return to the lab instructions page.
2. Open **Details → Show** to display the lab credentials.
3. Click **Download PEM** to download the `labsuser.pem` private key.
4. Copy the **PublicIP** address displayed in the same panel.
5. Close the Details panel.

**Important:** Use the PEM file and public IP associated with the current lab. If you have a PEM file from a previous lab, make sure you select the correct one. You can keep both files; deleting the old PEM is not necessary. Using the wrong key may cause an SSH authentication error.

Never upload your PEM file to GitHub or share it publicly.

## Step 3: Connect to the EC2 Instance Using SSH

Open Windows PowerShell or Windows Terminal.

Navigate to your Downloads directory:

```powershell
cd "$HOME\Downloads"
```

Connect to the EC2 instance using the downloaded PEM file:

```powershell
ssh -i ".\labsuser.pem" ec2-user@<PUBLIC-IP>
```

Replace `<PUBLIC-IP>` with the address copied from the lab's Details panel. If your PEM file has a different name, replace `labsuser.pem` with the correct filename.

If prompted to confirm the host's authenticity, type:

```text
yes
```

After successful authentication, you will enter the Amazon Linux EC2 terminal.

## Step 4: Verify the EC2 Connection

Check the current username:

```bash
whoami
```

Expected output:

```text
ec2-user
```

Check the current working directory:

```bash
pwd
```

Expected output:

```text
/home/ec2-user
```

## Step 5: Create a Directory for the Challenge

Create a dedicated directory:

```bash
mkdir -p ~/bash-challenge
```

Navigate into it:

```bash
cd ~/bash-challenge
```

Verify the current location:

```bash
pwd
```

Expected output:

```text
/home/ec2-user/bash-challenge
```

## Step 6: Create the Bash Script

Create a script named `create_files.sh`:

```bash
vi create_files.sh
```

Press `i` to enter insert mode and add the following code:

```bash
#!/bin/bash

PREFIX="avinash"
COUNT=25
MAX=0

for file in "$PREFIX"*; do
    if [[ -f "$file" && "$file" =~ ^${PREFIX}([0-9]+)$ ]]; then
        NUM="${BASH_REMATCH[1]}"
        if (( NUM > MAX )); then
            MAX=$NUM
        fi
    fi
done

START=$((MAX + 1))
END=$((MAX + COUNT))

for ((i=START; i<=END; i++)); do
    touch "${PREFIX}${i}"
done

echo "Created 25 files: ${PREFIX}${START} to ${PREFIX}${END}"
```

Save the script and exit the editor:

1. Press `Esc`.
2. Type `:wq`.
3. Press `Enter`.

**Note:** The prefix is set to `avinash` as used in this lab. You can replace it with your preferred name.

## Step 7: Make the Script Executable

Run:

```bash
chmod +x create_files.sh
```

Display the script to verify its contents:

```bash
cat create_files.sh
```

Check the file permissions:

```bash
ls -l create_files.sh
```

The permissions should show that the script is executable by its owner.

## Step 8: Execute the Script for the First Time

Run:

```bash
./create_files.sh
```

Expected output:

```text
Created 25 files: avinash1 to avinash25
```

The script creates these 25 empty files:

```text
avinash1
avinash2
avinash3
...
avinash25
```

## Step 9: Verify the First Batch of Files

Display the directory contents in long-listing format:

```bash
ls -l
```

Count the generated files:

```bash
ls -1 avinash[0-9]* | wc -l
```

Expected output:

```text
25
```

Verify that the generated files are empty:

```bash
du -b avinash1 avinash2 avinash25
```

The reported sizes should be zero bytes.

## Step 10: Execute the Script Again

Run the same script a second time:

```bash
./create_files.sh
```

Expected output:

```text
Created 25 files: avinash26 to avinash50
```

The script identifies the highest existing number and generates the next 25 files instead of restarting from 1.

## Step 11: Verify the Second Batch

Count all generated files:

```bash
ls -1 avinash[0-9]* | wc -l
```

Expected output:

```text
50
```

Verify the number of zero-byte files:

```bash
find . -maxdepth 1 -type f -name 'avinash[0-9]*' -size 0c | wc -l
```

Expected output:

```text
50
```

Check the last five files:

```bash
ls -l avinash{46..50}
```

The files should exist and have a size of zero bytes.

## Step 12: Display the Final Directory Listing

Display the directory contents in long format:

```bash
ls -lh
```

To display the generated files specifically:

```bash
ls -l avinash*
```

After two successful executions, the directory should contain 50 generated files, numbered from `avinash1` through `avinash50`, along with the script file.

## How the Script Works

| Component | Purpose |
|---|---|
| `#!/bin/bash` | Specifies Bash as the script interpreter. |
| `PREFIX="avinash"` | Defines the filename prefix. |
| `COUNT=25` | Specifies how many files to create per execution. |
| `MAX=0` | Initializes the maximum file number. |
| `for file in "$PREFIX"*` | Iterates through filenames matching the prefix. |
| `BASH_REMATCH[1]` | Extracts the numeric suffix from a matching filename. |
| `MAX=$NUM` | Updates the highest number found. |
| `START=$((MAX + 1))` | Calculates the next starting number. |
| `END=$((MAX + COUNT))` | Calculates the final number for the batch. |
| `touch` | Creates empty files if they do not already exist. |

## Key Concepts Learned

- Bash scripting and task automation
- Variables and arithmetic operations
- Loops and conditional statements
- Filename pattern matching and regular expressions
- Automatic numbering based on existing files
- Creating empty files using `touch`
- Executable file permissions using `chmod`
- Connecting to an Amazon Linux EC2 instance using SSH and a PEM key
- Verifying files using `ls`, `wc`, `du`, and `find`

## Result

Successfully created and tested a Bash shell script that generates 25 empty files per execution, automatically continues numbering from the highest existing file number, and avoids hard-coding the starting number.

After two successful executions, the directory contains 50 generated files, numbered from `avinash1` through `avinash50`.

## Cleanup

After completing the challenge:

1. Return to the AWS Academy lab instructions.
2. Click **End Lab**.
3. Confirm the action by clicking **Yes**.
4. Wait for the lab termination message.

Keep your PEM private key secure and never commit it or AWS credentials to your GitHub repository.