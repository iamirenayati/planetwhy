# 📋 Requirements & System Specifications

This document outlines the system, runtime, network, and hosting requirements for running and deploying **PlanetWhy AI**.

---

## 1. Client & Browser Compatibility

The frontend is built on vanilla web standards without framework hydration overhead.

| Platform / Browser | Minimum Supported Version | Recommended | Notes |
|---|---|---|---|
| **Google Chrome / Chromium** | Version 90+ | Latest | Full WebGL & smooth CSS 3D transforms |
| **Apple Safari (iOS / macOS)** | iOS 14+ / macOS 11+ | Latest | Uses `touch-action: pan-y` & 16px inputs |
| **Mozilla Firefox** | Version 88+ | Latest | Hardware-accelerated WebGL canvas |
| **Microsoft Edge** | Version 90+ | Latest | Chromium-based |
| **Samsung Internet** | Version 15+ | Latest | Full mobile touch gesture support |

### Hardware Requirements (Client)
- **RAM**: Minimum 1 GB available browser memory.
- **GPU**: Any standard GPU or integrated chipset supporting WebGL 1.0 / 2.0.
- **Display Resolution**: Responsive from 320px width (ultra-compact phones) up to 4K displays.

---

## 2. Development Environment (Local)

To develop or preview the website locally:

| Dependency | Minimum Version | Purpose | Required? |
|---|---|---|---|
| **Python 3** | 3.8+ | Instant local HTTP server (`python -m http.server`) | Optional |
| **Node.js / npm** | Node 18+ / npm 9+ | Tooling, linting, or `npx serve` | Optional |
| **Docker Engine** | 20.10+ | Local container testing | Optional |

---

## 3. Production Server Requirements (VPS)

PlanetWhy AI is optimized for extreme efficiency and minimal resource footprint.

### Minimal Hardware Footprint
- **Operating System**: Ubuntu 22.04 LTS or 24.04 LTS (recommended)
- **CPU**: 1 Core (App container uses `<= 0.50` CPU)
- **Memory (RAM)**: 512 MB available (App container capped at `128MB`, idle consumption `< 15MB`)
- **Disk Space**: ~200 MB for static files and Docker image layers.

### Server Software Stack
1. **Docker Engine & Docker Compose V2**:
   - `docker` >= 24.0
   - `docker compose` >= 2.20
2. **Host Nginx**:
   - Version >= 1.18 (Acts as the master reverse-proxy on public ports `80` and `443`)
3. **Certbot**:
   - `certbot` and `python3-certbot-nginx` for automated TLS/SSL certificate issuance and renewal.

---

## 4. Network & Port Allocation

In accordance with the **VPS Multi-Site Architecture**:

| Service | Address / Port | Exposure | Description |
|---|---|---|---|
| **Host Nginx** | `0.0.0.0:80`, `[::]:80` | Public Internet | HTTP traffic & Let's Encrypt validation |
| **Host Nginx** | `0.0.0.0:443`, `[::]:443` | Public Internet | HTTPS secured traffic with TLS 1.2/1.3 |
| **PlanetWhy Web Container** | `127.0.0.1:8105:80` | **Loopback Only** | Private internal port, isolated from internet |
| **SSH** | `0.0.0.0:22` | Public (Protected) | VPS administrative remote access |

---

## 5. Third-Party Service Integrations

| Service | Purpose | Configuration Location | Status |
|---|---|---|---|
| **FormSubmit AJAX API** | Real-time lead email dispatch | `index.html` (`PLANETWHY_CONFIG.emailEndpoint`) | Active (`iamirhosseinenayati@gmail.com`) |
| **Browser LocalStorage** | Zero-drop client backup | `index.html` (`planetwhy_leads`) | Active |
| **Google Forms** (Optional) | Direct spreadsheet sync | `index.html` (`PLANETWHY_CONFIG.googleForm`) | Ready for Form ID |
| **Let's Encrypt CA** | Free 90-day automated SSL | Host Certbot | Configured in deployment guide |
