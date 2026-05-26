
---

### 📄 `docs/05-operacion.md`
```markdown
# 🛠️ Guía de Operación y Mantenimiento

## 1. Tareas Diarias

### 1.1. Monitorización Temprana (08:00 - 09:00)
- [ ] **Revisar dashboard Netdata**: Acceder a `https://[IP]:19999` y verificar:
  - CPU usage < 80% promedio
  - RAM disponible > 20%
  - Disco raíz `/` < 85% uso
  - MySQL threads running < 50
  - Apache workers disponibles > 10
- [ ] **Verificar alertas por email**: Revisar bandeja de `sysadmin@[dominio]` en busca de notificaciones automáticas
- [ ] **Confirmar ejecución de backups**: 
  ```bash
  ls -lh /backup/mysql/ | grep $(date +%F)    # Debe existir backup de hoy
  ls -lh /backup/www/daily_$(date +%F)        # Snapshot web presente
  tail -20 /var/log/backup.log                # Sin errores "❌"

## 2. Tareas Semanales
# 1. Listar actualizaciones disponibles (solo seguridad)
sudo apt update
sudo apt list --upgradable | grep -i security

# 2. Aplicar actualizaciones críticas
sudo apt upgrade -y --only-upgrade $(apt list --upgradable | grep security | cut -d'/' -f1)

# 3. Reiniciar servicios afectados (si es necesario)
sudo systemctl restart apache2 mysql

# 4. Validar post-actualización
sudo systemctl status apache2 mysql --no-pager
curl -sI http://localhost | grep HTTP


# Rotación manual de logs (si logrotate no ejecutó)
sudo logrotate -f /etc/logrotate.conf

# Limpiar paquetes cache antiguos
sudo apt autoclean -y
sudo apt autoremove -y

# Verificar espacio en disco por directorio
sudo du -h / --max-depth=2 | grep -E '[0-9]G' | sort -hr

# Limpiar logs antiguos de Netdata (retención > 30 días)
sudo find /var/cache/netdata -type f -mtime +30 -delete- [ ] 

# Verificar integridad de backups MySQL
for f in /backup/mysql/full_*.sql.gz; do
    if ! gzip -t "$f" 2>/dev/null; then
        echo "❌ CORRUPTO: $f" | mail -s "ALERTA: Backup corrupto" admin@[dominio]
    fi
done

# Test de restauración en entorno aislado (mensual recomendado)
# gunzip -c /backup/mysql/full_YYYY-MM-DD.sql.gz | mysql -u root -p test_restore

# Verificar sync remoto
sudo -u backup-user rsync -avn /backup/ user@remote:/backups/pyme/

## 3. Tareas Mensuales
- [ ] Revisar métricas de rendimiento (CPU, RAM, Disco, MySQL slow queries)
- [ ] Cambiar contraseñas de servicio (si aplica política)
- [ ] Actualizar CHANGELOG.md y documentación obsoleta

## 4. Gestión de Usuarios
- Crear: `sudo adduser [nombre] && sudo usermod -aG sudo [nombre]`
- Eliminar: `sudo deluser --remove-home [nombre]`
- Auditoría SSH: `sudo cat /etc/ssh/authorized_keys`