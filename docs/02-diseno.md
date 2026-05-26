# 🏗️ 02. Diseño de Infraestructura

> 📌 **Documento de arquitectura**: Define la topología, componentes, flujos de datos y decisiones técnicas del sistema.
> 
> 📅 **Versión**: 1.0 | 📆 **Fecha**: 2026-05-26 | 👥 **Autores**: Eva Nacho
> 🔗 **Trazabilidad**: ← `docs/01-analisis.md` | → `docs/03-planificacion.md` | → `docs/04-instalacion/`

---

## 1. Arquitectura General del Sistema

### 1.1. Diagrama de Arquitectura Lógica

```mermaid
graph TB
    subgraph "Internet"
        USERS[👥 Usuarios Web]
        ADMINS[👨‍💻 Administradores]
    end
    
    subgraph "Capa de Presentación - DMZ"
        LB[🔄 HAProxy<br/>Balanceador<br/>Futuro]
    end
    
    subgraph "Capa de Aplicación"
        APACHE[🌐 Apache 2.4 + PHP 8.1<br/>VirtualHosts]
        PHP[⚡ PHP-FPM<br/>Pool de procesos]
    end
    
    subgraph "Capa de Datos"
        MYSQL[(💾 MySQL 8.0<br/>InnoDB)]
        BK[💿 Backups<br/>mysqldump + rsync]
    end
    
    subgraph "Seguridad Perimetral"
        UFW[🛡️ UFW Firewall<br/>Reglas stateful]
        F2B[🚫 Fail2ban<br/>IPS]
    end
    
    subgraph "Monitorización"
        NET[📊 Netdata<br/>Métricas en tiempo real]
        ALERT[📧 Alertas Email]
    end
    
    USERS -->|HTTPS 443| LB
    ADMINS -->|SSH 22| UFW
    LB -->|HTTP 80| APACHE
    APACHE -->|FastCGI| PHP
    PHP -->|localhost:3306| MYSQL
    UFW -.-> APACHE
    F2B -.-> UFW
    APACHE -->|API:19999| NET
    MYSQL -->|02:00 AM| BK
    NET -->|Umbral crítico| ALERT
    
    style LB fill:#f9f,stroke:#333,stroke-width:2px
    style APACHE fill:#bbf,stroke:#333,stroke-width:2px
    style MYSQL fill:#bfb,stroke:#333,stroke-width:2px
    style UFW fill:#fbb,stroke:#333,stroke-width:2px
## 2. Componentes del Sistema

| Componente | Versión | Función |
|------------|---------|---------|
| Ubuntu Server | 22.04 LTS | SO base |
| Apache | Apache 2.4.60 | Servidor web |
| PHP | 8.1.x | Ejecución de aplicaciones |
| MySQL | 8.0 | Gestión de datos |
| UFW | 0.36.1 | Firewall básico |
| Fail2ban | 0.11.2 | Protección contra fuerza bruta |
| Netdata | 1.40+ | Monitorización en tiempo real |

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
