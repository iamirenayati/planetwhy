# 🪐 PlanetWhy AI (پلنت وای)

> **Official Interactive 3D Portal & Landing Page for PlanetWhy AI Academy — Children & Teens AI & Creativity Academy (Qaemshahr & Mazandaran).**

[![Docker](https://img.shields.io/badge/Docker-Production%20Ready-blue?logo=docker)](https://www.docker.com/)
[![Nginx](https://img.shields.io/badge/Nginx-Alpine%201.27-green?logo=nginx)](https://nginx.org/)
[![Three.js](https://img.shields.io/badge/Three.js-r128%20WebGL-black?logo=three.js)](https://threejs.org/)
[![Typography](https://img.shields.io/badge/Typography-Yekan%20Bakh%20Local-yellow)]()
[![License](https://img.shields.io/badge/License-Proprietary-red)]()

---

## 📖 Overview

**PlanetWhy AI (پلنت وای)** is a cutting-edge, high-performance web portal built for an innovative Artificial Intelligence and Critical Thinking academy for children and teens. It bridges futuristic AI education with playful Pop-Art Neo-Brutalist aesthetics, transforming passive screen time into creative superpowers: authoring printed storybooks, composing music with AI, and building 2D video games.

The platform is engineered with **vanilla web standards** for instant 50ms page load speeds, zero GPU lag, and zero runtime dependencies, paired with an interactive **Three.js WebGL procedural 3D world**.

---

## ✨ Key Features & Architectural Highlights

### 1. 🪐 Interactive 3D Cartoon Planet (Three.js WebGL)
- **Stylized Pixar/Disney World**: Dynamic celestial ocean sphere with sculpted emerald and golden continents, coral peaks, and polar ice caps.
- **Dual Planetary Rings**: Inner golden ring + translucent cyan holographic ring with orbiting sparkling stardust gems.
- **Fluffy 3D Clouds & Space Shuttle**: Multi-layer cartoon cloud clusters and an orbiting AI explorer rocket with a dynamic pulsing jet engine flame.
- **Ergonomic Touch & Drag**: Custom physics loop featuring inertia damping (`damping: 0.94`), squash-and-stretch spring reaction on tap, and non-blocking vertical scroll (`touch-action: pan-y`).

### 2. 🎨 Pop-Art Neo-Brutalist Design System
- **Karl Gonsalves Visual Doctrine**: Vibrant signature palette (Signal Yellow `#ffe600`, Globe Azure `#007fff`, Roof Coral `#ef3b2c`, Charcoal Ink `#333333`).
- **Ben-Day Halftone Dots**: Subtle comic dot background patterns adding tactile depth and vintage comic book energy.
- **Kinetic Micro-Interactions**:
  - `3D Parallax Tilt & Specular Shine`: Cards smoothly tilt toward cursor in 3D perspective with dynamic glare reflection.
  - `Shimmer Light Sweep`: Ambient animated shine sweep across primary badges and CTAs.
  - `Floating Kinetic Feature Chips`: Playful orbiting feature tags with spring physics.
  - `Kinetic Number Counters`: Smooth easing tickers counting up stats on scroll.

### 3. 📱 100% Mobile Responsive & Ergonomic
- Audited and verified across viewports from **320px to 1440px** (iPhone SE, iPhone 14/15/16 Pro, Samsung Galaxy, iPad, Desktop).
- Zero horizontal overflow (`hasOverflow: false`).
- Swipeable single-row horizontal tab rail with scroll-snap for the scientific Data Lab.
- 44x44px standard touch targets and iOS Safari auto-zoom prevention (`16px` form inputs).

### 4. 📬 Zero-Drop Lead Notification System
- **Instant Email Dispatch**: Submits lead data directly via FormSubmit AJAX API to `iamirhosseinenayati@gmail.com` with formatted HTML tables.
- **Local Browser Resilience**: All submissions are backed up in `localStorage` (`planetwhy_leads`) so zero inquiries are ever lost.
- **Google Forms / Webhook Ready**: Configured for plug-and-play synchronization with Google Forms or custom automation webhooks.

### 5. ⚡ Self-Hosted Performance
- **100% Local Typography**: Bundles self-hosted Yekan Bakh fonts (`yekanbakhreg.ttf`, `yekanbakhsemibold.ttf`, `yekanbakhbold.ttf`, `yekanbakhbold2.ttf`).
- **Fast First Contentful Paint (FCP)**: No blocking external font servers or heavyweight frameworks.

---

## 📁 Project Directory Structure

```text
planetwhy/
├── index.html                  # Core production landing page & WebGL scene
├── Dockerfile                  # Lightweight Alpine Nginx container build
├── docker-compose.production.yml # Multi-site production compose specification
├── nginx-app.conf              # Production Nginx config (Gzip, caching, security headers)
├── .dockerignore               # Build exclusion rules
├── .gitignore                  # Git repository exclusion rules
├── README.md                   # Project documentation
├── REQUIREMENTS.md             # System & environment prerequisites
├── package.json                # Project descriptor & tooling dependencies
├── fonts/                      # Self-hosted Yekan Bakh font families
│   ├── yekanbakhreg.ttf
│   ├── yekanbakhsemibold.ttf
│   ├── yekanbakhbold.ttf
│   └── yekanbakhbold2.ttf
├── images/                     # Production visual assets & exhibits
│   ├── hero.jpg
│   ├── storybook.jpg
│   ├── game.jpg
│   ├── music.jpg
│   └── ...
└── js/                         # Vendor JavaScript libraries
    └── three.min.js            # Three.js r128 UMD distribution
```

---

## 🚀 Quick Start (Local Development)

### Option A: Python Built-in Server
No installation required; run directly from the project directory:

```bash
# Python 3
python -m http.server 8090
```
Open [http://localhost:8090](http://localhost:8090) in your browser.

### Option B: Node.js / npx
```bash
npx serve -l 8090
```

### Option C: Docker Local Preview
```bash
docker build -t planetwhy-local .
docker run -p 8090:80 planetwhy-local
```

---

## 🌐 Production Deployment (VPS Multisite Architecture)

Designed to run smoothly on a shared Ubuntu VPS alongside other projects (such as `villa-one.ir`) with zero interference.

### 1. DNS Configuration
Point your domain's `A` records to your VPS IP address (`188.212.99.249`):
- `@` -> `188.212.99.249`
- `www` -> `188.212.99.249`

### 2. Deploy Container
Transfer files to `/srv/planetwhy` and launch the isolated container on loopback port `127.0.0.1:8105`:

```bash
cd /srv/planetwhy
docker compose -p planetwhy -f docker-compose.production.yml up -d --build
```

### 3. Host Nginx Reverse Proxy
Add `/etc/nginx/sites-available/planetwhy.conf`:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name planetwhy.ir www.planetwhy.ir;

    client_max_body_size 20M;

    location / {
        proxy_pass http://127.0.0.1:8105;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable configuration and reload host Nginx:
```bash
sudo ln -s /etc/nginx/sites-available/planetwhy.conf /etc/nginx/sites-enabled/planetwhy.conf
sudo nginx -t
sudo systemctl reload nginx
```

### 4. Enable Let's Encrypt SSL
```bash
sudo certbot --nginx -d planetwhy.ir -d www.planetwhy.ir
```

---

## 👨‍💻 Author & Maintainer

- **Founder & AI Instructor**: Amirhossein Enayati (امیرحسین عنایتی)
- **GitHub**: [@iamirenayati](https://github.com/iamirenayati)
- **Email**: iamirhosseinenayati@gmail.com
- **Phone**: `09118702633`
- **Academy Location**: Qaemshahr & Sari, Mazandaran, Iran
