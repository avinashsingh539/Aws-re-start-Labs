# Lab 265 — Internet Protocol Troubleshooting Commands

## 📌 Overview
This lab focuses on using Linux networking commands to troubleshoot connectivity and understand network communication across different layers of the OSI model. The commands help diagnose connectivity issues, trace network paths, inspect TCP connections, test ports, and analyze HTTP/HTTPS requests.

## 🎯 Objectives
- Test network connectivity using `ping`.
- Trace the path packets take using `traceroute`.
- Inspect established TCP connections and listening ports using `netstat`.
- Test remote TCP port connectivity using `telnet`.
- Analyze HTTPS requests and responses using `curl`.

## 🛠️ Environment
- **Platform:** AWS Academy / Vocareum
- **Compute:** Amazon EC2 Linux instance
- **Access:** SSH
- **Tools:** `ping`, `traceroute`, `netstat`, `telnet`, `curl`

## 🚀 Lab Implementation

### 1. Connect to the EC2 Instance

Retrieve the instance's public IP address from the Vocareum lab details and connect through SSH:

```bash
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@<PUBLIC_IP>
```

Replace `<PUBLIC_IP>` with the actual public IP address of the EC2 instance.

### 2. Layer 3 — Network Connectivity

**Test connectivity with `ping`:**

```bash
ping 8.8.8.8 -c 5
```

- Sends five ICMP echo requests to the destination.
- Helps verify IP-level connectivity.
- Packet loss or timeouts may indicate connectivity issues.

**Trace the network path with `traceroute`:**

```bash
traceroute 8.8.8.8
```

- Displays the intermediate network hops toward the destination.
- Helps investigate routing and path-related issues.
- Some routers may not respond to traceroute probes.

### 3. Layer 4 — TCP Connections and Listening Ports

**Display established TCP connections:**

```bash
netstat -tp
```

Shows TCP connections and associated process information when available.

**Display listening TCP services:**

```bash
netstat -tlp
```

Lists TCP ports on which services are listening.

**Display listening ports without resolving names:**

```bash
netstat -ntlp
```

Uses numeric addresses and port numbers, making the output easier to inspect during troubleshooting.

### 4. Test TCP Connectivity Using Telnet

Install Telnet:

```bash
sudo yum install telnet -y
```

Test connectivity to Google's HTTP port:

```bash
telnet www.google.com 80
```
- To Escape type '^]'.

- Tests whether a TCP connection can be established to port `80`.
- A successful connection indicates that the TCP connection to the destination port was established.
- A failed connection may indicate routing, firewall, security group, or destination-side restrictions.

### 5. Layer 7 — HTTP/HTTPS Troubleshooting

Execute a verbose HTTPS request:

```bash
curl -vLo /dev/null https://aws.com
```

This command:
- `-v` — Displays detailed request and connection information.
- `-L` — Follows HTTP redirects.
- `-o /dev/null` — Discards the response body.
- `https://aws.com` — Specifies the HTTPS destination.

Additional useful options:

```bash
curl -I https://aws.com
curl -i https://aws.com
```

- `-I` — Requests and displays HTTP response headers.
- `-i` — Includes response headers in the output along with the response body.

## 📚 Key Learnings

| Command | Purpose | Troubleshooting Layer |
|---|---|---|
| `ping` | Tests IP-level connectivity | Layer 3 — Network |
| `traceroute` | Identifies the route and intermediate hops | Layer 3 — Network |
| `netstat -tp` | Inspects established TCP connections | Layer 4 — Transport |
| `netstat -tlp` | Displays listening TCP services | Layer 4 — Transport |
| `netstat -ntlp` | Displays numeric listening addresses and ports | Layer 4 — Transport |
| `telnet` | Tests TCP connectivity to a remote port | Layer 4 — Transport |
| `curl` | Examines HTTP/HTTPS communication | Layer 7 — Application |

## ✅ Result

Successfully practiced essential Linux networking troubleshooting commands on an AWS EC2 instance to investigate IP connectivity, network routes, TCP connections, listening ports, and HTTP/HTTPS communication.

## 💡 Conclusion

This lab demonstrated how to troubleshoot network issues systematically, moving from basic IP connectivity checks to transport-layer port testing and application-layer request analysis. These commands are useful when diagnosing connectivity problems in cloud environments, particularly when investigating EC2 networking and service accessibility.