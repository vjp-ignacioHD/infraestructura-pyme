
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