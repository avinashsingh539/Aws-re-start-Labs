# Lab 261 — Public and Private IP Addresses

## Objective
Understand the difference between public and private IP addresses and investigate EC2 connectivity.

## Steps Performed

1. Started the AWS Academy lab.
2. Opened the **Details** tab and downloaded the `labsuser.pem` key. Copied the provided `PublicIP`.
3. Connected to the lab environment using SSH from Windows PowerShell:

```powershell
cd "$HOME\Downloads"
ssh -i ".\labsuser.pem" ec2-user@<PublicIP>
```

4. Opened **EC2 → Instances** in the AWS Console.
5. Inspected the Networking tab for Instance A and Instance B to compare their public and private IPv4 addresses.
6. Investigated why Instance A, with only a private IP, could not be accessed directly from outside the VPC, while Instance B had a public IP.
7. Concluded that a private CIDR range such as `10.0.0.0/16` is preferable to the public range `12.0.0.0/16` for internal VPC addressing.
8. Completed the lab questions and clicked **Submit**.

## Key Learnings

- Private IP addresses are not directly reachable over the public internet.
- Public IP addresses can enable external connectivity when routing and security rules permit it.
- Using public CIDR ranges for internal VPC addressing can cause routing conflicts.
- Private RFC 1918 ranges are recommended for internal VPC networks.

## Result

Learned how public and private IP addresses affect EC2 connectivity and why appropriate CIDR selection is important in AWS networking.

**Platform:** AWS Academy / Vocareum