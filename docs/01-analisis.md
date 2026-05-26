# 📋 01. Análisis de Requisitos

> 📌 **Documento vivo**: Este archivo define el alcance técnico y funcional del proyecto. Cualquier cambio debe gestionarse mediante Issue + PR y quedar registrado en `CHANGELOG.md`.
> 
> 📅 **Última revisión**: 2026-05-26 | 👥 **Autores**: [Nombre A] / [Nombre B] | 🔗 **Trazabilidad**: `docs/02-diseno.md` → `docs/04-instalacion/`

---

## 1. Contexto del Cliente y Objetivos de Negocio

### 1.1. Perfil del Cliente
Pequeña y mediana empresa (PYME) del sector servicios con operación presencial y remota. Requiere digitalizar su presencia pública y automatizar procesos internos básicos sin inversión en licencias propietarias.

### 1.2. Objetivos Estratégicos
| Objetivo | Impacto Esperado | Métrica de Éxito |
|----------|------------------|------------------|
| Publicar sitio corporativo | Visibilidad y captación de leads | Uptime ≥ 99.5% mensual |
| Centralizar gestión interna | Reducción de errores manuales | 100% de operaciones CRUD registradas en BD |
| Garantizar continuidad operativa | Minimizar impacto por fallos | RTO ≤ 4h | RPO ≤ 24h |
| Cumplir estándares básicos de seguridad | Protección de datos y acceso | 0 vulnerabilidades críticas en auditoría inicial |

### 1.3. Alcance del Proyecto
#### ✅ In-Scope (Incluido)
- Despliegue de pila LAMP sobre Ubuntu Server 22.04 LTS
- Hardening de SSH, firewall UFW y Fail2ban
- Monitorización básica con Netdata + alertas por correo
- Política de backups automatizados (MySQL + Web + Configs) con rotación
- Documentación técnica completa y plan de recuperación (DRP)

#### ❌ Out-of-Scope (Excluido)
- Arquitectura de alta disponibilidad (clusters, balanceo activo-activo)
- Contenerización (Docker/Kubernetes) o orquestación
- Integración con directorios corporativos (Active Directory/LDAP)
- Desarrollo de aplicaciones web o personalización de CMS
- Certificaciones compliance (PCI-DSS, ISO 27001, GDPR avanzado)

---

## 2. Requisitos Funcionales (RF)

| ID | Requisito | Descripción Técnica | Prioridad | Documento de Referencia |
|----|-----------|---------------------|-----------|-------------------------|
| **RF-01** | Servidor Web + PHP | Apache 2.4 con módulo `libapache2-mod-php` o PHP-FPM. Soporte para PHP 8.1+. VirtualHosts configurados y hardening básico. | 🔴 Alta | `docs/04-instalacion/servidor-web.md` |
| **RF-02** | Gestión de Datos | MySQL 8.0 / MariaDB 10.6. Esquemas aislados (`app_web`, `app_gestion`). Usuarios con privilegios mínimos. Backup lógico incluido. | 🔴 Alta | `docs/04-instalacion/base-de-datos.md` |
| **RF-03** | Acceso Remoto Seguro | OpenSSH con autenticación por clave pública. Root login deshabilitado. Restricción por IP origen. Fail2ban activo. | 🔴 Alta | `docs/04-instalacion/ssh-firewall.md` |
| **RF-04** | Monitorización Operativa | Agente/dashboard Netdata. Métricas de CPU, RAM, Disco, Red y servicios. Alertas configuradas para umbrales críticos. | 🟡 Media | `docs/04-instalacion/monitorizacion.md` |
| **RF-05** | Copias de Seguridad | Automatización vía cron. `mysqldump` comprimido + `rsync` incremental. Retención 30/15/90 días. Verificación de integridad. | 🔴 Alta | `docs/04-instalacion/backups.md` |
| **RF-06** | Documentación y DRP | Guías de instalación, operación diaria y plan de recuperación ante desastres con procedimientos validados. |  Alta | `docs/05-operacion.md` + `docs/06-recuperacion.md` |

---

## 3. Requisitos No Funcionales (RNF)

### 3.1. Rendimiento y Escalabilidad
- Tiempo de respuesta HTTP < 2s para páginas estáticas
- Soporte estimado: hasta 500 sesiones concurrentes (tráfico PYME típico)
- Capacidad de escalado vertical sin rediseño arquitectónico (hasta 8 vCPU / 16GB RAM)

