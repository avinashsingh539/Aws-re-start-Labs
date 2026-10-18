# Lab 264: Networking Resources for a VPC

## Objective
Configure AWS VPC networking resources and verify internet connectivity from an EC2 instance using `ping`.

## Resources Created

| Resource | Configuration |
|---|---|
| VPC | `Test VPC` — `192.168.0.0/18` |
| Subnet | `Public subnet` — `192.168.1.0/28` |
| Route Table | `Public route table` |
| Internet Gateway | `IGW test VPC` |
| Network ACL | `Public Subnet NACL` |
| Security Group | `public security group` |
| EC2 Instance | Amazon Linux 2023, `t3.micro` |

## Steps Performed
1. Created `Test VPC` and `Public subnet`.
2. Created `Public route table` and attached the Internet Gateway to the VPC.
3. Added the route `0.0.0.0/0` targeting the Internet Gateway.
4. Associated `Public subnet` with the route table.
5. Created a Network ACL with inbound and outbound rules allowing all traffic.
6. Created a Security Group allowing SSH (22), HTTP (80), and HTTPS (443), with outbound traffic allowed.
7. Launched an EC2 instance with a public IP in `Public subnet`.
8. Connected to the instance using SSH and tested internet connectivity.

## Commands Executed

**Connect through SSH (PowerShell):**

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@<PUBLIC_IP>
```

**Test connectivity (EC2 terminal):**

```bash
ping google.com
```

Stop ping using `Ctrl+C`.

## Key Learnings
- Internet connectivity requires an Internet Gateway, a route to `0.0.0.0/0`, and a public IP.
- Security Groups are stateful instance-level firewalls.
- Network ACLs are stateless subnet-level firewalls.
- SSH uses TCP port 22.

## Result
Successfully configured VPC networking resources and completed the lab's internet connectivity test.

**AWS Services:** Amazon VPC, EC2, Internet Gateway, Route Tables, Security Groups, Network ACLs
