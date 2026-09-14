<div align="center">

# 🛠️ Cybersecurity Lab Setup Guide
### Build a hacker-grade VirtualBox + Kali Linux lab — from scratch

![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
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
| CPU | Core i3 / i5 or equivalent |

</div>

> [!NOTE]
> Specs above are recommended, not mandatory — the lab will just run smoother with them.

---

## 📋 Progress Checklist

- [ ] Step 1 — Install 7-Zip
- [ ] Step 2 — Install VirtualBox
- [ ] Step 3 — Create NAT Network
- [ ] Step 4 — Import Kali Linux
- [ ] Step 5 — Attach Kali to NAT Network
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

### `02` Install VirtualBox
Grab the latest recommended version for your OS.

📥 **[Download VirtualBox →](https://virtualbox.org/wiki/Downloads)**

<br>

### `03` Create a NAT Network
This gives your VM internet access while keeping it isolated from your host.

1. Open VirtualBox → **File → Tools → Network**
2. Go to the **NAT Networks** tab → click **Add**
3. Configure:

```
IPv4 Prefix : 10.0.0.0/24
Enable DHCP : ✅
```

<br>

### `04` Download & Import Kali Linux
📥 **[Get Kali Linux →](https://kali.org/get-kali)** *(grab the VirtualBox pre-built image)*

Import it via **File → Import Appliance**.

<br>

### `05` Attach Kali to the NAT Network
1. Select your Kali VM → **Settings → Network**
2. **Adapter 1 → Attached to:** `NAT Network`
3. **Name:** the network you created in Step 3

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
> If the internet doesn't work with `8.8.8.8`, switch the DNS server to `10.0.0.1` instead.

<br>

### `07` Enable Clipboard & Drag-and-Drop
**Settings → General → Advanced**

```
Shared Clipboard : Bidirectional
Drag'n'Drop      : Bidirectional
```

<br>

### `08` Enable Shared Folders
**Settings → Shared Folders** → add your host's `Downloads` folder → check **Auto-mount** ✅

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

**Machine → Take Snapshot**

---

## ⚠️ Troubleshooting: No Internet in Kali?

> [!WARNING]
> This is a **known issue** on VirtualBox v7 + Kali 2026.1 or newer.

Try these, in order:

1. Double-check the NAT Network was created correctly *(Step 3)*
2. Confirm no other VM on the same network is also using `10.0.0.2`
3. Run these three commands inside Kali, then restart it:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

4. Restart VirtualBox, then your host machine, if the issue persists

---

## 🔮 Optional Next Step

Once this base lab is stable, expand it with additional VMs (Windows 10/11, Server 2016, Android) — all on the same NAT range. Sets you up for multi-machine attack/defense scenarios in later projects of this series.

---

<div align="center">

### 🚧 Part of a 10-Project Cybersecurity Series 🚧

**Learning together · Building together · Growing together**

[X (Twitter)](#) · [LinkedIn](#) · [GitHub](#)

</div>
