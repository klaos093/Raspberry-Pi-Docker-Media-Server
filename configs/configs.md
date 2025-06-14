---

## 🛠️ Configuration Files Overview

All configuration files and databases for the services in this media server are automatically created and managed by the Docker containers upon startup, based on the `docker-compose.yml` settings. Typically, you won't need to manually modify these files unless troubleshooting or customizing specific service behaviors.

Below is a breakdown of the key files and folders you’ll find in each service’s config directory, along with a short description of their purpose.

---

### Jellyfin

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/jellyfin/config $ ls
branding.xml  encoding.xml  logging.default.json  migrations.xml  network.xml  system.xml
```

* **branding.xml**: Contains UI branding like logos and themes.
* **encoding.xml**: Settings for media encoding and transcoding.
* **logging.default.json**: Configuration for Jellyfin’s logging system.
* **migrations.xml**: Tracks database and config migrations between versions.
* **network.xml**: Network configuration, including ports and access control.
* **system.xml**: Core system settings such as user preferences and cache.

---

### Sonarr

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/sonarr $ ls
asp  Backups  config.xml  logs  logs.db  logs.db-shm  logs.db-wal  sonarr.db  sonarr.db-shm  sonarr.db-wal  sonarr.pid  Sentry
```

* **config.xml**: Main Sonarr settings (indexers, download clients, profiles).
* **asp/**: Application-specific files.
* **Backups/**: Automatic backups of your configuration.
* **logs/**: Log files for Sonarr’s operation and debugging.
* **logs.db**, **logs.db-shm**, **logs.db-wal**: SQLite database files storing logs.
* **sonarr.db**, **sonarr.db-shm**, **sonarr.db-wal**: SQLite database for Sonarr’s metadata.
* **sonarr.pid**: Process ID file for running instance.
* **Sentry/**: Crash reports and error tracking data.

---

### Radarr

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/radarr $ ls
asp  Backups  config.xml  logs  logs.db  radarr.db  radarr.db-shm  radarr.db-wal  radarr.pid  Sentry
```

* **config.xml**: Radarr’s primary settings file (movie sources, download clients).
* **asp/**: Application-specific data.
* **Backups/**: Configuration backup snapshots.
* **logs/**: Runtime and error logs.
* **logs.db**: Logs database file.
* **radarr.db**, **radarr.db-shm**, **radarr.db-wal**: Core SQLite database files.
* **radarr.pid**: Running process ID.
* **Sentry/**: Error and crash reporting data.

---

### Prowlarr

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/prowlarr $ ls
asp  Backups  config.xml  Definitions  logs  logs.db  prowlarr.db  prowlarr.db-shm  prowlarr.db-wal  prowlarr.pid  Sentry
```

* **config.xml**: Prowlarr’s main configuration (indexers, API keys).
* **asp/**: Application-specific files.
* **Backups/**: Saved backups of the config.
* **Definitions/**: Indexer definitions and categories.
* **logs/**: Service logs.
* **logs.db**: Logs stored in SQLite.
* **prowlarr.db**, **prowlarr.db-shm**, **prowlarr.db-wal**: Main SQLite database files.
* **prowlarr.pid**: Process ID file.
* **Sentry/**: Crash and error reporting information.

---

### Portainer

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/portainer $ ls
bin  certs  chisel  compose  docker_config  portainer.db  portainer.key  portainer.pub  tls
```

* **portainer.db**: SQLite database storing Portainer user data and settings.
* **certs/**: TLS certificates for secure access.
* **chisel/**, **compose/**, **docker\_config/**: Support files for Docker management features.
* **portainer.key**, **portainer.pub**: Keys used for encryption.
* **tls/**: Additional TLS configuration files.
* **bin/**: Executable and helper binaries.

---

### Gluetun

```bash
klaos@raspberrypi:/media/klaos/externalhdd/media-server/config/gluetun $ ls
servers.json
```

* **servers.json**: List and status of VPN servers used by Gluetun for routing traffic securely.

---

## Summary

These directories and files are essential for the correct functioning of your media server stack. Since they are auto-generated and managed by Docker containers at runtime, manual modifications should be made cautiously and primarily during troubleshooting or advanced customization.

---
