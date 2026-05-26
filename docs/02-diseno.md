# ️ Diseño de Infraestructura

## 1. Diagrama de Arquitectura
```mermaid
graph TD
    Internet[Internet] --> LB[HAProxy / Futuro Balanceador]
    LB --> APACHE[Apache 2.4 + PHP 8.1]
    APACHE --> DB[(MySQL 8.0)]
    APACHE --> MON[Netdata / Monitorización]
    APACHE --> FW[UFW + Fail2ban]
    DB --> BK[Backups mysqldump + rsync]

## 2. Componentes del Sistema

| Componente | Versión | Función |
|------------|---------|---------|
| Ubuntu Server | 22.04 LTS | SO base |
| Apache | 2.4.59 | Servidor web |
| PHP | 8.1.x | Ejecución de aplicaciones |
| MySQL | 8.0 | Gestión de datos |
| UFW | 0.36.1 | Firewall básico |
| Fail2ban | 0.11.2 | Protección contra fuerza bruta |
| Netdata | 1.40+ | Monitorización en tiempo real |
| Certbot | 1.0 | SSL |

## 3. Diseño de Red

- **Zona Pública**: Puertos `80/TCP`, `443/TCP`
- **Zona de Administración**: Puerto `22/TCP` restringido por IP
- **Zona Interna**: MySQL solo accesible desde `localhost`
- **DNS**: `www.[dominio].com` → `[IP_PUBLICA]`

## 4. Flujo de Datos

```mermaid
flowchart LR
    Client[Cliente] -->|HTTP/HTTPS| Apache
    Apache -->|PHP-FPM/ModPHP| App[Lógica de aplicación]
    App -->|localhost| MySQL[(MySQL)]
    Apache -->|API local| Netdata[Netdata]
    Cron[Cron] -->|Diario| Backup[Backups]
    Backup -->|rsync| Remote[Sync remoto]