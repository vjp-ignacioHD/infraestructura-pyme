#  Servidor Web Apache + PHP

## 1. Instalación
```bash
sudo apt update && sudo apt install apache2 php8.1 libapache2-mod-php8.1 php8.1-mysql -y
sudo systemctl enable apache2 && sudo systemctl start apache2