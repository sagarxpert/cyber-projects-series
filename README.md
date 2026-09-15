<div align="center">

# Cyber Projects Series

**A 10-Part Hands-on Cybersecurity Lab Journey from Foundation to Advanced Detection**

[![Series Progress](https://img.shields.io/badge/Progress-1%2F10%20Completed-00ffcc?style=for-the-badge&logo=target)](https://github.com/sagarxpert/cyber-projects-series)
[![Status](https://img.shields.io/badge/Status-Active%20Build-blue?style=for-the-badge)](https://github.com/sagarxpert/cyber-projects-series)
[![Focus](https://img.shields.io/badge/Focus-SOC%20%7C%20Detection%20Engineering-red?style=for-the-badge)](https://github.com/sagarxpert/cyber-projects-series)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

<br />

<p align="center">
  <!-- Place your generated preview banner in an 'assets/' folder -->
  <img src="./assets/banner.png" alt="Cyber Projects Series Banner" width="850px" />
</p>

[Explore Labs](#-project-roadmap) • [Tech Stack](#-tools--technologies) • [Build Along](#-build-along-with-me) • [Connect](#-connect--community)

</div>

---

## 🎯 About The Series

Welcome to the **Cyber Projects Series**! 

I am a Computer Science Engineering student specializing in Cybersecurity, working toward becoming a **SOC Analyst** and **Detection Engineer**. To bridge the gap between abstract security theory and applied defensive operations, I created this 10-part project roadmap.

Every project in this repository documents a real-world lab scenario—moving step-by-step from core networking and virtualization up to enterprise SIEM telemetry, threat hunting, and automated detection rule development.

### Why in Public?
* **Transparent Execution:** Documenting successes, edge cases, misconfigurations, and key takeaways without filtering out the learning curves.
* **Recruiter-Ready Artifacts:** Providing auditable, production-grade evidence of analytical problem-solving and telemetry engineering.
* **Community-First:** Breaking down gatekeeping in cybersecurity by giving beginners reproducible guides to run their own home labs.

---

## 🧭 Project Roadmap

| # | Project Name | Primary Focus | Difficulty | Status | Documentation |
| :-: | :-- | :-- | :-: | :-: | :-: |
| **01** | **Cybersecurity Lab Environment Setup** | VMware Pro, Virtual Networking & Kali | `Easy` | `Completed` | [View Lab →](./01-lab-setup/) |
| **02** | **Network Traffic Analysis & Packet Inspection** | Wireshark, Tshark & Protocol Analysis | `Easy` | `Planned` | [Pending] |
| **03** | **Vulnerability Assessment & Scanning** | Nessus, Nmap & Attack Surface Profiling | `Easy` | `Planned` | [Pending] |
| **04** | **Centralized Log Collection & Syslog Pipe** | Linux Auditing & Central Log Management | `Medium` | `Planned` | [Pending] |
| **05** | **SIEM Deployment & Telemetry Ingestion** | Splunk Enterprise / Elastic Stack Pipeline | `Medium` | `Planned` | [Pending] |
| **06** | **SOC Alert Triage & Incident Investigation** | Threat Hunting, Log Correlation & Timeline Analysis | `Medium` | `Planned` | [Pending] |
| **07** | **Endpoint Detection & Response (EDR) Architecture** | Wazuh EDR / Sysmon Telemetry Engineering | `Medium` | `Planned` | [Pending] |
| **08** | **Offensive TTP Simulation vs. Detection Engineering** | Atomic Red Team, Sigma Rules & MITRE ATT&CK | `Hard` | `Planned` | [Pending] |
| **09** | **Digital Forensics & Artifact Extraction** | Memory Forensics (Volatility) & Disk Analysis | `Hard` | `Planned` | [Pending] |
| **10** | **Automated Incident Response & Threat Triage Pipeline** | SOAR Concepts, Python Detection Scripts & Playbooks | `Hard` | `Planned` | [Pending] |

---

## 🛠️ Tools & Technologies

<br />

[![VMware](https://img.shields.io/badge/VMware-Workstation%20Pro-gray?style=flat-square&logo=vmware&logoColor=white)](https://www.vmware.com)
[![Kali Linux](https://img.shields.io/badge/OS-Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white)](https://www.kali.org)
[![Linux](https://img.shields.io/badge/OS-Linux%20Debian%2FRedHat-FCC624?style=flat-square&logo=linux&logoColor=black)](https://www.kernel.org)
[![Wireshark](https://img.shields.io/badge/Analysis-Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)](https://www.wireshark.org)
[![Splunk](https://img.shields.io/badge/SIEM-Splunk-000000?style=flat-square&logo=splunk&logoColor=white)](https://www.splunk.com)
[![Python](https://img.shields.io/badge/Scripting-Python%203-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org)

---

## 🤝 Build Along With Me

Cybersecurity is best learned by getting your hands dirty on the terminal. You don't need an enterprise data center; a standard home laptop running isolated virtual subnets is enough to master detection engineering fundamentals.

If you are following along:
1. **Fork the Repository:** Keep track of your own lab iterations and notes.
2. **Replicate the Labs:** Read the documentation in each folder for hardware prerequisites and step-by-step setup commands.
3. **Share Your Iteration:** Drop a tag on [LinkedIn](https://linkedin.com) or [X (Twitter)](https://x.com/sagarxpert) when you finish a lab using the hashtag `#CyberProjectsSeries`. Feedback, alternative architecture proposals, and PRs are always welcome!

<details>
<summary>💡 Suggested Lab Hardware Prerequisites</summary>

* **CPU:** 4+ Cores (VT-x / AMD-V virtualization enabled)
* **RAM:** 16 GB minimum recommended for running nested attacker + defender environments
* **Storage:** 100 GB SSD free space for virtual hard disks and PCAP snapshots
* **Hypervisor:** VMware Workstation Pro (now free for personal use) or VirtualBox
</details>

---

## 📬 Connect & Community

Let's exchange detection engineering notes, SOC workflows, and threat intel:

[![GitHub](https://img.shields.io/badge/GitHub-sagarxpert-181717?style=for-the-badge&logo=github)](https://github.com/sagarxpert)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com)
[![X](https://img.shields.io/badge/X-Follow%20%40sagarxpert-000000?style=for-the-badge&logo=x)](https://x.com/sagarxpert)

---

<div align="center">
  <sub>All labs are built in strictly isolated, sandboxed virtual environments for educational and defensive threat research purposes only.</sub>
</div>
