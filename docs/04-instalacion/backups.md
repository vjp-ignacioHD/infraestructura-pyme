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

### 1.2. Regla 3-2-1 (Recomendada)
- ✅ **3** copias de los datos (original + 2 backups)
- ✅ **2** medios de almacenamiento diferentes (local + remoto/cloud)
- ✅ **1** copia fuera del sitio principal (off-site)

---

## 2. Preparación del Entorno

### 2.1. Crear estructura de directorios
```bash
sudo mkdir -p /backup/{mysql,www,configs,logs}
sudo chown root:root /backup -R
sudo chmod 700 /backup -R