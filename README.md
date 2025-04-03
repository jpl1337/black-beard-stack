Sure thing, Captain! Here's a draft for a **professional and pirate-themed README** tailored for your Black Beard Stack:

---

# 🏴‍☠️ Black Beard Stack

> A modular, VPN-protected, dockerized media automation stack for anyone interested in sailing the high seas!

---

## ⚓ Overview

Black Beard Stack is a full-featured, privacy-respecting, self-hosted media stack for:
- Automated downloading of all media
- Managing, organizing, and enriching your media library
- Secure VPN-enforced torrenting and usenet traffic
- Streaming with Plex
- Reverse proxy via Nginx
- All services routed through a secure VPN tunnel except Plex and Nginx (optional)

This stack aims to automate hauling in treasures while protecting your ship from the eyes of the British Navy... or your ISP.

---

## 🧩 Included Services

| Service | Purpose |
|---------|---------|
| **Plex** | Media server for streaming TV, movies, music |
| **qBittorrent** | Torrent client (via VPN) |
| **SABnzbd** | Usenet downloader (via VPN) |
| **Radarr** | Automated movie management |
| **Sonarr** | Automated TV show management |
| **Readarr** | Book downloader & manager |
| **Lidarr** | Music downloader & manager |
| **Bazarr** | Subtitle downloader |
| **Overseerr** | Media request management |
| **Prowlarr** | Indexer management for Sonarr/Radarr/etc |
| **Tdarr** | Automated transcoding |
| **Tautulli** | Plex analytics |
| **Flaresolverr** | Cloudflare bypass tool |
| **Gluetun** | VPN tunnel and firewall (for torrent and usenet traffic) |
| **Nginx** | Reverse proxy for all web UIs |
| **Portainer** (optional) | Docker stack GUI management |

---

## ☠️ Architecture

```
[ Clients ]
     │
[ Tunnel / VPN / LAN ]
     │
[ NGINX Reverse Proxy ]
     │
 ┌───────────────┬──────────────┬──────────────┐
 │               │              │              │
[ Plex ]   [ Overseerr ]   [ Other Services ]
                      │
           [ All downloaders + radarr, sonarr, lidarr, etc ] 
                      │
               [ VPN Protected via Gluetun ]
```

---

## ⚙️ Requirements

- Docker
- Docker Compose
- `.env` file with all necessary variables (see below)
- Port forwarding (for Plex only, if needed)
- Optional: Cloudflare Tunnel or Tailscale for secure remote access

---

## 💾 Installation

1. Clone the repo:
   ```bash
   git clone https://github.com/jpl1337/black-beard-stack.git
   cd black-beard-stack
   ```

2. Create your `.env` file:
   ```bash
   cp .env.example .env
   ```

3. Review and edit your `.env` variables:
   - VPN credentials
   - Media paths
   - PUID, PGID
   - Plex claim token (optional)
   - Base URL paths for Sonarr, Radarr, etc.

4. Start the stack:
   ```bash
   docker-compose up -d
   ```

---

## ⚠️ First Time Setup Notes

- Generate a Plex claim token from here:[https://www.plex.tv/claim](https://www.plex.tv/claim "https://www.plex.tv/claim")
- Reference the [GlueTun Wiki](https://github.com/qdm12/gluetun-wiki) and setup VPN as you like!
- Most of these services require you to set a **URL Base** like `/sonarr`, `/radarr`, etc. before reverse proxying will work.
- Temporarily access them via `localhost:<service-port>` to configure.
- Once configured, access them via Nginx:
   ```
   http://yourdomain/sonarr
   http://yourdomain/radarr
   etc...
   ```

---

## 🔒 Security Notes

- All services except Nginx and Plex are routed **exclusively** through Gluetun.
- Nginx will only see services through the VPN tunnel, reducing leak risks.
- Disabled IPv6 inside Gluetun to prevent IPv6 leaks:
   ```yaml
   environment:
     - DISABLE_IPV6=true
   ```

---

## 🐳 Docker Compose Tips

- To rebuild a single service:
   ```bash
   docker-compose up -d --force-recreate --no-deps nginx
   ```
- To check VPN status:
   ```bash
   docker exec -it vpn curl ifconfig.me
   ```
- To view container logs:
   ```bash
   docker-compose logs -f <service>
   ```

---

## 💡 Recommendations

- Consider using Cloudflare Tunnel or Tailscale for secure remote access without open ports

---

## ☠️ Credits

- Inspired by Swizzin and other of the community's media stacks
- Crafted with 🏴‍☠️ pirate aesthetics and modern docker practices
