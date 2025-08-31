# SSH Honeypot with Cowrie (Docker)

Dockerized SSH honeypot using [Cowrie](https://github.com/cowrie/cowrie).
Host port **22** serves the honeypot; your real SSH should live on a different port (e.g., **22222**).

## Structure
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

## Run
```bash
# from repo root:
mkdir -p cowrie/{etc,var}
# ensure etc files exist (see above), then:
cd infra
sudo docker-compose up -d
sudo docker ps
```

Test it from another machine (or host):
```bash
ssh -p 22 <VM_OR_HOST_IP>
```

View logs:
```bash
tail -f ../cowrie/var/log/cowrie/cowrie.json
```

## Operational Notes
- Real SSH for admin should listen on **22222**.
- Honeypot keys change on container rebuild unless you persist them; with the `etc/` bind mount in this repo, keys persist after the first run.
- Logs can grow; `docker` log rotation is enabled. Rotate/trim `cowrie.json` as needed.

## Risks & Limits
- Honeypot does **not** harden your system; it only records activity.
- Mostly captures bot traffic; advanced actors may detect the emulation.
- If publishing data, remove/obfuscate PII (IPs, usernames).

## Tear Down
```bash
cd infra
sudo docker-compose down
```
## Acknowledgments

This project uses [Cowrie](https://github.com/cowrie/cowrie), an SSH/Telnet honeypot written in Python.  
All credit for the honeypot engine goes to the Cowrie maintainers — this repo only provides a Docker-based setup and educational wrapper.
