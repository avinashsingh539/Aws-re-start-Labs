# Lab 263: Create Subnets in a VPC

## Objective
Create an Amazon VPC with a suitable private IPv4 address range and a public subnet that meets the customer's IP address requirements.

## Configuration

| Resource | Configuration |
|---|---|
| VPC Name | `First VPC` |
| VPC IPv4 CIDR | `192.168.0.0/18` |
| Subnet Name | `Public subnet` |
| Subnet IPv4 CIDR | `192.168.1.0/26` |
| IPv6 | No IPv6 CIDR block |
| Availability Zone | No Preference |

## Steps Performed
1. Opened the Amazon VPC Console in the AWS Academy lab environment.
2. Selected **Create VPC** and chose **VPC with a Single Public Subnet**.
3. Configured the VPC CIDR as `192.168.0.0/18`.
4. Configured the public subnet CIDR as `192.168.1.0/26`.
5. Named the resources `First VPC` and `Public subnet`.
6. Created the VPC and verified the CIDR blocks under **Your VPCs** and **Subnets**.
7. Ended the lab after verification.

## IP Address Calculations

- **VPC — `/18`:** 16,384 total IPv4 addresses, sufficient for approximately 15,000 private IP addresses.
- **Public subnet — `/26`:** 64 total IPv4 addresses. AWS reserves 5 addresses in each subnet, leaving **59 usable addresses**, sufficient for at least 50 IP addresses.

## Key Learnings
- A VPC provides an isolated virtual network in AWS.
- CIDR notation defines the size of an IP address range.
- Subnets divide a VPC's address range into smaller network segments.
- Public subnet classification depends on routing to an internet gateway; a subnet's name alone does not make it public.

## Result
Successfully created and verified a VPC with the required IPv4 CIDR range and a public subnet meeting the IP address requirement.

**Services Used:** Amazon VPC, Subnets, IPv4 CIDR
