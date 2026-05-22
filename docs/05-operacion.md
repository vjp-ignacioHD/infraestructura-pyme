
---

### 📄 `docs/05-operacion.md`
```markdown
# 🛠️ Guía de Operación y Mantenimiento

## 1. Tareas Diarias
- [ ] Revisar alertas Netdata
- [ ] Verificar logs de errores: `journalctl -u apache2 --since today`
- [ ] Confirmar ejecución de backups: `ls -lh /backup/`
- [ ] Revisar intentos fallidos SSH: `grep "Failed password" /var/log/auth.log`

## 2. Tareas Semanales
- [ ] Actualizar paquetes de seguridad: `sudo apt update && sudo apt upgrade -y`
- [ ] Rotar logs: `sudo logrotate -f /etc/logrotate.d/apache2`
- [ ] Validar integridad de backups: `gunzip -t /backup/mysql/full_*.sql.gz`

## 3. Tareas Mensuales
- [ ] Revisar métricas de rendimiento (CPU, RAM, Disco, MySQL slow queries)
- [ ] Cambiar contraseñas de servicio (si aplica política)
- [ ] Actualizar CHANGELOG.md y documentación obsoleta

## 4. Gestión de Usuarios
- Crear: `sudo adduser [nombre] && sudo usermod -aG sudo [nombre]`
- Eliminar: `sudo deluser --remove-home [nombre]`
- Auditoría SSH: `sudo cat /etc/ssh/authorized_keys`