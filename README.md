# SSH Honeypot with Cowrie (Docker)

[![MIT License](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)
[![GitHub Pages](https://img.shields.io/badge/docs-GitHub%20Pages-blue.svg)](https://github.com/ufukulaserdem/ssh-honeypot-cowrie)
[![Docker](https://img.shields.io/badge/docker-cowrie-blue?logo=docker)](https://hub.docker.com/r/cowrie/cowrie)
[![Made with Python](https://img.shields.io/badge/made%20with-Python%203.11-yellow.svg?logo=python)](https://www.python.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](./CONTRIBUTING.md)

## Overview
This project runs an **SSH honeypot** using [Cowrie](https://github.com/cowrie/cowrie) inside Docker.  
It listens on host port **22**, logs brute-force attempts and attacker activity, and stores data in JSON for later analysis.

> ⚠ This project is **for educational and research purposes only**.  
> Do not run honeypots in production networks. See [SECURITY.md](./SECURITY.md) for details.

---

## Project Structure
```
.
├─ infra/
│  └─ docker-compose.yml
└─ cowrie/
   ├─ etc/
   │  ├─ cowrie.cfg
   │  └─ userdb.txt
   └─ var/                 # logs/data (created at runtime)
      └─ log/cowrie/cowrie.json
```

---

## Quick Start
From the repo root:
```bash
mkdir -p cowrie/{etc,var}
# ensure etc files exist (see this repo for examples)
cd infra
sudo docker-compose up -d
sudo docker ps
```

### Test the honeypot
From another machine (or your host):
```bash
ssh -p 22 <VM_OR_HOST_IP>
```

### View logs
```bash
tail -f ../cowrie/var/log/cowrie/cowrie.json
```

---

## Operational Notes
- Real SSH for admin should listen on **22222** to avoid conflict.
- Cowrie generates host keys on first run; because of the `etc/` bind mount, they persist across container rebuilds.
- Logs can grow large. Docker log rotation is enabled (`max-size=50m`, `max-file=5`), but you should also monitor `cowrie.json` size.

---

## Risks & Limitations
- Honeypot does **not** harden your system — it only records attacker activity.
- You will mostly capture bot traffic and dictionary brute force attempts.
- Skilled attackers may detect Cowrie’s emulation quickly.
- Logs may contain IPs, usernames, or other identifiers. **Sanitize data** before publishing.

---

## Tear Down
```bash
cd infra
sudo docker-compose down
```

---

## Licensing
- This repository’s configuration and documentation: **MIT License** ([LICENSE](./LICENSE))  
- [Cowrie](https://github.com/cowrie/cowrie) honeypot: **BSD-2-Clause License** (see Cowrie repo)  
- This project does not modify Cowrie’s license; it only provides Docker setup and configs.

---

## Acknowledgments
- [Cowrie](https://github.com/cowrie/cowrie) — SSH/Telnet honeypot engine  
- [Docker](https://www.docker.com/) — container runtime  
- [Shields.io](https://shields.io) — for badges  up and educational wrapper.
