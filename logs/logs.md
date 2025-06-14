---

# 📋 Logs Guide

This document explains how to access and view logs for your media server services using the command line and Portainer web UI.

---

## 🔍 Viewing Logs via Command Line

### List all running containers:

```bash
docker ps
```

### View logs for a specific container:

```bash
docker logs <container_name>
```

Example:

```bash
docker logs qbittorrent
```

### View logs in real-time (follow logs):

```bash
docker logs -f <container_name>
```

Press `Ctrl + C` to exit live view.

### Access logs stored on disk

Some services save logs inside mounted volumes, e.g.:

```bash
ls -lh /media/klaos/externalhdd/media-server/logs/
```

You can read these log files using:

```bash
cat <filename>
less <filename>
tail -n 100 <filename>
```

---

## 🌐 Viewing Logs in Portainer

### 1. Open Portainer Web UI

Go to:

```
http://<your_pi_ip>:9000
```

Login with your credentials.

### 2. Find Your Container

* Click **Containers** on the sidebar.
* Select the container (e.g., `sonarr`, `gluetun`, `jellyfin`, etc.).

### 3. View Logs

* Click on the container name.
* Navigate to the **Logs** tab to view recent logs.
* Use filtering and live update options if needed.

---

That’s it! This guide covers all the ways to check your media server logs easily.

---
