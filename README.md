# Network Scan Report

This project contains a reconnaissance exercise using Nmap to identify open ports, active services, and potential security risks in a local network.

## 1. Overview

A TCP SYN scan was performed on the target network:

nmap -sS 192.20.10.0/24

yaml
Copy code

Four hosts responded. The goal was to enumerate open ports, map services, and assess exposure.

---

## 2. Host Findings

### Host: 192.20.10.1

**Open Ports**
- 21/tcp — FTP  
- 53/tcp — DNS  
- 49152/tcp — Dynamic/Ephemeral  
- 62078/tcp — Device sync protocol  

**Risk Summary**
- FTP uses clear-text authentication.  
- DNS exposure may allow poisoning or misuse.  
- High dynamic ports may expose internal RPC functions.  
- Sync services should not be exposed across the LAN.

---

### Host: 192.20.10.2

This system exposes a large number of services and looks like a lab VM or intentionally vulnerable machine.

**Open Ports (Notable)**
- 21 — FTP  
- 22 — SSH  
- 23 — Telnet  
- 25 — SMTP  
- 53 — DNS  
- 80 — HTTP  
- 111 — rpcbind  
- 139/445 — SMB  
- 512/513/514 — r-services  
- 1099 — Java RMI  
- 1524 — ingreslock  
- 2049 — NFS  
- 3306 — MySQL  
- 5432 — PostgreSQL  
- 5900 — VNC  
- 6000 — X11  
- 6667 — IRC  
- 8009 — AJP13  

**Risk Summary**
- Multiple insecure legacy services (FTP, Telnet, r-services).  
- SMB, NFS, and RPC expand attack surface.  
- Databases are exposed to the entire network.  
- VNC and X11 allow remote access if unprotected.  
- Java RMI exposes deserialization-based attack surfaces.  
- Overall a high-risk host.

---

### Host: 192.20.10.5

**Open Ports**
- 135 — MSRPC  
- 139/445 — SMB  
- 3306 — MySQL  

**Risk Summary**
- SMB is a common ransomware target.  
- MSRPC has a long history of remote exploits.  
- MySQL should not be exposed on a local network without access restrictions.

---

### Host: 192.20.10.11

All scanned ports were closed.

**Risk Summary**
- Low risk.  
- Likely behind a firewall or hardened.

---

## 3. Common Services and Risks

| Port | Service | Description | Risk |
|------|---------|-------------|------|
| 21 | FTP | File transfer | Clear-text credentials |
| 22 | SSH | Secure shell | Weak passwords are risky |
| 23 | Telnet | Remote login | Not encrypted |
| 25 | SMTP | Mail server | Can be abused for spam |
| 53 | DNS | Name resolution | Poisoning/misconfig risks |
| 80 | HTTP | Web server | Web exploits |
| 111 | rpcbind | RPC mapping | Classic RPC exploits |
| 139/445 | SMB | File sharing | Ransomware target |
| 2049 | NFS | File share | Exposes directories |
| 3306 | MySQL | Database | Should be restricted |
| 5432 | PostgreSQL | Database | Should be restricted |
| 5900 | VNC | Remote desktop | Weak auth takeover |
| 6000 | X11 | Display server | Screen/keystroke capture |
| 1099 | Java RMI | Remote execution | Deserialization attacks |

---

## 4. Key Takeaways

- Several hosts expose unnecessary or insecure services.  
- Clear-text protocols (FTP, Telnet, r-services) are high-risk.  
- SMB, VNC, and databases should be restricted to trusted systems.  
- Host **192.20.10.2** is a vulnerable lab VM metasploit2.

---

## 5. Recommendations

- Disable unused services.  
- Apply firewall rules to restrict access.  
- Replace FTP/Telnet with SSH/SFTP.  
- Restrict database ports to trusted clients.  
- Keep systems updated.  
- Isolate high-exposure hosts.
