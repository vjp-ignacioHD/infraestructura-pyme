# 🆘 Plan de Recuperación ante Desastres (DRP)

## 1. Objetivos
- **RTO**: ≤ 4 horas
- **RPO**: ≤ 24 horas (último backup diario)

## 2. Escenarios y Procedimientos
| Escenario | Síntoma | Acción |
|-----------|---------|--------|
| Caída Apache | HTTP 503 / puerto 80 cerrado | `sudo systemctl restart apache2` → revisar logs |
| Corrupción MySQL | Tablas inaccesibles / error InnoDB | Restaurar desde `mysqldump` + `mysql < backup.sql` |
| Pérdida de disco | Sistema no arranca | Desplegar nueva VM → restaurar backups + configs |
| Compromiso SSH | Logs muestran accesos no autorizados | Aislar red → cambiar claves → auditar con `rkhunter` |

## 3. Procedimiento de Restauración Completo
1. Aprovisionar nueva instancia Ubuntu 22.04
2. Aplicar hardening SSH + UFW (ver `ssh-firewall.md`)
3. Instalar LAMP (ver `servidor-web.md`, `base-de-datos.md`)
4. Restaurar configs: `rsync -a /backup/configs/ /etc/`
5. Restaurar DB: `zcat /backup/mysql/full_YYYY-MM-DD.sql.gz | mysql -u root -p`
6. Restaurar web: `rsync -a /backup/www/ /var/www/html/`
7. Validar servicios y monitorización

## 4. Comunicación
- Notificar a [Contacto Cliente] en < 1 hora
- Actualizar estado en [Canal/Issue]
- Documentar post-mortem en `REVISION.md`