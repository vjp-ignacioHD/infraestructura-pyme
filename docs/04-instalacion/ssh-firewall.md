
---

### 📄 `docs/04-instalacion/ssh-firewall.md`
```markdown
# 🔒 SSH y Firewall UFW

## 1. Hardening SSH (`/etc/ssh/sshd_config`)
```ini
Port 22
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2

## Configuración de firewall con UFW
- `ufw default deny incoming`
- `ufw allow 22/tcp`   # SSH para administración
- `ufw allow 80,443/tcp`  # Web
- `ufw enable`