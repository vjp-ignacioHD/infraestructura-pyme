# 🔒 SSH y Firewall UFW

> 📌 **Objetivo:** Asegurar el acceso remoto al servidor mediante SSH hardening y configurar un firewall básico con UFW para proteger los servicios expuestos.

---

## 1. Hardening SSH

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
