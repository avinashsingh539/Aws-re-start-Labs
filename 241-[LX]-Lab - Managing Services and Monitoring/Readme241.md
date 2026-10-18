# 241-[LX]-Lab - Managing Services and Monitoring

## 📌 Overview
This lab focuses on managing Linux services, monitoring system processes, simulating CPU load, and analyzing EC2 performance metrics using Amazon CloudWatch.

## 🎯 Objectives
- Inspect and manage Linux services using `systemctl`.
- Start and stop the Apache HTTP Server.
- Monitor processes and resource utilization using `top`.
- Simulate CPU workload using a shell script.
- Analyze EC2 performance metrics in Amazon CloudWatch.

## 🛠️ Technologies Used
- **Amazon EC2** — Virtual Linux server
- **Amazon Linux** — Operating system
- **Apache HTTP Server** — Web server service
- **systemctl** — Service management
- **top** — Real-time process monitoring
- **Amazon CloudWatch** — Performance monitoring

## 🚀 Tasks Performed

### 1. Connect to the EC2 Instance
Connected to the Amazon Linux instance through SSH.

```bash
ssh -i "labsuser.pem" ec2-user@<PUBLIC_IP>
```

### 2. Manage the Apache HTTP Server

Checked the service status:

```bash
sudo systemctl status httpd.service
```

Started Apache:

```bash
sudo systemctl start httpd.service
```

Verified the service status and accessed the server's public IP through a web browser to check the HTTP response.

Stopped Apache:

```bash
sudo systemctl stop httpd.service
```

**Key Learning:** Linux services can be inspected, started, and stopped using `systemctl`.

### 3. Monitor Processes and CPU Utilization

Used the `top` command to inspect running processes, CPU utilization, and memory usage.

```bash
top
```

Simulated CPU workload and monitored system activity:

```bash
./stress.sh & top
```

Exited the `top` interface by pressing `q`.

**Key Learning:** Process monitoring helps identify resource-intensive processes and understand system behavior under load.

### 4. Monitor EC2 Metrics Using CloudWatch

Navigated to **Amazon CloudWatch → Dashboards → Automatic dashboards → EC2**.

Inspected the available performance metrics, including:

- CPU utilization
- Disk read and write activity
- Network activity

Observed system metrics during the simulated workload and reviewed the CPU utilization graph after the workload completed.

**Key Learning:** Amazon CloudWatch provides visibility into EC2 performance and helps monitor resource usage over time.

## 📚 Key Takeaways
- Managed Linux services with `systemctl`.
- Monitored processes and resource utilization using `top`.
- Simulated CPU load to understand system performance.
- Explored EC2 monitoring dashboards in Amazon CloudWatch.
- Learned how service management and monitoring support Linux administration and troubleshooting.

## ✅ Conclusion
Successfully completed the Managing Services and Monitoring lab, gaining practical experience with Linux service management, process monitoring, simulated CPU workloads, and EC2 performance metrics using Amazon CloudWatch.

---
**Lab:** 241-[LX]-Lab — Managing Services and Monitoring  
**Platform:** AWS Academy / Vocareum