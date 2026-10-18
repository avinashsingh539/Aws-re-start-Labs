# 237-[LX]-Lab - Managing File Permissions

## Objective
Practiced Linux ownership, groups, file permissions, and company directory access management on Amazon Linux EC2.

## Tasks Completed

### 1. Change File and Folder Ownership
```bash
cd /home/ec2-user/companyA
sudo chown -R mjackson:Personnel /home/ec2-user/companyA
sudo chown -R ljuan:HR HR
sudo chown -R mmajor:Finance HR/Finance
ls -laR
```

### 2. Change Permission Modes
```bash
sudo vi symbolic_mode_file
sudo chmod g+w symbolic_mode_file

sudo vi absolute_mode_file
sudo chmod 764 absolute_mode_file

ls -l
```

- `g+w`: Adds write permission for the group.
- `764`: Owner `rwx`, group `rw-`, others `r--`.

### 3. Assign Department Permissions
```bash
sudo chown -R eowusu:Shipping Shipping
sudo chown -R nwolf:Sales Sales

ls -laR Shipping
ls -laR Sales
```

## Result
Completed the lab tasks for managing Linux file ownership, group assignments, and symbolic and numeric permissions.

## Technologies
- Amazon Linux EC2
- Linux command line
- `chown`, `chmod`, `ls`, and `vi`
