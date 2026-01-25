# quickarr

A script for **Ubuntu Server 24.04.x (Noble)** that installs:

**This script was generated with ChatGPT and tested/reviewed by a Human.**

- Docker Engine + Docker Compose plugin
- Samba + an SMB share (interactive setup)
- A standard media folder layout:
  - `media/Movies`
  - `media/Music`
  - `media/Shows`
- Radarr/Sonarr/Prowlarr (via docker)
- Jellyfin (via Jellyfin’s official Deb/Ubuntu installer script)
- Jellyseerr (via Docker Compose)

## What This Does

### Installs
- **Docker Engine** (official Docker apt repository)
- **Samba** (SMB file sharing)
- **Radarr/Sonarr/Prowlarr**
- **Jellyfin** media server
- **Jellyseerr** request management UI (Docker Compose)

### Configures
- An SMB share you name + a path you choose
- A `media/` directory inside that share with standard subfolders
- Group permissions via a `media` group (so services/users can work together without permission wars)

---

## Requirements

- Ubuntu Server **24.04** or **24.04.x** (including **24.04.3**)
- Root privileges (`sudo`)
- A network where SMB makes sense (LAN / trusted subnet recommended)

---

**This script was generated with ChatGPT and tested/reviewed by a Human.**

## Quick Start

### 1) Clone the repo

    git clone https://github.com/SixtyAteWhiskey/quickarr.git
    cd quickarr

### 2) Make the script executable

    chmod +x quickarr.sh

### 3) Run it

    sudo ./quickarr.sh

---

## Interactive Prompts (SMB Setup)

The script will ask you for:

- **Share name** (example: `media-share`)
- **Share path** (example: `/srv/samba/media-share`)
- **Linux user** that will own/share files (created if missing)
- Whether to allow **guest access** (`yes` / `no`)

If you choose **non-guest**, you’ll be prompted to set a Samba password for the selected user.

---

## Accessing the SMB Share

After the script completes, you should be able to access the share:

### Windows

    \\SERVER_IP\SHARE_NAME

### Linux (mount example)

    sudo mkdir -p /mnt/SHARE_NAME
    sudo mount -t cifs //SERVER_IP/SHARE_NAME /mnt/SHARE_NAME -o username=SMB_USER

---

## Jellyfin

- Web UI: `http://SERVER_IP:8096`

Check status:

    systemctl status jellyfin --no-pager

---

## Jellyseerr

- Web UI: `http://SERVER_IP:5055`

Default locations:
- Compose file: `/opt/jellyseerr/compose.yaml`
- Config volume: `/opt/jellyseerr/config`

Common commands:

    cd /opt/jellyseerr
    sudo docker compose ps
    sudo docker compose logs -f
    sudo docker compose restart

---

## Security Notes

- **Guest SMB access** means anyone on the network can access it. Use only on trusted networks.
- If you use `ufw`, be aware Docker can publish ports in ways that don’t behave how you’d expect with `ufw` defaults. Consider explicitly managing firewall rules.

---

## Torrenting Reminder (Not Legal Advice, Just Practical)

If you plan to sail the seas:
- Consider installing **qbittorrent-nox**
- Consider using a reputable **VPN provider**
- Consider isolating your download client (VPN container or network namespace) so it never leaks traffic

---

## Tested On

- Ubuntu Server 24.04.x (Noble)

If you hit an issue, open an issue and include:
- Output of: `lsb_release -a`
- Output of: `docker --version`
- Output of: `systemctl status jellyfin --no-pager`
- Any relevant logs

---

## Disclaimer

**This script was generated with ChatGPT and tested/reviewed by a Human.**


This script makes system-level changes (installs packages, edits `/etc/samba/smb.conf`, creates users/groups, starts services).  
Read it before running it, you’re the admin here.

---
