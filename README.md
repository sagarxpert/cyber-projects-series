
<div align="center">

# 💻 CYBERSECURITY LAB ENVIRONMENT SETUP

### Project Building series - Project 01

### Kali Linux • VMware Workstation • Virtual Networking

![VMware](https://img.shields.io/badge/VMware-Workstation-orange)
![Kali Linux](https://img.shields.io/badge/Kali-Linux-blue)
![Windows 11](https://img.shields.io/badge/Host-Windows%2011-success)
![Cybersecurity](https://img.shields.io/badge/Domain-Cybersecurity-red)
![Project Series](https://img.shields.io/badge/Project-KaliSetup-purple)

**Building a controlled cybersecurity laboratory environment using VMware Workstation and Kali Linux.**

</div>

---

# 📌 Project Overview

This project documents the setup of a personal cybersecurity laboratory environment created during **Week 01** of my **Project Building Series**.

The objective was to build a controlled virtual environment that can be safely used for future cybersecurity learning, testing, and practical exercises.

The lab was designed using:

- VMware Workstation
- Kali Linux Virtual Machine
- NAT Networking
- Host-Only Networking
- Network Verification Testing
- Snapshot-Based Recovery

---

# 🎯 Lab Purpose

Cybersecurity activities often require isolated environments where tools and configurations can be tested without affecting the host operating system.

Virtualization provides:

- Safe experimentation
- Environment isolation
- Easy recovery
- Repeatable lab setups
- Network segmentation

For this lab:

- **VMnet8 (NAT)** provides Internet connectivity.
- **VMnet2 (Host-Only)** provides an isolated laboratory network.

This design allows Internet access while maintaining a separate network segment for future cybersecurity exercises.

---

# 🖥️ Host Environment

| Component | Details |
|------------|----------|
| Operating System | Windows 11 |
| Processor | Intel Core i5-12500H |
| Memory | 16 GB RAM |
| Storage | 512 GB SSD |
| Graphics | NVIDIA RTX 3050 |
| Virtualization Platform | VMware Workstation |

---

# 🧰 Technologies Used

| Tool | Purpose |
|--------|----------|
| VMware Workstation | Virtualization Platform |
| Kali Linux | Cybersecurity Operating System |
| VMware Virtual Networks | Network Segmentation |
| Linux Networking Tools | Network Verification |
| VMware Snapshots | Recovery & Rollback |

---

# 🏗️ Lab Architecture

```text
                 Windows 11 Host
                         |
                         v
                VMware Workstation
                         |
      +---------------------------------------+
      |                                       |
      v                                       v

 VMnet8 (NAT)                         VMnet2 (Host-Only)
192.168.72.0/24                        10.0.0.0/24

      |                                       |
      v                                       v

eth0                                 eth1
192.168.72.128/24                    10.0.0.129/24

       \                             /
        \                           /
         +-------------------------+
         |       Kali Linux        |
         +-------------------------+
```

### Network Design

| Network | Purpose |
|----------|----------|
| VMnet8 | Internet Connectivity |
| VMnet2 | Isolated Lab Network |
| eth0 | Connected to VMnet8 |
| eth1 | Connected to VMnet2 |

---

# 🚀 Step 1 — VMware Workstation Setup

A Kali Linux virtual machine was deployed using VMware Workstation.

📸 <img width="1839" height="910" alt="Screenshot 2026-08-13 234703" src="https://github.com/user-attachments/assets/e2162b46-477b-4e45-b860-2f22b0c7b2f3" />



**Suggested Screenshot:**
- VMware Workstation Home Screen

---

# 🚀 Step 2 — Kali Linux Virtual Machine

A Kali Linux virtual machine was created using VMware's available Kali Linux virtual machine option.

📸 <img width="1012" height="815" alt="Screenshot 2026-08-13 224808" src="https://github.com/user-attachments/assets/f3ad845a-eb08-4984-a947-f6f3b347f074" />


**Suggested Screenshot:**
- Kali Linux VM Overview Page

---

# 🚀 Step 3 — Virtual Machine Hardware Configuration

The Kali Linux virtual machine was configured with dedicated virtual hardware resources.

| Resource | Value |
|-----------|--------|
| RAM | 4 GB |
| CPU | 4 Cores |
| Disk | 80 GB |
| Network Adapters | 2 |

📸 <img width="877" height="869" alt="Screenshot 2026-08-13 235235" src="https://github.com/user-attachments/assets/5a654a37-d5fc-4824-ab01-cc63c38c60a6" />



**Suggested Screenshot:**
- VMware Hardware Configuration Screen

---

# 🌐 Step 4 — VMware Virtual Network Configuration

## VMnet8 (NAT)

Used for Internet access.

| Setting | Value |
|-----------|---------|
| Type | NAT |
| Network | 192.168.72.0/24 |
| Gateway | 192.168.72.2 |

📸 <img width="895" height="922" alt="Screenshot 2026-08-13 223142" src="https://github.com/user-attachments/assets/6a677610-27ad-481c-851c-a5d2942686bf" />


**Suggested Screenshot:**
- VMware Network Adapter 1 Configuration

---

## VMnet2 (Host-Only)

Used as an isolated laboratory network.

| Setting | Value |
|-----------|---------|
| Type | Host-Only |
| Network | 10.0.0.0/24 |

📸 <img width="895" height="922" alt="Screenshot 2026-08-13 223147" src="https://github.com/user-attachments/assets/1a3937ef-930c-473d-bb02-95a2df50883a" />


**Suggested Screenshot:**
- VMware Network Adapter 2 Configuration

---

## VMware Virtual Network Editor

Verification of VMware virtual network configuration.

📸 <img width="700" height="703" alt="Screenshot 2026-08-13 223201" src="https://github.com/user-attachments/assets/5b9749d7-755e-4809-ab5d-caef731145c4" />


**Suggested Screenshot:**
- VMware Virtual Network Editor showing VMnet8 and VMnet2

---

# 🌐 Step 5 — Kali Network Configuration

After configuring the second network adapter, Kali Linux detected two interfaces.

| Interface | Address |
|------------|-----------|
| eth0 | 192.168.72.128/24 |
| eth1 | 10.0.0.129/24 |

### Verify Interfaces

```bash
ip addr
```

Expected Result:

```text
eth0 -> 192.168.72.128/24
eth1 -> 10.0.0.129/24
```

📸 <img width="1635" height="725" alt="Screenshot 2026-08-13 224923" src="https://github.com/user-attachments/assets/677b60f9-db14-4289-a5e3-d60740ce78a3" />


**Suggested Screenshot:**
- Output of ip addr

---

# 🌐 Step 6 — Routing Verification

The routing table confirmed:

- Internet traffic uses eth0
- Lab traffic uses eth1

Command:

```bash
ip route
```

Observed Routing Table:

```text
default via 192.168.72.2 dev eth0 proto dhcp src 192.168.72.128 metric 100

10.0.0.0/24 dev eth1 proto kernel scope link src 10.0.0.129 metric 101

192.168.72.0/24 dev eth0 proto kernel scope link src 192.168.72.128 metric 100
```

📸 <img width="1320" height="193" alt="Screenshot 2026-08-13 224951" src="https://github.com/user-attachments/assets/307216bb-e763-447a-b4a9-fc000759c34d" />


**Suggested Screenshot:**
- Output of ip route

---

# ✅ Step 7 — Connectivity Testing

## Internet Connectivity Test

Command:

```bash
ping -c 4 8.8.8.8
```

Result:

```text
4 packets transmitted
4 received
0% packet loss
```

## DNS Resolution Test

Command:

```bash
ping -c 4 google.com
```

Result:

```text
Replies successfully received
```

📸 <img width="1360" height="739" alt="Screenshot 2026-08-13 225036" src="https://github.com/user-attachments/assets/e7a9ad7c-a550-4cff-a228-2cbed62f59b1" />
 

**Suggested Screenshot:**
- Ping 8.8.8.8
- Ping google.com

---

# 🔄 Snapshot & Recovery

After completing the base laboratory configuration, a VMware snapshot was created (or planned) to provide a recovery point.

Benefits:

- Safe experimentation
- Easy rollback
- Malware analysis preparation
- Rapid recovery from misconfigurations

📸 <img width="420" height="287" alt="Screenshot 2026-08-13 235700" src="https://github.com/user-attachments/assets/5655ddf4-655e-452d-9571-a1c454c64361" />


**Suggested Screenshot:**
- VMware Snapshot Manager or Snapshot Creation Screen

---

# ⚠️ Problems Encountered & Solutions

| Problem | Solution |
|-----------|-----------|
| Reference implementation used VirtualBox | Adapted the setup for VMware Workstation |
| Host-only lab network was not initially available inside Kali | Added a second VMware network adapter |
| Kali initially only showed eth0 | Configured VMnet2 and attached it to the VM |
| Ping to 10.0.0.1 failed before host-only setup | Verified network adapter configuration and routing |

### Key Observation

Before the second adapter was configured:

```text
eth0 = 192.168.72.128
```

After adding VMnet2:

```text
eth1 = 10.0.0.129
```

The routing table then displayed:

```text
10.0.0.0/24 dev eth1
```

confirming successful connection to the isolated laboratory network.


---

# 📚 Key Learning Outcomes

- VMware Workstation fundamentals
- Virtual machine deployment
- Network segmentation
- NAT networking
- Host-only networking
- Linux interface management
- Routing verification
- Connectivity troubleshooting
- Cybersecurity lab design principles

---

# 🔐 Security & Ethics

This laboratory environment was created exclusively for:

- Educational purposes
- Cybersecurity learning
- Network experimentation
- Defensive security practice

All activities should be performed only in authorized environments and in accordance with applicable laws, policies, and ethical guidelines.

---

# 👨‍💻 Project Information

| Item | Details |
|--------|----------|
| Project Building Series | project 1 |
| Week | Week 01 |
| Focus Area | Cybersecurity Lab Environment Setup |

---


# 👤 Author

**Name:** Sagar Singh Shekhawat

**Tweeter:** https://x.com/SagarXploit

**LinkedIn:** https://www.linkedin.com/in/sagar-singh-shekhawat-251a0032a/


---

<div align="center">

### ⭐ Project Building series — Project 01

Building a strong foundation for future cybersecurity laboratories and practical learning.

</div>
