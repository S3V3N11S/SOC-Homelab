## Current layout at the time of creating this brief writeup for portfolio purposes, subject to change and has been changed numerous times prior to this. 
# Cybersecurity Homelab Architecture

## Overview
This homelab setup is designed to simulate a secure and monitored network environment for testing cybersecurity tools, practices, and techniques. The lab includes multiple virtual machines for different purposes, interconnected through a private network, yet still allowing internet access. The architecture includes network segmentation, active traffic monitoring, and log aggregation for security event detection and analysis.

## Components

1. **Attacker Machine (Kali Linux)**:
   - **Role**: Acts as the primary attack platform for penetration testing, vulnerability scanning, and network exploitation.
   - **Tools**: Preloaded with offensive tools like Nmap, Metasploit, and custom scripts.

2. **Firewall (OPNSense)**:
   - **Role**: Serves as the core firewall, routing, and network segmentation tool for the lab.
   - **Features**:
     - Provides internet access to internal machines.
     - Configured with **static ARP entries** and **VLANs** to prevent ARP spoofing.
     - Integrated with **Suricata** for intrusion detection and prevention.

3. **Intrusion Detection/Prevention System (Suricata)**:
   - **Role**: Monitors and inspects network traffic in real-time for malicious activity and anomalies.
   - **Deployment**: Installed on **pfSense** in **inline mode** to actively block suspicious traffic.
   - **Features**: Monitors for ARP spoofing, scans, and other network-based attacks.

4. **Security Information and Event Management (Wazuh)**:
   - **Role**: Centralized log collection, security event correlation, and alerting system.
   - **Deployment**: Wazuh agent is deployed across all VMs, and the **Wazuh Manager** aggregates logs and alerts.
   - **Features**:
     - Provides detailed event logging, monitoring host-based intrusion events, file integrity monitoring, and vulnerability detection.
     - Integrated with **Suricata** for network traffic logs and threat intelligence.

5. **Domain Controller (Windows Active Directory)**:
   - **Role**: Manages the domain network, providing authentication services and group policy management.
   - **Features**: Actively monitored by Wazuh for any signs of compromise.

6. **Vulnerable Machines**:
   - **Role**: Targets for penetration testing and exploitation.
   - **VMs**: Ubuntu, Windows, DVWA, VulnHub machines.
   - **Segmentation**: Isolated in separate VLANs (firing range) to minimize exposure and impact of attacks.

## Network Layout
- **OPNSense** acts as the central hub, routing traffic between all internal network segments and the internet.
- **VLANs** are used to isolate different machines and minimize ARP spoofing risks.
- All machines communicate internally over a separate, private network, with **Suricata** monitoring traffic between them.
- Logs from all machines, including Suricata alerts, are forwarded to **Wazuh** for centralized analysis and reporting.

