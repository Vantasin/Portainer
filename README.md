# 📦 Portainer Docker Compose Stack

[![MIT License](https://img.shields.io/github/license/Vantasin/Portainer?style=flat-square)](LICENSE)
[![Woodpecker CI](https://img.shields.io/badge/Woodpecker%20CI-self--hosted-green?logo=drone&style=flat-square)](https://woodpecker-ci.org/)
[![Docker Pulls: portainer/portainer-ce](https://img.shields.io/docker/pulls/portainer/portainer-ce?style=flat-square&logo=docker)](https://hub.docker.com/r/portainer/portainer-ce)

This repository provides a self-contained Docker Compose stack to run [Portainer CE](https://www.portainer.io/), a lightweight UI for managing Docker environments.

---

## 📁 Directory Structure

```bash
tank/
├── docker/
│   ├── compose/
│   │   └── portainer/              # Git repo lives here
│   │       ├── docker-compose.yml  # Main Docker Compose config
│   │       ├── .env                # Runtime environment variables and secrets (gitignored!)
│   │       ├── env.example         # Example .env file for reference
│   │       └── README.md           # This file
│   └── data/
│       └── portainer/              # Volume mounts and persistent data
```

---

## 🧰 Prerequisites

* Docker Engine
* Docker Compose V2
* Git
* (Optional) ZFS on Linux for dataset management

> ⚠️ **Note:** These instructions assume your ZFS pool is named `tank`. If your pool has a different name (e.g., `rpool`, `zdata`, etc.), replace `tank` in all paths and commands with your actual pool name.

---

## ⚙️ Setup Instructions

1. **Create the stack directory and clone the repository**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/compose/Portainer
   cd /tank/docker/compose/Portainer
   sudo git clone https://github.com/Vantasin/Portainer.git .
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/compose/Portainer
   cd ~/docker/compose/Portainer
   git clone https://github.com/Vantasin/Portainer.git .
   ```

2. **Create the runtime data directory**

   If using ZFS:
   ```bash
   sudo zfs create -p tank/docker/data/Portainer
   ```

   If using standard directories:
   ```bash
   mkdir -p ~/docker/data/Portainer
   ```

3. **Configure environment variables**

   Copy and modify the `.env` file:

   ```bash
   sudo cp env.example .env
   sudo nano .env
   sudo chmod 600 .env
   ```

   > **Note:** You only need to change the `.env` file if you are not using the default storage path.

4. **Start Portainer**

   ```bash
   docker compose up -d
   ```

---

## 🌐 Access Portainer

Once running, open your browser to:

```
https://portainer.example.com
```

> **Note:** You must use [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy to access `Portainer`.

> **Note:** Use `portainer` for the **Forward Hostname / IP** and `9000` for the **Forward Port**

<p align="center">
  <img
    src="images/proxy-host.png"
    alt="New Proxy Host UI"
    style="width:50%; height:auto;"
  />
</p>

---

## 🙏 Acknowledgements

- [Portainer](https://www.portainer.io/)
- [Docker](https://www.docker.com/)
- [Woodpecker CI](https://woodpecker-ci.org/)
- [ZFS on Linux](https://openzfs.org/)
