<div align="center">

# 🛠️ Cybersecurity Lab Setup Guide
### Build a hacker-grade VMware + Kali Linux lab — from scratch

![VMware](https://img.shields.io/badge/VMware_Workstation-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-🟢_Easy-brightgreen?style=for-the-badge)

</div>

---

## 🎯 What You'll Build

By the end of this guide, you'll have:

- ✅ An isolated virtual lab, fully cut off from your main system
- ✅ **Kali Linux** running as your attack machine
- ✅ A private NAT network (`10.0.0.0/24`) — with full internet access
- ✅ Clipboard + drag-and-drop working between host and VM

<br>

<div align="center">

| 💻 Component | 🔧 Requirement |
|:---:|:---:|
| RAM | 8 GB or more |
| Storage | 256 GB SSD or more |
| CPU | Core i3 / i5 or equivalent (VT-x/AMD-V enabled in BIOS) |

</div>

> [!NOTE]
> Specs above are recommended, not mandatory — the lab will just run smoother with them.

---

## 📋 Progress Checklist

- [ ] Step 1 — Install 7-Zip
- [ ] Step 2 — Install VMware Workstation
- [ ] Step 3 — Configure the NAT network (VMnet8)
- [ ] Step 4 — Import Kali Linux
- [ ] Step 5 — Attach Kali to the NAT network
- [ ] Step 6 — Set Kali's static IP
- [ ] Step 7 — Enable clipboard & drag/drop
- [ ] Step 8 — Enable shared folders
- [ ] Step 9 — Verify internet access
- [ ] Step 10 — Take a snapshot

---

## 🚀 Step-by-Step Guide

### `01` Install 7-Zip
Needed to extract the Kali Linux archive later.

📥 **[Download 7-Zip →](https://7-zip.org/download.html)**

<br>

### `02` Install VMware Workstation
Grab **VMware Workstation Pro** (free for personal use) or **Player**.

📥 **[Download VMware Workstation →](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)**

<br>

### `03` Configure the NAT Network (VMnet8)
VMware uses **VMnet8** as its default NAT network you just need to set the subnet.

1. Open **Edit → Virtual Network Editor** *(may need "Change Settings" / run as Administrator on Windows)*
2. Select **VMnet8** (Type: `NAT`)
3. Click **NAT Settings** and confirm/set the subnet:

```
Subnet IP     : 10.0.0.0
Subnet Mask   : 255.255.255.0
```

4. Ensure **"Use local DHCP service to distribute IP addresses to VMs"** is checked (or leave unchecked if you plan to assign static IPs manually, as in Step 6)

<br>

### `04` Download & Import Kali Linux
📥 **[Get Kali Linux →](https://kali.org/get-kali)** *(grab the VMware pre-built image)*

Extract it with 7-Zip, then **File → Open** in VMware and select the `.vmx` file or use **File → Import** for an OVA.

<br>

### `05` Attach Kali to the NAT Network
1. Select your Kali VM → **VM → Settings → Network Adapter**
2. Choose **NAT** *(this maps to VMnet8)*
3. Click **OK**

<br>

### `06` Set Kali's Static IP
Inside Kali → top-right network icon → **Edit Connections → IPv4 Settings**

<div align="center">

| Setting | Value |
|:---|:---|
| Method | `Manual` |
| Address | `10.0.0.2` |
| Netmask | `24` |
| Gateway | `10.0.0.1` |
| DNS | `8.8.8.8` |

</div>

> [!TIP]
> If the internet doesn't work with `8.8.8.8`, switch the DNS server to `10.0.0.1` instead. On VMware, `10.0.0.1` is typically also the NAT gateway assigned to VMnet8.

<br>

### `07` Enable Clipboard & Drag-and-Drop
**VM → Settings → Options → Guest Isolation**

```
Enable drag and drop     : ✅
Enable copy and paste    : ✅
```

> [!NOTE]
> This requires **VMware Tools** to be installed inside the Kali guest. Install it via **VM → Install VMware Tools** if it's not already present.

<br>

### `08` Enable Shared Folders
**VM → Settings → Options → Shared Folders**

1. Select **Always enabled**
2. Click **Add** → point it to your host's `Downloads` folder
3. Shared folders will appear inside Kali under `/mnt/hgfs/`

<br>

### `09` Verify Internet Access
Open a terminal in Kali:

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

✅ Both should return replies with **0% packet loss**.

<br>

### `10` Take a Snapshot
Lock in your clean, working state so you can always roll back.

**VM → Snapshot → Take Snapshot**

---

## ⚠️ Troubleshooting: No Internet in Kali?

> [!WARNING]
> Common causes: VMnet8 misconfiguration, VMware Tools missing, or a stale IP lease.

Try these, in order:

1. Reopen **Virtual Network Editor** and confirm VMnet8's subnet is exactly `10.0.0.0/24`
2. Confirm no other VM on the same network is also using `10.0.0.2`
3. Run these three commands inside Kali, then restart it:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

4. Restart the **VMware NAT service** on your host (Windows: `services.msc` → restart *VMware NAT Service*), then restart VMware and your host machine if the issue persists

---

## 🔮 Optional Next Step

Once this base lab is stable, expand it with additional VMs (Windows 10/11, Server 2016, Android) — all on the same NAT range (VMnet8), or add a second **Host-only** network (e.g. VMnet2) for isolated attack/defense scenarios in later projects of this series.

---

<div align="center">

### 🚧 Part of a 10-Project Cybersecurity Series 🚧

**Learning together · Building together · Growing together**

[X (Twitter)](#) · [LinkedIn](#) · [GitHub](#)

</div>
