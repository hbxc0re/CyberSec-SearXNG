# CyberSec-SearXNG
# Hardened SecXNG: Dedicated Cybersecurity & OSINT Search Appliance

A production-ready, self-hosted metasearch appliance optimized for cybersecurity research, vulnerability intelligence, and OSINT reconnaissance. Deployed via Docker Compose on Ubuntu Server, this architecture integrates strict OPSEC hardening, Tor routing, hybrid direct API bypasses, and custom vulnerability engines (`!cve`, `!crt`, `!edb`, `!gtfo`, `!ht`).

---

## 🏗️ Architecture Overview

+-------------------------------------------------------+
| Ubuntu Server 24.04 LTS (Host Firewall: UFW Locked)   |
|                                                       |
|  +-------------------------------------------------+  |
|  | Caddy Reverse Proxy (Ports 80 / 443)            |  |
|  +------------------------+------------------------+  |
|                           | (Internal Bridge)         |
|  +------------------------v------------------------+  |
|  | SearXNG Core Appliance (Port 8080 - Loopback)   |  |
|  +------+-------------------+-------------------+--+  |
|         |                   |                   |     |
|  +------v------+     +------v------+     +------v---+ |
|  | Valkey Cache|     | Alpine Tor  |     | Direct   | |
|  | (In-Memory) |     | SOCKS5 Proxy|     | WAN APIs | |
|  +-------------+     +-------------+     +----------+ |
|                         (Anonymized)       (Bypasses) |
+-------------------------------------------------------+


---

## 🛠️ Tech Stack & Components
* **SearXNG:** Uncapped JSON/CSV API outputs for local AI and automation workflows, with safe search disabled and telemetry autocomplete stripped.
* **Alpine Tor Sidecar:** Lightweight container running a modern, stable Tor SOCKS5h client to anonymize general reconnaissance traffic.
* **Hybrid Engine Routing:** Dedicated bypass rules (`proxy: false`) for heavy vulnerability and OSINT endpoints (`crt.sh` and NIST NVD) to eliminate Tor exit-node timeouts and rate limits.
* **Valkey:** High-performance Redis-compatible in-memory data store for low-latency session and rate-limit handling.
* **Caddy:** Automatic local TLS/HTTP reverse proxy terminating incoming connections securely.

---

## 📂 Repository Structure
```text
├── docker-compose.yml     # Multi-container orchestration manifest
├── Caddyfile              # Reverse proxy routing rules
└── searxng/
    └── settings.yml       # Hardened SearXNG configuration & custom engines

