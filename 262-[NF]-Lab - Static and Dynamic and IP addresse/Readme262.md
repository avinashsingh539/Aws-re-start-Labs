# Lab 261 — Public and Private IP Addresses

## Objective
Understand the difference between public and private IP addresses and investigate EC2 connectivity in AWS.

## Steps Performed

1. Started the AWS Academy lab and opened the **Details** tab.
2. Downloaded `labsuser.pem` and copied the provided `PublicIP`.
3. Connected to the lab environment through SSH using Windows PowerShell.

```powershell
cd "$HOME\Downloads"
ssh -i ".\labsuser.pem" ec2-user@<PublicIP>
```

4. Opened **EC2 → Instances** in the AWS Console.
5. Inspected the Networking tab of Instance A and Instance B to compare their public and private IPv4 addresses.
6. Investigated why Instance A, which has only a private IP, cannot be accessed directly from outside the VPC, while Instance B has a public IP.
7. Recommended using a private CIDR range such as `10.0.0.0/16` instead of the public range `12.0.0.0/16` for internal VPC addressing.
8. Completed the lab and clicked **Submit**.

## Key Learnings

- **Private IP:** Used for communication within private networks; not directly reachable over the public internet.
- **Public IP:** Can enable external connectivity when routing and security rules permit it.
- **VPC CIDR:** Using public address ranges internally can cause routing conflicts.
- **Best Practice:** Use appropriate private CIDR ranges for internal VPC networking.

## Result

Understood public and private IP addressing, EC2 connectivity, and the importance of selecting appropriate CIDR ranges in AWS.

**Platform:** AWS Academy / Vocareum