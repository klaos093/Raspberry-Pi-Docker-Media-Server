---

# ⚙️ Raspberry Pi Media Server – Setup Guide

This guide walks you through setting up your self-hosted, containerized media server on a **Raspberry Pi 4 (4GB)** using **Docker** and **Gluetun with NordVPN** for secure torrenting and streaming via **Jellyfin**.

---

## 📋 Table of Contents

* [Prerequisites](#prerequisites)
* [1. Flash Raspberry Pi OS Lite](#1-flash-raspberry-pi-os-lite)
* [2. Initial System Setup](#2-initial-system-setup)
* [3. Install Docker & Docker Compose](#3-install-docker--docker-compose)
* [4. Set Up Portainer (Docker Management UI)](#4-set-up-portainer-docker-management-ui)
* [5. Configure Gluetun VPN Container (NordVPN)](#5-configure-gluetun-vpn-container-nordvpn)
* [6. Deploy Media Services](#6-deploy-media-services)
* [7. Connect & Configure Services](#7-connect--configure-services)
* [8. Tips & Common Pitfalls](#8-tips--common-pitfalls)
* [🔗 Related Resources](#-related-resources)

---

## 🧰 Prerequisites

### 🖥 Hardware

* Raspberry Pi 4 (4GB recommended)
* SD Card (16GB or more)
* External SSD or HDD (preferably SSD for speed and durability)
* Official Raspberry Pi power supply (recommended)
* Passive or active cooling case for heat management
* Ethernet cable or stable Wi-Fi connection

### 💾 Software

* Raspberry Pi OS Lite (64-bit)
* Docker & Docker Compose
* SSH access enabled on Raspberry Pi

---

## 1. Flash Raspberry Pi OS Lite

1. Download [Raspberry Pi Imager](https://www.raspberrypi.com/software/).
2. Select **Raspberry Pi OS Lite (64-bit)**.
3. Open “Advanced Options” (gear icon), enable SSH, and configure your hostname and user credentials for ease of use.
4. Flash the OS to your SD card.
5. Insert the SD card into your Pi and power it on.

---

## 2. Initial System Setup

SSH into your Raspberry Pi:

```bash
ssh pi@<your_pi_ip_address>
```

Update and upgrade the system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

### Set a Static IP Address (Recommended)

* Either reserve an IP in your router’s DHCP settings (preferred)
* Or edit `/etc/dhcpcd.conf` directly to assign a static IP

### Mount Your External SSD/HDD

Create mount point and mount the drive:

```bash
sudo mkdir -p /media/externalhdd
sudo mount /dev/sda1 /media/externalhdd
```

> To auto-mount the drive at boot, add an entry to `/etc/fstab`:

```bash
/dev/sda1  /media/externalhdd  auto  defaults,nofail  0  2
```

---

## 3. Install Docker & Docker Compose

Install Docker with the official convenience script:

```bash
curl -sSL https://get.docker.com | sh
```

Install Docker Compose:

```bash
sudo apt install docker-compose -y
```

Add your user to the Docker group for permission management:

```bash
sudo usermod -aG docker $USER
```

Log out and back in, or reboot:

```bash
sudo reboot
```

---

## 4. Set Up Portainer (Docker Management UI)

Create a Docker volume for Portainer data persistence:

```bash
docker volume create portainer_data
```

Run Portainer container:

```bash
docker run -d \
  -p 9000:9000 -p 8000:8000 \
  --name=portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce
```

Access Portainer UI at:

```
http://<your_pi_ip_address>:9000
```

Set up your admin account on first access.

---

## 5. Configure Gluetun VPN Container (NordVPN)

Create a `.env` file in your project directory (copy from `.env.example` if available):

```env
VPN_SERVICE_PROVIDER=nordvpn
VPN_TYPE=openvpn
OPENVPN_USER=your_nordvpn_username
OPENVPN_PASSWORD=your_nordvpn_password
TZ=Europe/Oslo
SERVER_REGIONS=Norway
OPENVPN_PROTOCOL=udp
```

Start the Gluetun container via Docker Compose:

```bash
docker-compose up -d gluetun
```

Verify VPN is active by checking your IP inside the Gluetun container:

```bash
docker run --rm --network container:gluetun curlimages/curl ifconfig.me
```

---

## 6. Deploy Media Services

Add the following services to your [`docker-compose.yml`](../docker-compose/docker-compose.yml)

* qBittorrent (networked through Gluetun)
* Radarr
* Sonarr
* Prowlarr
* Jellyfin

Ensure all download clients are routed through the VPN container for privacy.

Start all containers:

```bash
docker-compose up -d
```

---

## 7. Connect & Configure Services

* Access **qBittorrent** UI (default port `8080`) and change default password.
* Link **Radarr** and **Sonarr** to qBittorrent and **Prowlarr** for automatic media searching and downloading.
* Set download and media library paths to the mounted external drive (e.g., `/media/externalhdd/`).
* Configure **Jellyfin** to scan and serve your media library.

---

## 8. Tips & Common Pitfalls

* Always verify VPN connection is active before starting downloads.
* Monitor your disk space regularly to prevent Jellyfin crashes.
* Use `docker logs <container_name>` to troubleshoot containers.
* If configuration corruption occurs, stop the container, clear the config folder, and restart.
* For password issues with qBittorrent, make sure no duplicate passwords exist in config files.

Refer to our [Troubleshooting Guide](../docs/troubleshooting.md) for common fixes and diagnostic tips.

---

## 🔗 Related Resources

* **Docker Compose File**: [docker-compose.yml](../docker-compose/docker-compose.yml)
* **Troubleshooting**: [Troubleshooting Guide](../docs/troubleshooting.md)
* **Official Gluetun Docker Hub**: [https://hub.docker.com/r/qmcgaw/gluetun](https://hub.docker.com/r/qmcgaw/gluetun)
* **Jellyfin Setup Guide**: [https://jellyfin.org/docs/general/administration/installing/](https://jellyfin.org/docs/general/administration/installing/)
* **Portainer Docs**: [https://docs.portainer.io/](https://docs.portainer.io/)
* **Sonarr, Radarr, Prowlarr Guides**: [https://linuxserver.io/](https://linuxserver.io/)

---
