# pfSense IDS/IPS Network Security Lab

## Overview
This lab demonstrates the implementation of a secure virtual network using pfSense firewall, Snort IDS, Squid proxy, Kali Linux, and a Windows host. The environment was designed to simulate a real network security monitoring scenario where threats are detected, analyzed, and mitigated.

The objective of this lab was to identify network vulnerabilities, detect intrusion attempts, and implement security controls using firewall rules and monitoring tools. :contentReference[oaicite:0]{index=0}

---

## Lab Architecture

The virtual network consisted of three main machines:

- **pfSense Firewall (Gateway)** – 192.168.100.1
- **Kali Linux (Attacker Machine)** – 192.168.100.100
- **Windows 10 (Target Machine)** – 192.168.100.101

The pfSense firewall acted as the gateway and security control point for all network traffic. :contentReference[oaicite:1]{index=1}

Network services implemented:

- Snort IDS
- Squid Web Proxy
- pfSense Firewall Rules
- ClamAV antivirus

---

## Tools Used

- pfSense Firewall
- Snort IDS
- Squid Proxy
- Kali Linux
- Nmap
- Windows 10
- VirtualBox / Virtual Network Environment

---

## Security Tests Performed

### 1. Intrusion Detection using Snort

A ping sweep was launched from Kali Linux to detect hosts in the internal network:
sudo nmap -sn 192.168.100.0/24

Snort successfully detected the ICMP scan and generated alerts indicating reconnaissance activity. :contentReference[oaicite:2]{index=2}

This demonstrates how IDS tools can detect network scanning behavior commonly used by attackers.

---

### 2. Web Access Control using Squid Proxy

The Squid proxy server was configured on pfSense to control web access from the internal network.

A blacklist rule was added to block access to:
www.facebook.com

Testing from the Windows machine confirmed that the website was successfully blocked through the proxy filter. :contentReference[oaicite:3]{index=3}

This demonstrates how proxy filtering can enforce organizational browsing policies.

---

### 3. Vulnerability Scanning and Port Mitigation

An Nmap SYN scan was executed from Kali Linux to identify open ports on the Windows host:
sudo nmap -sS 192.168.100.101

Several open ports were discovered.

Mitigation steps included:

- Blocking ports using pfSense firewall rules
- Disabling unnecessary services on the Windows host

A second Nmap scan confirmed that the previously open ports were filtered or closed. :contentReference[oaicite:4]{index=4}

---

### 4. Firewall Rule Enforcement

A firewall rule was configured in pfSense to block ICMP traffic from Kali Linux.

Testing with:
ping 192.168.100.1

The ping requests were successfully dropped, demonstrating the effectiveness of firewall filtering. :contentReference[oaicite:5]{index=5}

---

## Results

Key security outcomes of the lab:

- Intrusion detection alerts triggered by network scans
- Web filtering successfully blocked unauthorized sites
- Vulnerable open ports were identified and mitigated
- Firewall rules effectively blocked ICMP traffic

These controls improved the security posture of the virtual network.

---

## Skills Demonstrated

- Network Security Monitoring
- Intrusion Detection Systems (IDS)
- Firewall Configuration
- Vulnerability Scanning
- Network Traffic Analysis
- Security Hardening

---

## Future Improvements

Potential enhancements for the lab include:

- Continuous traffic monitoring
- SIEM integration
- Automated alert response
- Threat intelligence rule updates