### 3.2. Disponibilidad y Continuidad
- **SLA Objetivo**: 99.5% mensual (~3.6h de indisponibilidad permitida)
- **Ventana de mantenimiento**: Domingos 02:00–04:00 UTC (comunicada con 48h de antelación)
- **RTO** (Recovery Time Objective): ≤ 4 horas
- **RPO** (Recovery Point Objective): ≤ 24 horas

### 3.3. Seguridad y Cumplimiento
- Principio de menor privilegio en todos los servicios y usuarios
- Cifrado en tránsito: TLS 1.2+ (preparado para Let's Encrypt en fase 2)
- Hardening alineado a CIS Ubuntu Linux 22.04 LTS Benchmark (Nivel 1)
- Logs de acceso y auditoría retenidos mínimo 90 días

### 3.4. Mantenibilidad y Operación
- Configuración reproducible mediante scripts bash documentados
- Estructura de directorios estandarizada (`/var/www`, `/etc`, `/backup`)
- Documentación versionada con Git. CHANGELOG actualizado en cada release
- Procedimientos de operación validados en entorno de pruebas antes de producción

---

## 4. Restricciones Técnicas y Supuestos

### 4.1. Restricciones
| Tipo | Descripción | Impacto |
|------|-------------|---------|
| 💰 Presupuesto | Sin licencias de pago. Solo software open-source y repositorios oficiales | Limita herramientas enterprise (ej. Veeam, Plesk) |
| 🖥️ Hardware | VM cloud básica: 2 vCPU, 4GB RAM, 40GB SSD NVMe | Requiere tuning de MySQL y limits de procesos |
| 🌐 Red | IPv4 estática proporcionada por proveedor. Sin IPv6 nativo | Configuración UFW y DNS orientada a IPv4 |
| 🔐 Certificados | No se incluye wildcard ni gestión automatizada en v1.0 | Se delega a fase 2 con Certbot + cron |

### 4.2. Supuestos Validados
- ✅ El cliente proporcionará nombre de dominio y acceso a gestión DNS
- ✅ El equipo de sistemas tendrá acceso root/sudo durante el despliegue
- ✅ No se requiere integración con sistemas legacy externos (ERP/CRM)
- ✅ Las aplicaciones web serán compatibles con PHP 8.1 y MySQL 8.0

---

## 5. Criterios de Aceptación (Definition of Done)

El proyecto se considerará **finalizado y apto para entrega** cuando se cumplan todos los siguientes puntos:

- [ ] `curl -I http://<IP>` devuelve `HTTP/1.1 200 OK` y cabeceras de seguridad activas
- [ ] PHP ejecuta `phpinfo()` y conecta a MySQL sin errores de privilegios
- [ ] UFW activo con solo puertos `22, 80, 443` abiertos. SSH restringido por IP
- [ ] Fail2ban bloquea IPs tras 3 intentos fallidos consecutivos
- [ ] Netdata accesible en `:19999` y muestra métricas de todos los servicios
- [ ] Backups se ejecutan automáticamente y pasan verificación `gzip -t` / `rsync --dry-run`
- [ ] Prueba de restauración completa realizada con éxito en entorno aislado
- [ ] Todos los documentos Markdown enlazan correctamente entre sí y pasan lint básico
- [ ] `REVISION.md` redactado y tag `v1.0` publicado en GitHub

---

## 6. Matriz de Riesgos Iniciales

| ID | Riesgo | Probabilidad | Impacto | Estrategia de Mitigación |
|----|--------|--------------|---------|--------------------------|
| R-01 | Caída de servicio por actualización no probada | Media | Alto | Ventana de mantenimiento + rollback documentado |
| R-02 | Pérdida de datos por fallo de disco | Baja | Crítico | Backups diarios + verificación semanal + sync off-site |
| R-03 | Acceso no autorizado vía SSH | Media | Alto | Claves SSH + Fail2ban + UFW + monitorización de logs |
| R-04 | Desbordamiento de disco por logs/backups | Alta | Medio | Rotación automática + alertas Netdata >85% uso |
| R-05 | Conflictos en colaboración Git | Alta | Bajo | PR obligatorios + revisión cruzada + pull frecuente de `main` |

---

> 📎 **Próximo paso**: Validar este análisis con el cliente/equipo y proceder a `docs/02-diseno.md` (Arquitectura y Diagrama de Red).
> 🔍 **Revisión técnica**: Comprobar versiones de software contra repositorios oficiales de Ubuntu 22.04 LTS antes de冻结r la línea base.