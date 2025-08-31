# SSH Honeypot with Cowrie

This project runs a Cowrie SSH honeypot in Docker and logs brute-force attempts for analysis.

## Quick Start
```bash
git clone https://github.com/ufukulaserdem/ssh-honeypot-cowrie
cd ssh-honeypot-cowrie/infra
sudo docker-compose up -d
```

Now connect to the host/VM on port **22** to trigger the honeypot:
```bash
ssh -p 22 <IP>
```

Logs:
```
cowrie/var/log/cowrie/cowrie.json
```

## Architecture
```
[Internet/LAN] → host:22  → container:2222 (Cowrie)
[Admin SSH]    → host:22222 (real SSH, separate from the honeypot)
```

## Notes
- Use at your own risk; honeypots attract noise.
- Sanitize data before sharing publicly.
