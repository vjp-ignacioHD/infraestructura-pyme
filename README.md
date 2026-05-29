# 🌐 Infraestructura LAMP para PYME

Documentación técnica completa para el despliegue, operación y recuperación de un entorno LAMP (Linux, Apache, MySQL, PHP) con monitorización y copias de seguridad automatizadas.

## 👥 Equipo
- [Ignacio Hoyas De Diego] – Rol: [Documentalista de Plataforma/Operaciones]
- [Eva Cantero Abad] – Rol: [Documentalista de Plataforma/Operaciones]

## 📚 Estructura de Documentación
| Archivo | Descripción |
|---------|-------------|
| `docs/01-analisis.md` | Requisitos del cliente y alcance |
| `docs/02-diseno.md` | Arquitectura y diagrama de red |
| `docs/03-planificacion.md` | Fases, cronograma y gestión de riesgos |
| `docs/04-instalacion/` | Guías de instalación y hardening |
| `docs/05-operacion.md` | Mantenimiento diario y monitoreo |
| `docs/06-recuperacion.md` | Plan de recuperación ante desastres |
| `CHANGELOG.md` | Historial de versiones |
| `tareas.md` | Seguimiento de tareas pendientes |

##  Estado Actual
- [x] Análisis y diseño
- [x] Instalación y hardening
- [x] Monitorización y backups
- [ ] Validación final y entrega v1.0

## 🔗 Enlaces Útiles
- [GitHub Repository](https://github.com/vjp-ignacioHD/infraestructura-pyme)
- [Issues Activos](https://github.com/vjp-ignacioHD/infraestructura-pyme/issues)
- [Release v1.0](https://github.com/vjp-ignacioHD/infraestructura-pyme/releases)

# 🏢 Infraestructura PYME - Proyecto Final

## 📋 Descripción del Proyecto

Despliegue de una infraestructura completa para una pequeña y mediana empresa (PYME) que incluye:

- **Servidor Web**: Apache 2.4 con PHP 8.1
- **Base de Datos**: MySQL 8.0
- **Balanceador de Carga**: HAProxy para alta disponibilidad y escalabilidad
- **Seguridad**: UFW + Fail2ban
- **Monitorización**: Netdata con alertas por email
- **Backups**: Automatizados con mysqldump + rsync

## 🏗️ Arquitectura
[Usuarios] → [HAProxy] → [Apache + PHP] → [MySQL]
↓
[Netdata + Alertas]
↓
[Backups Diarios]

text
## 📁 Estructura del Documento

| Documento | Descripción |
|-----------|-------------|
| `docs/01-analisis.md` | Análisis de requisitos y restricciones |
| `docs/02-diseno.md` | Diseño de arquitectura y componentes **(incluye HAProxy)** |
| `docs/03-planificacion.md` | Planificación temporal y riesgos **(actualizado con fase balanceo)** |
| `docs/04-instalacion/` | Guías de instalación paso a paso |
| `docs/05-operacion.md` | Mantenimiento y tareas operativas |
| `docs/06-recuperacion.md` | Plan de recuperación ante desastres |

## 🚀 Guía Rápida

### Clonar el repositorio
```bash
git clone https://github.com/vjp-ignacioHD/infraestructura-pyme.git
cd infraestructura-pyme

> ⚠️ Este repositorio contiene **documentación técnica**, no scripts de ejecución automática. Todos los comandos deben validarse en entorno de pruebas antes de aplicar en producción.