# 🍿 Raspberry Pi Media Server Project

A self-hosted, VPN-secure, containerized media server powered by **Raspberry Pi 4 (4GB)**. This project demonstrates how to use Docker, Gluetun (with NordVPN), and open-source tools to create a fully functional home media ecosystem for downloading and streaming content via **Jellyfin**.

---

## ⚠️ Disclaimer

This project is intended for educational and personal use only. It does **not** encourage or support any piracy or illegal activity. Please ensure you comply with all applicable laws and respect content creators' rights.

---


## 📖 Project Overview

This project transforms a Raspberry Pi 4 into a compact and energy-efficient media server capable of downloading, organizing, and streaming movies and TV shows securely over the internet.

Here’s how it works:

- All services are containerized using **Docker**, providing isolation, scalability, and easier maintenance.
- **Gluetun** acts as a VPN gateway container, securely routing all internet traffic through **NordVPN**.
- **Qbittorrent** handles all torrent downloading tasks, operating only through the VPN container to ensure privacy.
- **Radarr** and **Sonarr** manage and automate the downloading of movies and TV shows, respectively.
- **Prowlarr** functions as an indexer manager, providing integration and search capabilities for both Sonarr and Radarr.
- **Jellyfin** serves as the streaming platform, organizing and serving media to client devices on the local network.
- **Portainer** provides a user-friendly web interface to manage and monitor Docker containers and services.
- All media is stored on an external SSD connected to the Pi and mounted at boot.

This setup ensures that media is automatically fetched, securely downloaded, neatly organized, and easily accessible — all from a low-cost, low-power device.

---

## ⚙️ Technologies Used

- **Hardware**: Raspberry Pi 4 (4GB), SSD/HDD, cooling case
- **OS**: Raspberry Pi OS Lite (64-bit)
- **Containerization**: Docker & Docker Compose
- **Media Downloader**:
  - 🧲 Qbittorrent (via Gluetun VPN tunnel)
  - 📡 Sonarr (TV Shows)
  - 🎬 Radarr (Movies)
  - 🔍 Prowlarr (Indexer Manager)
- **Media Streaming**: 🍿 Jellyfin
- **Security**: 🛡️ Gluetun (NordVPN integration)
- **UI Management**: ⚙️ Portainer (Docker container management)

---

## 📂 Project Structure

| Path                          | Description                                               |
|------------------------------|-----------------------------------------------------------|
| 📂 `docker-compose/`         | YAML files for all containers (Gluetun, Jellyfin, etc.)   |
| 📂 `configs/`                | Configuration files for each service                      |
| 📂 `logs/`                   | Sample logs and troubleshooting examples                  |
| 📂 `docs/`                   | Documentation and setup notes                             |
| 📜 `README.md`               | Project overview and setup instructions                   |
| 📜 `LICENSE`                 | Open-source license (MIT recommended)                     |

---

## 📢 Why This Project is Great for Your Portfolio?

- **Demonstrates Practical Skills:** Showcases your ability to work with Raspberry Pi, Docker, and VPNs in a real-world scenario.  
- **Full-Stack Project:** Covers setup from hardware to software, including container orchestration and networking.  
- **Security Focused:** Highlights your knowledge of VPN integration and secure data handling.  
- **Automation & Maintenance:** Shows experience with container management and troubleshooting.  
- **Open-Source Friendly:** Encourages collaboration and continuous improvement.  
- **Scalable & Adaptable:** Easily extendable with additional services or features to fit future needs.  

---

## 🔗 Connect & Share

If you found this project useful, share it on GitHub, LinkedIn, or Twitter! 🚀

Feel free to contribute or reach out for improvements.

---
