#  Servidor Web Apache + PHP

## 1. Instalación
```bash
sudo apt update && sudo apt install apache2 php8.1 libapache2-mod-php8.1 php8.1-mysql -y
sudo systemctl enable apache2 && sudo systemctl start apache2

## Configuración básica (/etc/haproxy/haproxy.cfg)

### Instalación
```frontend http_front
    bind *:80
    default_backend http_back

```backend http_back
    balance roundrobin
    server web1 192.168.1.10:80 check
    # server web2 192.168.1.11:80 check  # Futuro segundo nodo