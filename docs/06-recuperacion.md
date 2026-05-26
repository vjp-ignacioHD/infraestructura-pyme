# 🆘 06. Plan de Recuperación ante Desastres (DRP)

> 📌 **Documento crítico**: Define procedimientos, roles y tiempos de respuesta para restaurar servicios ante fallos graves. Debe revisarse trimestralmente y probarse en entorno aislado antes de aplicarse en producción.
> 
> 📅 **Última revisión**: 2026-05-26 | 👥 **Responsables DRP**: [Nombre A] / [Nombre B] | 🔗 **Relacionado**: `docs/04-instalacion/backups.md` | `docs/05-operacion.md`

---

## 1. Objetivos y Métricas de Recuperación

### 1.1. Indicadores Clave (SLA de Continuidad)
| Métrica | Valor Objetivo | Definición Técnica |
|---------|----------------|-------------------|
| **RTO** (Recovery Time Objective) | ≤ 4 horas | Tiempo máximo permitido desde la detección hasta la restauración operativa del servicio |
| **RPO** (Recovery Point Objective) | ≤ 24 horas | Pérdida máxima de datos aceptable (depende del último backup diario válido) |
| **MTTR** (Mean Time To Recovery) | < 3 horas | Tiempo promedio histórico de recuperación (meta de mejora continua) |

### 1.2. Alcance del Plan
- ✅ **Incluido**: Recuperación de SO, configuración de red/seguridad, stack LAMP, bases de datos, archivos web, monitorización y jobs de backup.
- ❌ **Excluido**: Recuperación de hardware físico损坏, desastres a nivel de datacenter, datos eliminados manualmente por usuarios sin backup previo, aplicaciones de terceros no documentadas.

---

## 2. Roles y Cadena de Mando (Activación DRP)

| Rol | Responsabilidad | Contacto / Canal |
|-----|-----------------|------------------|
| **Coordinador DR** | Decide activar el plan, autoriza cambios críticos, comunica con cliente | [Teléfono] / Slack `#drp-coord` |
| **Técnico de Recuperación** | Ejecuta procedimientos de restauración, valida servicios y datos | [Teléfono] / Slack `#drp-tech` |
| **Gestor de Comunicaciones** | Actualiza Issue/Slack, redacta avisos al cliente, controla expectativas | [Email] / Slack `#drp-comms` |
| **Auditor Post-Mortem** | Documenta cronología, identifica gaps, actualiza DRP y `CHANGELOG.md` | [Email] / GitHub Issues |

---

## 3. Escenarios de Fallo y Procedimientos de Respuesta

| Escenario | Severidad | Síntomas Clave | Acción Inmediata (Primeros 15 min) | Verificación |
|-----------|-----------|----------------|-----------------------------------|--------------|
| **Caída Apache/PHP** |  Alta | HTTP 503, timeout, puerto 80/443 sin respuesta | 1. `sudo systemctl restart apache2`<br>2. `journalctl -u apache2 --since "15 min ago"`<br>3. Verificar espacio disco y permisos `/var/www` | `curl -sI http://localhost` → `200 OK` |
| **Corrupción MySQL/InnoDB** | 🔴 Crítica | Tablas `crashed`, errores `InnoDB: Unable to lock`, apps sin BD | 1. `sudo systemctl stop mysql`<br>2. No forzar `innodb_force_recovery` sin backup<br>3. Preparar entorno limpio para restauración | `mysqlcheck -u root -p --all-databases` → `OK` |
| **Pérdida Total de Disco/VM** | 🔴 Crítica | SSH inaccesible, consola cloud sin respuesta, backups locales perdidos | 1. Activar DRP completo<br>2. Solicitar nueva VM al proveedor<br>3. Montar volúmenes de backup remoto | Ping/SSH reachable + servicios corriendo |
| **Compromiso de Seguridad** | 🔴 Crítica | Logs auth anómalos, procesos desconocidos, tráfico saliente sospechoso | 1. Aislar red o cambiar IP pública<br>2. No apagar (preservar forense)<br>3. Rotar todas las claves SSH y contraseñas | `rkhunter --check`, `chkrootkit`, revisión `netstat` |
| **Fallo de Backup Automático** | 🟡 Media | Cron no ejecuta, errores en `/var/log/backup.log`, espacio insuficiente | 1. Ejecutar backup manual inmediato<br>2. Diagnosticar causa (espacio, permisos, lock)<br>3. Reparar script/cron | Backup manual exitoso + verificación `gzip -t` |

---

## 4. Procedimiento de Restauración Completa (Paso a Paso)

> ⏱️ **Tiempo estimado**: 2.5 - 3.5 horas | 🎯 **Objetivo**: Servicio operativo con datos ≤ 24h antiguos

### 🔹 Fase 1: Aprovisionamiento y Hardening Base (30-45 min)
1. Crear nueva VM Ubuntu 22.04 LTS (mismo perfil: 2vCPU/4GB RAM/40GB SSD)
2. Configurar IP estática y DNS preliminar (TTL bajo configurado previamente)
3. Aplicar hardening inicial:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo ufw default deny incoming && sudo ufw allow 22/tcp && sudo ufw enable
   sudo adduser admin_dr && sudo usermod -aG sudo admin_dr
   # Configurar SSH con clave pública, deshabilitar root/password (ver ssh-firewall.md)