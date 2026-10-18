# Lab 266 — Troubleshooting a Network Issue

## 📌 Overview

This lab simulates a real-world AWS cloud support scenario in which a customer cannot access an Apache web server hosted on an Amazon EC2 instance. Although the Apache server is running, the website cannot be reached through the instance's public IP address.

The objective is to investigate the VPC networking configuration, identify connectivity issues, and restore access to the web server.

Time required :- 5 minutes

## 🎯 Objectives

- Analyze a customer's network connectivity problem.
- Connect to an Amazon Linux EC2 instance using SSH.
- Install or verify and start the Apache HTTP Server.
- Inspect VPC networking resources.
- Troubleshoot routing, Internet Gateway, security group, and network ACL configurations.
- Verify HTTP access through the instance's public IP address.

## 🛠️ AWS Services and Tools

- **Amazon EC2** — Hosts the Apache web server.
- **Amazon VPC** — Provides the virtual network.
- **Public Subnet** — Hosts the EC2 instance.
- **Internet Gateway (IGW)** — Enables internet connectivity for eligible resources in the VPC.
- **Route Tables** — Control network traffic routing.
- **Security Groups** — Control instance-level inbound and outbound traffic.
- **Network ACLs** — Control subnet-level inbound and outbound traffic.
- **SSH and Linux systemctl** — Used to access and manage the server.

## 🚀 Implementation Steps

### 1. Connect to the EC2 Instance

Retrieve the instance's public IP address and SSH key from the Vocareum lab environment.

Connect from Windows PowerShell:

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@<PUBLIC_IP>
```

Replace `<PUBLIC_IP>` with the instance's actual public IP address.

### 2. Check and Start Apache

Check the Apache HTTP Server status:

```bash
sudo systemctl status httpd.service
```

Start the service:

```bash
sudo systemctl start httpd.service
```

Verify the service again:

```bash
sudo systemctl status httpd.service
```

The expected service state is `active (running)`.

### 3. Test the Web Server

Open the following URL in a browser:

```text
http://<PUBLIC_IP>
```

If the page does not load even though Apache is active, investigate the VPC networking configuration.

### 4. Verify Internet Connectivity

Run the following command on the EC2 instance:

```bash
ping www.amazon.com -c 5
```

This helps determine whether the instance can reach the destination. A successful ping does not, by itself, prove that inbound HTTP access to the instance is configured correctly.

### 5. Inspect the VPC Configuration

Navigate to **AWS Console → VPC** and investigate the following resources.

| Resource | Configuration to Verify |
|---|---|
| Subnet | Confirm the EC2 instance is in the intended public subnet and the correct route table is associated. |
| Route Table | Verify that a default route (`0.0.0.0/0`) points to the Internet Gateway. |
| Internet Gateway | Confirm that an Internet Gateway is attached to the correct VPC. |
| Security Group | Verify that inbound HTTP traffic on TCP port `80` is permitted. Check SSH access on TCP port `22` as needed. |
| Network ACL | Check inbound and outbound rules, including the return traffic required for the connection. |

Also verify that the instance has a public IPv4 address. A public subnet route alone does not automatically make an instance internet-accessible.

### 6. Validate the Fix

After correcting the identified configuration issue, open:

```text
http://<PUBLIC_IP>
```

The expected result is the Apache HTTP Server test page.

### 7. Complete the Lab

After verifying connectivity, select **End Lab** in Vocareum and confirm the action.

## 📚 Key Learnings

- A running application does not guarantee network accessibility.
- A public subnet generally requires a route to an Internet Gateway for direct internet connectivity.
- An EC2 instance needs a public IP address or another appropriate addressing arrangement for direct IPv4 internet access.
- Security groups and network ACLs can block otherwise valid network traffic.
- Security groups are stateful, whereas network ACLs are stateless.
- HTTP normally uses TCP port `80`; HTTPS normally uses TCP port `443`.
- Systematically checking the application, addressing, routing, and traffic rules makes network troubleshooting more effective.

## ✅ Result

Practiced troubleshooting an Apache web server hosted on Amazon EC2 by checking the service status and investigating the VPC, subnet, route table, Internet Gateway, security group, and network ACL configuration.

## 💡 Conclusion

This lab demonstrates a structured approach to AWS network troubleshooting. By separating application-level problems from addressing, routing, and access-control issues, cloud support engineers can identify the source of connectivity failures and restore access to services hosted in a VPC.