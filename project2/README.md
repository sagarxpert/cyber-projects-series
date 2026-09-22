<div align="center">

# 💻 MINI HOME SOC LAB

### Project Building series - Project 02


**Proving attacker activity on one machine is visible and attributable in defender-side logs .**

![Level](https://img.shields.io/badge/Level-Easy-brightgreen)
![Status](https://img.shields.io/badge/Status-Complete-success)
![Platform](https://img.shields.io/badge/Platform-VirtualBox-blue)
![OS](https://img.shields.io/badge/Attacker-Kali_Linux-557C94)
![Tool](https://img.shields.io/badge/Logging-Sysmon-0078D4)

</div>

---

## 📌 TL;DR

This project builds a 2-machine SOC lab a Kali attacker VM and a Sysmon-monitored Windows host and proves an attacker's network activity shows up, correctly attributed, in defenderside logs. Includes two real troubleshooting fights (firewall silently eating ICMP, and Sysmon logging nothing until a real process was listening) documented step by step below.

---

## 📖 Table of Contents
- [🎯 Goal](#-goal)
- [🏗️ Architecture](#️-architecture)
- [🧰 Tech Stack](#-tech-stack)
- [1️⃣ Kali Linux VM Setup](#1️⃣-kali-linux-vm-setup)
- [2️⃣ Lab Networking (Host-only + NAT)](#2️⃣-lab-networking-host-only--nat)
- [3️⃣ Sysmon Installation on Host](#3️⃣-sysmon-installation-on-host)
- [4️⃣ Verifying Sysmon Logging](#4️⃣-verifying-sysmon-logging)
- [5️⃣ Cross-Machine Visibility Test](#5️⃣-cross-machine-visibility-test)
- [🐛 Troubleshooting Log](#-troubleshooting-log)
- [✅ Result / Proof of Detection](#-result--proof-of-detection)
- [💡 Lessons Learned](#-lessons-learned)
- [➡️ Next Steps](#️-next-steps)

---

## 🎯 Goal

Build a minimal, resource-constrained home SOC lab that proves a core blue-team fundamental:

> **Attacker activity on one machine is visible and attributable in defender-side logs on another.**

This lab is the foundation for the rest of the series log analysis, a mini SIEM, an attack→detection lab, and a full incident-response lab all build on the environment set up here.

**The constraint that shaped this build:** 8GB RAM total. A separate Windows victim VM was off the table alongside a running Kali VM. Solved by monitoring the **real host laptop** directly instead of virtualizing a second endpoint.

---

## 🏗️ Architecture

```
┌───────────────────────┐         Host-only (192.168.56.0/24)        ┌────────────────────────────┐
│     Kali Linux VM       │ ───────────────────────────────────────►  │     Windows Host (real)      │
│     192.168.56.101       │        (isolated lab network)             │     192.168.56.1               │
│     (Attacker)            │                                            │     Sysmon (Defender/Logging)  │
│                             │                                            │                                  │
│     Adapter 2: NAT ─────────┼──► Internet (updates, tool downloads)      │                                  │
└───────────────────────┘                                            └────────────────────────────┘
```

| Component | Role | RAM Footprint |
|---|---|---|
| 🐉 Kali Linux (VM) | Attacker | 2 GB |
| 💻 Windows laptop (host, native) | Defender / monitored endpoint | 0 GB extra — it's the host |

---

## 🧰 Tech Stack

`VirtualBox` `Kali Linux 2026.2` `Sysmon` `SwiftOnSecurity Sysmon Config` `Windows Event Viewer` `PowerShell` `nmap` `Windows Firewall`

---

## 1️⃣ Kali Linux VM Setup

- **Hypervisor:** VirtualBox
- **Image:** Kali Linux 2026.2, pre-built VirtualBox OVA *(not the ISO — much faster to get running)*
- **Allocated resources:**

| Resource | Value |
|---|---|
| RAM | 2048 MB |
| CPU | 2 cores |
| Disk | 80 GB (dynamically allocated) |

![Kali Linux VM Setup](Screenshots/Kali%20Linux%20VM%20Setup.png)

---

## 2️⃣ Lab Networking (Host-only + NAT)

Two adapters, two jobs — isolation *and* internet access:

| Adapter | Type | Purpose |
|---|---|---|
| Adapter 1 | Host-only | Isolated private link between Kali and host — the "lab network" |
| Adapter 2 | NAT | Internet access for `apt update` / tool downloads |

![Network Adapters](Screenshots/Lab%20Networking%20(Host-only%20+%20NAT).png)

Verified with `ip a` on Kali:

```bash
eth0 → 192.168.56.101/24   # Host-only — the lab network
eth1 → 10.0.3.15/24        # NAT — internet access
```

---

## 3️⃣ Sysmon Installation on Host

Installed **natively on the real Windows laptop** — not inside a VM — to keep resource usage low while still getting real endpoint-level visibility.

<details>
<summary>📋 Click to expand install steps</summary>

1. Downloaded Sysmon from [Microsoft Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
2. Downloaded [SwiftOnSecurity's community Sysmon config](https://github.com/SwiftOnSecurity/sysmon-config) (`sysmonconfig-export.xml`)
   > ⚠️ Grab this via GitHub's **raw file** download button — saving the rendered page directly can save an HTML wrapper instead of the actual XML.
3. Installed via elevated PowerShell:
   ```powershell
   cd C:\Sysmon
   .\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
   ```
4. Verified service status:
   ```powershell
   Get-Service Sysmon*
   ```

</details>

![Sysmon installation on host](Screenshots/Sysmon%20Installation%20on%20Host.png)

---

## 4️⃣ Verifying Sysmon Logging

Confirmed logs were flowing by checking:

```
Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```

**Result:** tens of thousands of events generated from normal background activity — mostly **Event ID 1** (process creation) and **Event ID 13** (registry value set).

---

## 5️⃣ Cross-Machine Visibility Test

The core test of the whole lab: generate real traffic from Kali toward the host, and confirm it's visible and attributable in the host's Sysmon logs.

**Test 1 — Connectivity:**
```bash
# On Kali
ping -c 4 192.168.56.1
```
❌ Initially failed — 100% packet loss, no host-unreachable response. *(See [Troubleshooting Log](#-troubleshooting-log) below.)*

**Test 2 — Scanned connection:**
```bash
# On Kali
nmap -sV -p 8000 192.168.56.1
```
Required a real listening process on the host first — otherwise Windows Firewall drops the packet before any process can accept it, leaving Sysmon nothing to log.

![Cross-machine visibility test — ping and nmap from Kali](Screenshots/cross-machine%20visibility-test.png)

**✅ Confirmed detection** — filtered the host's Sysmon log for Event ID 3 (*Network connection detected*):

![Event 3 detection proof — Kali source IP to host destination IP](Screenshots/eventviewer.png)

```yaml
SourceIp: 192.168.56.101        # ← Kali VM
SourcePort: 60820
DestinationIp: 192.168.56.1     # ← Windows host
DestinationPort: 8080
```

---

## 🐛 Troubleshooting Log

Real issues hit during the build — the kind of thing that doesn't show up in a clean tutorial, but does show up on the job.

### Issue 1 — Ping from Kali to host timed out (100% packet loss)

| | |
|---|---|
| **Symptom** | `ping -c 4 192.168.56.1` from Kali returned 100% packet loss, no error response |
| **Cause** | Windows Firewall default rules block inbound ICMP Echo Request on unclassified/public-profile networks |
| **Fix** | Added a scoped inbound firewall rule allowing ICMP only from the lab subnet |

```powershell
New-NetFirewallRule -DisplayName "Allow ICMP from Lab Network" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -RemoteAddress 192.168.56.0/24 -Action Allow
```

**Result:** ✅ Ping succeeded afterward (0% packet loss).

![Firewall rule created successfully](Screenshots/Troubleshooting%20Log.png)

### Issue 2 — nmap scan generated no Sysmon Event ID 3 logs

| | |
|---|---|
| **Symptom** | After ping worked, an `nmap -sV` scan against the host produced zero matching Sysmon events for Kali's IP |
| **Cause** | Sysmon's Event ID 3 only logs a connection once a **process** actually accepts/initiates it Windows Firewall was silently dropping the scanned ports first, so no process ever saw the connection |
| **Fix** | Started an actual listening process on the host (`python -m http.server 8000`, later tested on port 8080) so there was something real to accept the connection, then re-scanned from Kali |

**Result:** ✅ Event ID 3 appeared, correctly attributing the connection to Kali's source IP and port.

---

## ✅ Result / Proof of Detection

Final confirmed log entry (Sysmon Event ID 3, *Network connection detected*):

| Field | Value |
|---|---|
| SourceIp | `192.168.56.101` (Kali) |
| SourcePort | `60820` |
| DestinationIp | `192.168.56.1` (Windows host) |
| DestinationPort | `8080` |

**This confirms end-to-end visibility:** an action taken on the attacker VM is captured, attributed, and queryable on the defender side.

---

## 💡 Lessons Learned

- 🔥 Default OS firewalls block more than you'd expect — even in an "isolated" lab network, ICMP and unsolicited connections need explicit rules to get through.
- 🕵️ Endpoint logging only logs what actually reaches a process — a scan blocked at the firewall layer produces **no** log entry at all. Blocked isn't the same as invisible, but it does mean Sysmon has nothing to show.
- 💻 Running the "victim" role on a real host instead of a VM is a legitimate way to save resources without losing the learning value — as long as the lab network stays isolated from your real home network.

---

## ➡️ Next Steps

- [ ] **Project 3:** Log Analysis & Threat Hunting using this same Kali + host Sysmon setup, generate broader suspicious activity and practice hunting it in the logs
- [ ] Snapshot the current clean Kali VM state before running heavier attack simulations in later projects

---


# 👤 Author

**Name:** Sagar Singh Shekhawat

**Tweeter:** https://x.com/SagarXploit

**LinkedIn:** https://www.linkedin.com/in/sagar-singh-shekhawat-251a0032a/


---

<div align="center">

### ⭐ Project Building series Project 02

Building a strong foundation for future cybersecurity laboratories and practical learning.

</div>
