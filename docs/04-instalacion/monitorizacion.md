# 📊 Monitorización con Netdata

## 1. Objetivo
Establecer un sistema de monitorización que permita al equipo de administración detectar anomalías de forma proactiva en el servidor LAMP antes de que afecten a los servicios.

## 2. Alcance
- Monitorización de recursos del sistema (CPU, RAM, disco, red)
- Monitorización de servicios críticos (Apache, MySQL, SSH)
- Alertas básicas por consumo excesivo
- Dashboard visual para el administrador

## 3. Herramienta seleccionada: Netdata

| Característica | Justificación |
|----------------|----------------|
| Instalación sencilla | Un comando, sin dependencias complejas |
| Dashboard en tiempo real | Acceso vía puerto 19999 |
| Bajo consumo | ~1-2% CPU, ~50MB RAM |
| Alertas integradas | Configuración por defecto suficiente para PYME |
| Gratuito y open source | Sin costes de licencia |

> Alternativa considerada: script personalizado con `top`, `df`, `ss`. Descartada por falta de histórico y visualización amigable.

## 4. Instalación de Netdata

### 4.1. Requisitos previos
```bash
# Asegurar que el sistema está actualizado
sudo apt update && sudo apt upgrade -y

# Instalar dependencias necesarias
sudo apt install -y curl wget