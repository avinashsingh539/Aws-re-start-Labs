# Lab 267 — Build Your VPC and Launch a Web Server

## 📌 Overview

This lab demonstrates how to build a custom network infrastructure using Amazon Virtual Private Cloud (Amazon VPC) and deploy a web server on Amazon EC2.

The lab involves creating public and private subnets, configuring route tables, setting up a security group, and launching an EC2 instance with Apache HTTP Server and a sample web application.

## 🎯 Objectives

- Create a custom VPC with IPv4 addressing.
- Configure public and private subnets across multiple Availability Zones.
- Associate subnets with the appropriate route tables.
- Create a security group to permit HTTP traffic.
- Launch and configure an EC2 web server.
- Verify web application accessibility using the instance's public DNS name.

## 🛠️ AWS Services Used

- **Amazon VPC** — Creates an isolated virtual network.
- **Amazon EC2** — Hosts the web server.
- **Internet Gateway** — Enables internet connectivity for eligible resources in public subnets.
- **NAT Gateway** — Provides outbound internet connectivity for resources in private subnets.
- **Route Tables** — Control network traffic routing.
- **Security Groups** — Control inbound and outbound instance traffic.

## 🏗️ Network Architecture

The lab builds the following infrastructure:

| Resource | Name | IPv4 CIDR / Configuration |
|---|---|---|
| VPC | `Lab VPC` | `10.0.0.0/16` |
| Public Subnet 1 | `Public Subnet 1` | `10.0.0.0/24` |
| Private Subnet 1 | `Private Subnet 1` | `10.0.1.0/24` |
| Public Subnet 2 | `Public Subnet 2` | `10.0.2.0/24` |
| Private Subnet 2 | `Private Subnet 2` | `10.0.3.0/24` |
| Public Route Table | `Public Route Table` | Associated with public subnets |
| Private Route Table | `Private Route Table` | Associated with private subnets |
| Security Group | `Web Security Group` | Allows inbound HTTP |
| EC2 Instance | `Web Server 1` | `t3.micro` |

The VPC wizard initially creates one public subnet and one private subnet in a single Availability Zone. Two additional subnets are then created and associated with the corresponding route tables.

## 🚀 Implementation Steps

### 1. Create the VPC

Navigate to **AWS Console → VPC → Create VPC**.

Configure the following settings:

- Resources to create: `VPC and more`
- VPC name: `Lab VPC`
- IPv4 CIDR: `10.0.0.0/16`
- IPv6: No IPv6 CIDR block
- Tenancy: Default
- Availability Zones: 1
- Public subnets: 1
- Private subnets: 1
- NAT gateways: In 1 AZ
- VPC endpoints: None

Set the initial subnet CIDRs to `10.0.0.0/24` for the public subnet and `10.0.1.0/24` for the private subnet.

Create the VPC and name the generated route tables `Public Route Table` and `Private Route Table`.

### 2. Create Additional Subnets

Navigate to **VPC → Subnets → Create subnet**.

Create these additional subnets inside `Lab VPC`:

- `Public Subnet 2` — `10.0.2.0/24`
- `Private Subnet 2` — `10.0.3.0/24`

Choose **No preference** for the Availability Zone, as specified in the lab.

### 3. Configure Subnet Associations

Navigate to **VPC → Route Tables**.

For `Public Route Table`:
1. Open Subnet associations.
2. Choose Edit subnet associations.
3. Select `Public Subnet 2`.
4. Save the associations.

For `Private Route Table`:
1. Open Subnet associations.
2. Choose Edit subnet associations.
3. Select `Private Subnet 2`.
4. Save the associations.

The original public and private subnets created by the wizard remain associated with their respective route tables.

### 4. Create the Web Security Group

Navigate to **VPC → Security Groups → Create security group**.

Configure:

- Name: `Web Security Group`
- Description: `Enable HTTP access`
- VPC: `Lab VPC`
- Inbound rule: HTTP
- Source: Anywhere IPv4 (`0.0.0.0/0`)
- Rule description: `Permit web requests`

This allows incoming HTTP requests over TCP port `80`.

### 5. Launch the EC2 Instance

Navigate to **EC2 → Instances → Launch instances**.

Configure:

| Setting | Value |
|---|---|
| Instance name | `Web Server 1` |
| AMI | Amazon Linux 2 AMI (HVM) |
| Instance type | `t3.micro` |
| Key pair | `vockey` |
| VPC | `Lab VPC` |
| Subnet | `Public Subnet 2` |
| Auto-assign public IP | Enable |
| Security group | `Web Security Group` |

### 6. Configure the Web Server Using User Data

Under **Advanced details → User data**, enter the following script:

```bash
#!/bin/bash
# Install Apache Web Server and PHP
yum install -y httpd mysql php

# Download lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RESTRT-1/267-lab-NF-build-vpc-web-server/s3/lab-app.zip

# Extract application files
unzip lab-app.zip -d /var/www/html/

# Enable and start Apache
chkconfig httpd on
service httpd start
```

Launch the instance and wait for its status checks to show **2/2 checks passed**.

### 7. Verify the Web Application

1. Select `Web Server 1` in the EC2 console.
2. Open the instance's Details tab.
3. Copy the **Public IPv4 DNS** value.
4. Open the DNS name in a browser.

The expected result is the sample web application page, confirming that the web server is accessible.

## 🔐 Security Configuration

The `Web Security Group` permits public HTTP traffic on port `80`. This is appropriate for the web server exercise, but production deployments should use more restrictive access controls where possible and configure HTTPS when required.

## 📚 Key Learnings

- A VPC provides an isolated logical network for AWS resources.
- IPv4 CIDR blocks define the address ranges available to subnets.
- Each subnet belongs to one Availability Zone.
- Public and private subnet behavior depends on routing configuration.
- An Internet Gateway supports direct internet connectivity for appropriately configured public resources.
- A NAT Gateway allows eligible private-subnet resources to initiate outbound internet connections.
- Route table associations determine which routes apply to a subnet.
- Security groups act as stateful virtual firewalls for network interfaces.
- EC2 user data can automate initial software installation and application deployment.
- Instance status checks and browser testing help validate deployment.

## ✅ Result

Created a custom VPC with four subnets, configured public and private route table associations, created a web security group, and launched an EC2 instance configured to run Apache and serve a sample web application.

## 💡 Conclusion

This lab demonstrates the end-to-end process of building AWS networking infrastructure and deploying a web application. It combines VPC design, subnetting, routing, security group configuration, EC2 provisioning, and startup automation into a practical cloud deployment workflow.