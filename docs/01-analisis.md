#  Análisis de Requisitos

## 1. Contexto del Cliente
PYME del sector servicios que requiere:
- Presencia web pública (sitio corporativo + formulario de contacto)
- Sistema interno de gestión (inventario, facturación básica)
- Acceso remoto seguro para administradores
- Cumplimiento básico de seguridad y disponibilidad

## 2. Requisitos Funcionales
| ID | Requisito | Prioridad |
|----|-----------|-----------|
| RF-01 | Servidor web con soporte PHP 8+ | Alta |
| RF-02 | Base de datos relacional para web y gestión | Alta |
| RF-03 | Acceso SSH con autenticación por clave | Alta |
| RF-04 | Monitorización de recursos en tiempo real | Media |
| RF-05 | Backups automáticos con retención 30 días | Alta |

## 3. Requisitos No Funcionales
- **Disponibilidad**: 99.5% mensual
- **RTO**: ≤ 4 horas | **RPO**: ≤ 24 horas
- **Seguridad**: Firewall activo, puertos mínimos abiertos, actualizaciones automáticas de seguridad
- **Escalabilidad**: Diseñado para crecer a un cluster básico si la carga aumenta

## 4. Restricciones y Supuestos
- Presupuesto limitado → uso de software open-source
- Sin hardware dedicado → máquina virtual en proveedor cloud (2 vCPU, 4GB RAM, 40GB SSD)
- Se asume conectividad IPv4 estática
- No se incluye configuración de certificados SSL wildcards (se delega a Let's Encrypt en fase 2)