
---

### 📄 `docs/03-planificacion.md`
```markdown
# 📅 Planificación del Proyecto

## 1. Fases de Despliegue
| Fase | Actividades | Duración | Responsable |
|------|-------------|----------|-------------|
| 1. Análisis | Requisitos, restricciones, alcance | 1 sesión | Ambos |
| 2. Diseño | Arquitectura, red, componentes | 1 sesión | Plataforma |
| 3. Instalación | SO, LAMP, seguridad, monitorización | 2 sesiones | Rotativo |
| 4. Operación | Backups, mantenimiento, documentación | 1 sesión | Operaciones |
| 5. Validación | Pruebas, hardening final, release | 1 sesión | Ambos |

## 2. Cronograma (Gantt Simplificado)
| Semana | Tarea | Estado |
|--------|-------|--------|
| S1 | Repositorio, estructura, PR iniciales | ✅ |
| S2 | Revisión cruzada, merge, conflicto 1 | ✅ |
| S3 | Intercambio roles, conflicto 2 (rebase) | ✅ |
| S4 | HAProxy (opcional), release v1.0, REVISION.md | 🟡 |

## 3. Matriz de Riesgos
| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Pérdida de configuración | Baja | Alto | Backups diarios + control de versiones |
| Ataque por fuerza bruta SSH | Media | Alto | Claves SSH + Fail2ban + UFW |
| Caída de MySQL | Baja | Alto | Monitorización + alertas + DR plan |
| Conflictos en Git | Alta | Bajo | PR obligatorios, pull frecuente, comunicación |