# 🐳 Portainer Docker Stack with Optional ZFS Integration

This repository provides a self-contained Docker Compose stack to run [Portainer CE](https://www.portainer.io/), a lightweight UI for managing Docker environments.

It supports optional ZFS integration for advanced users who want to mount Portainer data volumes onto a ZFS dataset, while remaining fully usable without ZFS.

---

## 📁 Features

- 🐳 Deploys `portainer/portainer-ce:latest` via Docker Compose
- 🔐 Environment-configurable volumes and ports via `.env`
- 🧪 Includes `preflight.sh` to verify and create ZFS datasets if needed
- ⚙️ Compatible with Ansible templating for infrastructure automation

---

## ⚙️ Requirements

- Docker and Docker Compose installed on your system
- (Optional) ZFS installed if you plan to use ZFS volumes
- (Optional) Ansible to manage environment variables

---

## 🚀 Usage

### 📦 1. Clone the Repository

```bash
git clone https://github.com/Vantasin/Portainer.git
cd Portainer
```

### ⚙️ 2. Set Up the `.env` File

```bash
cp env.example .env
nano .env
```

Edit the `.env` file to match your setup:

```dotenv
PORTAINER_PORT=9000

# If using ZFS (required by preflight.sh):
ZPOOL=tank
PORTAINER_DATASET_PATH=docker/volumes/portainer/data
PORTAINER_DATASET=tank/docker/volumes/portainer/data
PORTAINER_DATA_VOLUME=/tank/docker/volumes/portainer/data

# If NOT using ZFS, set:
# PORTAINER_DATA_VOLUME=./data
```

---

## 🧰 Option A: Run with ZFS

> Ideal for setups where your Docker volumes are backed by a ZFS pool.

1. Ensure your ZFS pool exists and ZFS is installed:
   ```bash
   zpool list
   ```

2. Run the preflight script to validate or create the dataset:
   ```bash
   ./preflight.sh
   ```

3. Launch the stack:
   ```bash
   docker compose up -d
   ```

---

## 📦 Option B: Run without ZFS

> For typical Docker setups or development environments.

1. Set `PORTAINER_DATA_VOLUME=./data` in your `.env`.

2. Launch the stack:
   ```bash
   docker compose up -d
   ```

---

## 🌐 Access Portainer

Once running, open your browser to:

```
http://localhost:9000
```

Or replace `localhost` with your server’s IP and the `PORTAINER_PORT` you defined in `.env`.

> **Note:** Consider using [Nginx Proxy Manager](https://github.com/Vantasin/Nginx-Proxy-Manager.git) as a reverse proxy for HTTPS certificates via Let's Encrypt.

---

## 🤖 Optional: Ansible Integration

Use `env.j2` to generate `.env` using Ansible. If your Ansible role is templating per stack, you might even do:

```yaml
- name: Template .env for {{ stack.name }}
  template:
    src: "{{ compose_root }}/{{ stack.name }}/env.j2"
    dest: "{{ compose_root }}/{{ stack.name }}/.env"
```

Where compose_root is /tank/docker/compose.

This allows you to inject variables like `portainer_port`, `zfs_pool`, and `portainer_dataset_path` from Ansible inventory or vault.

---

## 📄 File Overview

| File                 | Description                                           |
|----------------------|-------------------------------------------------------|
| `docker-compose.yml` | Portainer container definition                        |
| `env.example`        | Example environment file for local overrides          |
| `env.j2`             | Ansible template for generating `.env`                |
| `preflight.sh`       | Optional script to prepare ZFS dataset (if enabled)   |
| `git_push.py`        | Helper script to stage, commit, and push to all remotes |
| `summarize_codebase.sh` | Script to generate codebase summary                 |

---

## 📝 License

Licensed under the [MIT License](LICENSE).

---

## 🙏 Acknowledgements

- [Portainer](https://www.portainer.io/)
- [Docker](https://www.docker.com/)
- [ZFS on Linux](https://openzfs.org/)
