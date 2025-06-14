---

# 🛠️ Raspberry Pi Media Server – Troubleshooting Guide

This guide helps you quickly diagnose and fix common issues encountered during the Raspberry Pi Media Server setup.

---

## 1. External SSD Not Mounted

### Problem

The external SSD was not mounted, causing services to fail because download and media folders were missing.

### Symptoms

* Docker containers cannot access `/downloads`, `/movies`, or `/tv` directories.
* Jellyfin and other services show missing media.

### Solution

* Ensure the drive is properly connected and recognized:

  ```bash
  lsblk
  ```

* Manually mount the drive:

  ```bash
  sudo mkdir -p /media/externalhdd
  sudo mount /dev/sda1 /media/externalhdd
  ```

* Configure `/etc/fstab` for automatic mounting on boot:

  ```bash
  sudo nano /etc/fstab
  ```

  Add:

  ```
  /dev/sda1  /media/externalhdd  auto  defaults,nofail  0  2
  ```

* Reboot and verify mount:

  ```bash
  df -h
  ```

---

## 2. Jellyfin Container Misconfiguration

### Problem

Jellyfin was running but the media library was empty or not scanning properly.

### Symptoms

* Jellyfin UI shows no media.
* Media folders are empty inside the container.

### Solution

* Verify volumes in your `docker-compose.yml` for Jellyfin:

  ```yaml
  volumes:
    - ./config/jellyfin:/config
    - ./cache/jellyfin:/cache
    - /media/externalhdd/tv:/data/tvshows
    - /media/externalhdd/movies:/data/movies
  ```

* Ensure the paths inside the container match Jellyfin’s library paths (`/data/tvshows` and `/data/movies`).

* Rescan libraries from Jellyfin dashboard after correcting paths.

* Check Jellyfin logs for errors:

  ```bash
  docker logs jellyfin
  ```

---

## 3. VPN (Gluetun) Configuration Issues

### Problem

Gluetun VPN container was not establishing a connection or services were leaking traffic outside VPN.

### Symptoms

* IP check inside container shows local IP instead of VPN IP.
* qBittorrent or other containers are not routed via VPN.

### Solution

* Check Gluetun environment variables for NordVPN credentials and settings in `.env` or `docker-compose.yml`.

* Verify VPN connection status via Gluetun Web UI:

  ```
  http://<your_pi_ip>:8000
  ```

* Test IP routing inside the VPN container:

  ```bash
  docker run --rm --network container:gluetun curlimages/curl ifconfig.me
  ```

* Restart the Gluetun container after config changes:

  ```bash
  docker-compose restart gluetun
  ```

---

## 4. qBittorrent Login Issues

### Problem

Unable to login to qBittorrent web UI after initial setup or password changes.

### Symptoms

* Login fails repeatedly despite correct credentials.
* Conflicting passwords due to multiple entries in config files.

### Solution

* Inspect qBittorrent container logs for password-related errors:

  ```bash
  docker logs qbittorrent
  ```

* Default web UI username/password may be:

  ```
  Username: admin
  Password: adminadmin
  ```

* To reset the password, edit or delete the config file at:

  ```
  ./config/qbittorrent/qBittorrent.conf
  ```

* Remove duplicate or conflicting password entries.

* Restart qBittorrent container after changes:

  ```bash
  docker-compose restart qbittorrent
  ```

* Always update your password in only one place to avoid conflicts.

---

## 5. Overnight qBittorrent Access Problems

### Problem

Could not login to qBittorrent in the morning after previously successful access.

### Symptoms

* Suddenly blocked or denied login attempts.
* Configuration file contains multiple password entries causing conflicts.

### Solution

* Verify no additional password lines have been appended accidentally.

* Keep a backup of your clean config before editing.

* Use `docker exec` to enter container and inspect configs:

  ```bash
  docker exec -it qbittorrent bash
  cat /config/qBittorrent.conf
  ```

* Remove any duplicate or conflicting entries.

* Restart container after cleanup.

* Consider setting environment variables for password via Docker Compose to avoid manual config edits.

---

## 6. Hard Drive Full – Data Corruption and Service Failures

### Problem

The external SSD/HDD ran out of space, causing file corruption and making containers or services unstable or unusable.

### Symptoms

* Services (Jellyfin, qBittorrent, Sonarr, etc.) crash or behave erratically.
* Media files become corrupted or inaccessible.
* Docker containers fail to start or throw errors related to missing or corrupt files.

### Solution

* **Monitor disk space regularly:**

  ```bash
  df -h /media/externalhdd
  ```

* Set up alerts or cron jobs to notify you when disk space is low.

* **Clean up unnecessary files**:

  * Delete old downloads, logs, or temporary files.
  * Use Radarr and Sonarr’s built-in options to manage and remove unwanted media.

* **Backup important data regularly** to avoid permanent loss.

* **If corruption occurs:**

  * Stop all containers immediately to prevent further damage:

    ```bash
    docker-compose down
    ```

  * Run a filesystem check on the external drive (replace `/dev/sda1` with your device):

    ```bash
    sudo umount /media/externalhdd
    sudo fsck /dev/sda1
    ```

  * Repair any errors found by `fsck`.

  * After repair, remount the drive:

    ```bash
    sudo mount /dev/sda1 /media/externalhdd
    ```

  * Restore any backups if files are lost or corrupted.

* **Prevent future issues** by:

  * Monitoring disk usage.
  * Configuring Radarr/Sonarr to auto-delete or archive files.
  * Ensuring adequate disk space before bulk downloads.

---

## 🔍 Additional Debugging Tips

* Use `docker ps` to confirm containers are running.

* Inspect logs for any container with issues:

  ```bash
  docker logs <container_name>
  ```

* Restart problematic containers individually to isolate issues.

* For configuration resets, stop the container, remove config folders, and redeploy.

---

If you want, I can help you generate a set of scripts or commands to automate some of these fixes. Would you like that?
