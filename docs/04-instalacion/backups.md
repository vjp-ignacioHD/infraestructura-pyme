# 💾 Política de Copias de Seguridad y Recuperación

> 📌 **Objetivo**: Garantizar la integridad y disponibilidad de los datos críticos de la PYME mediante copias de seguridad automatizadas, verificadas y con rotación controlada.

---

## 1. Estrategia de Backup

### 1.1. Tipo de Copias

| Tipo | Frecuencia | Método | Retención | Destino |
|------|------------|--------|-----------|---------|
| **MySQL Full** | Diario 02:00 | `mysqldump` + gzip | 30 días | `/backup/mysql/` + remoto |
| **Archivos Web** | Diario 03:00 | `rsync` incremental | 15 días | `/backup/www/` + remoto |
| **Configuraciones** | Semanal (Lunes 04:00) | `tar` + gzip | 90 días | `/backup/configs/` |
| **Logs críticos** | Mensual | `tar` + gzip | 1 año | `/backup/logs/` |

### 1.2. Métricas RPO / RTO

| Métrica | Valor | Justificación |
|---------|-------|----------------|
| **RPO** (Recovery Point Objective) | 24 horas | Se acepta pérdida de máximo 1 día de datos |
| **RTO** (Recovery Time Objective) | 4 horas | Servicio debe restaurarse en medio día laboral |

### 1.3. Regla 3-2-1 (Recomendada)
- ✅ **3** copias de los datos (original + 2 backups)
- ✅ **2** medios de almacenamiento diferentes (local + remoto/cloud)
- ✅ **1** copia fuera del sitio principal (off-site)

### 1.4. Componentes a respaldar

| Componente | Ruta / Recurso | Frecuencia | Método |
|------------|----------------|------------|---------|
| Base de datos (web) | `miweb` | Diaria | `mysqldump` |
| Base de datos (gestión) | `gestion_interna` | Diaria | `mysqldump` |
| Archivos web | `/var/www/html/` | Diaria | `rsync` |
| Configuración Apache | `/etc/apache2/` | Semanal | `tar` |
| Configuración MySQL | `/etc/mysql/` | Semanal | `tar` |
| Scripts y cron | `/usr/local/bin/`, `/etc/crontab` | Semanal | `tar` |

---

## 2. Preparación del Entorno

### 2.1. Crear estructura de directorios
```bash
# Crear árbol de directorios completo
sudo mkdir -p /backup/{mysql,www,configs,logs}
sudo chown root:root /backup -R
sudo chmod 700 /backup -R

# Estructura resultante:
# /backup/
# ├── mysql/       # Backups de bases de datos (.sql.gz)
# ├── www/         # Backups de archivos web (.tar.gz)
# ├── configs/     # Backups de configuraciones (.tar.gz)
# └── logs/        # Registros de ejecución (.log)