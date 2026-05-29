# 🔍 Revisión del Proyecto - Infraestructura PYME

> 📅 **Fecha**: 2026-05-29  
> 👥 **Equipo**: Eva e Ignacio  
> 🏷️ **Versión**: v1.0

---

## ❓ ¿Qué conflictos tuvisteis y cómo los resolvisteis?

### Conflicto 1: Merge de README.md
**Situación**: Ambos modificamos `README.md` en ramas diferentes.

**Resolución**: Usamos `git mergetool` para fusionar manualmente conservando ambas contribuciones.

### Conflicto 2: Rebase en CHANGELOG.md
**Situación**: Durante el `git pull --rebase`, `CHANGELOG.md` tenía entradas duplicadas.

**Resolución**: Editamos manualmente unificando las entradas y ejecutamos `git rebase --continue`.

---

## 💻 ¿Qué comandos Git usasteis más?

| Comando | Propósito |
|---------|-----------|
| `git checkout -b` | Crear ramas de características |
| `git add/commit/push` | Guardar cambios diariamente |
| `git pull --rebase` | Sincronizar antes de PR |
| `git status` | Verificar estado del working directory |
| `git log --oneline --graph` | Visualizar historial de ramas |
| `git merge --no-ff` | Mantener traza de ramas fusionadas |

---

## 🔄 ¿Qué haríais diferente en un próximo proyecto?

1. Usar plantillas de issues (`ISSUE_TEMPLATE.md`)
2. Automatizar validación de enlaces con GitHub Actions
3. Definir un `CONTRIBUTING.md` con normas claras
4. Proteger la rama `main` requiriendo aprobación en PRs

---

## 👥 ¿Cómo os funcionó el intercambio de roles?

**Valoración: 4/5**

**Lo que funcionó bien:**
- Aprendimos ambas perspectivas: autor y revisor
- Es más fácil encontrar errores ajenos que propios
- Mejoró la comunicación del equipo

**Dificultades:**
- Costó adaptarse al rol de revisor al principio
- Un conflicto de rebase nos bloqueó 15 minutos

---

## 📊 Conclusión

Hemos logrado un repositorio completo con 8 documentos markdown, historial Git profesional con ramas y PRs, 2 conflictos reales resueltos, HAProxy integrado, y release v1.0 publicado.

*Reflexión elaborada por Eva e Ignacio - Sesión 4*