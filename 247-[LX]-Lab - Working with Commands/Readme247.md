# 247-[LX]-Lab - Working with Commands

## Overview
Practiced essential Linux command-line operations on an Amazon Linux EC2 instance, including output redirection, sorting, text extraction, stream editing, and command pipelines.

## Objectives
- Use `tee` to display and save command output.
- Use `sort` to organize CSV records.
- Use the pipe operator and `grep` to search text.
- Use `cut` to extract fields from text.
- Use `sed` to replace text in files.

## Tasks and Commands

### 1. Connect to EC2
Connected to the Amazon Linux EC2 instance using SSH.

### 2. Using `tee`
```bash
cd /home/ec2-user
pwd
hostname | tee file1.txt
ls
```
**Result:** Displayed the hostname and saved it to `file1.txt`.

### 3. Using `sort` and Pipes
Created `test.csv` containing factory and store records.

```bash
sort test.csv
grep Paris test.csv
```

**Result:** Sorted records alphabetically and searched for the record containing `Paris`.

### 4. Using `cut`
Created `cities.csv` containing city and state information.

```bash
cut -d ',' -f 1 cities.csv
```

**Result:** Extracted the city names by selecting the first comma-separated field.

### 5. Using `sed`
Replaced the first comma with a period in each line.

```bash
sed 's/,/./' cities.csv
sed 's/,/./' test.csv
```

To apply the replacements directly to the files:

```bash
sed -i 's/,/./' cities.csv
sed -i 's/,/./' test.csv
```

Verified the resulting file contents:

```bash
cat cities.csv
cat test.csv
```

## Concepts Learned
- **`tee`** — Writes standard input to both the terminal and a file.
- **`sort`** — Sorts lines of text.
- **Pipe (`|`)** — Passes one command's output to another command.
- **`grep`** — Searches text for a matching pattern.
- **`cut`** — Extracts selected fields or characters.
- **`sed`** — Performs text transformations and substitutions.

## Technologies Used
- Amazon Linux EC2
- Linux command line
- SSH
- Bash utilities

## Conclusion
Completed the lab exercises covering Linux command-line utilities for handling output, sorting records, searching text, extracting fields, and modifying file contents.
