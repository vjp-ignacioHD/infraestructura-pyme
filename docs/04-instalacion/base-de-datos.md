# 🗄️ Base de Datos MySQL / MariaDB

> 📌 **Objetivo:** Instalar, asegurar y configurar el servidor de bases de datos para las aplicaciones web y de gestión interna de la PYME.

---

## 1. Instalación del Servidor

### 1.1. Instalación con apt
```bash
# Actualizar repositorios
sudo apt update && sudo apt upgrade -y

# Instalar MySQL Server
sudo apt install mysql-server -y

# Verificar que el servicio está corriendo
sudo systemctl status mysql