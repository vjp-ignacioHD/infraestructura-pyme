# 🌐 Servidor Web Apache + PHP

> 📌 **Objetivo:** Instalar, configurar y asegurar el servidor web Apache con soporte PHP para alojar las aplicaciones web de la PYME.

---

## 1. Instalación de Apache y PHP

### 1.1. Instalación con apt
```bash
# Actualizar repositorios
sudo apt update && sudo apt upgrade -y

# Instalar Apache, PHP y módulos necesarios
sudo apt install apache2 php8.1 libapache2-mod-php8.1 php8.1-mysql php8.1-cli php8.1-curl php8.1-gd php8.1-mbstring php8.1-xml php8.1-zip -y

# Habilitar y arrancar el servicio
sudo systemctl enable apache2
sudo systemctl start apache2

# Verificar estado
sudo systemctl status apache2