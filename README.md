# 🖥️ Home Server Setup on GMKtec Mini PC (Ubuntu 24.04 LTS)

This document captures the complete process of setting up a home server on a GMKtec M2 Pro Mini PC using Ubuntu Server 24.04 LTS. The server is configured for SSH access and ready for hosting services like static websites, file sync, etc.

---

## ✅ Hardware Used

* **Mini PC**: GMKtec M2 Pro

  * Intel Core i7-1195G7
  * 32GB DDR4 RAM
  * 1TB NVMe SSD
* Pre-installed OS: Windows 11 Pro (later wiped)

---

## ✅ Goal

Set up a clean Ubuntu Server environment ready for self-hosting static websites, document editing tools (Nextcloud), and remote access via SSH.

---

## 🔧 Pre-Setup Work (on Mac)

### 1. **Download Ubuntu Server ISO**

* Ubuntu Server 24.04.2 LTS
* URL: [https://ubuntu.com/download/server](https://ubuntu.com/download/server)

### 2. **Flash ISO to USB (on Mac)**

```bash
# Unmount the USB drive first (assuming it's /dev/disk4)
diskutil unmountDisk /dev/disk4

# Use dd to flash the ISO (replace with actual path)
sudo dd if=~/Downloads/ubuntu-24.04.2-live-server-amd64.iso of=/dev/rdisk4 bs=1m status=progress

# Eject the USB safely
diskutil eject /dev/disk4
```

---

## 💽 Installing Ubuntu on Mini PC

### 1. **Boot from USB**

* Plug in USB, power on the Mini PC
* Press `F7`, `ESC`, or `DEL` to enter boot menu
* Select USB drive to boot Ubuntu installer

### 2. **Go Through Installer**

* Language: English
* Keyboard: US
* Network: Wi-Fi (wlp3s0 selected and connected)
* Disk setup: Use entire 1TB NVMe SSD (erase Windows)
* Username: `vishnu`   (// This is what i named, you can name anything else)
* Server Name (hostname): `garuda`  (// This is what i named, you can name anything else)
* Install OpenSSH: ✅ Yes
* Featured Snaps: ❌ Skipped all

### 3. **Finish Installation & Reboot**

* Remove USB after install
* Log in with created credentials

---

## 🔄 Post-Install Configuration

### 1. **Update System and Install Tools**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ufw htop curl git -y
```

### 2. **Configure UFW Firewall**

```bash
sudo ufw allow OpenSSH
sudo ufw enable
```

### 3. **Check IP Address**

```bash
ip a
# Wi-Fi IP: 192.168.1.165 (used for remote SSH) (// This is what i have got, yours can be something else but similar)
```

### 4. **SSH from Mac**

```bash
ssh vishnu@192.168.1.165  (// This is what i should bedoing, yours can be something else but similar, use the same IP from above step)
```

✅ SSH access verified from Mac.

---

## 📍 Next Step (Planned)

* Host a static website using Nginx
* Document setup of Docker, Portainer, Nextcloud
* Add domain and HTTPS using DuckDNS + Let's Encrypt

---

## 📂 Notes

* Windows license preserved (OEM key embedded in firmware)
* Ubuntu Pro offer skipped (can be attached later with `sudo pro attach`)
* All configurations done headlessly via CLI

---

## 📌 Repo Link (To be added)

* GitHub: [vishnutheja1998/home-server-setup](https://github.com/vishnutheja1998/Home-Server-Setup-on-GMKtec-Mini-PC-Ubuntu-24.04-LTS-/)


