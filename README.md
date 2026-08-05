# skills-AQ

Marketplace de Claude Code con algunos skills para el equipo de Excelencia Operacional de AquaChile,
agrupados en un plugin instalable con el comando de abajo.

## Instalar

```bash
/plugin marketplace add JalisteAQ/skills-AQ
/plugin install skills-aq
```

## Skills que incluye

| Skill | Qué hace |
| --- | --- |
| `genera-handoff` | Genera un handoff de sesión task-specific más un prompt para pegar en la siguiente sesión |

## Agregar o corregir un skill

Editar o agregar la carpeta en `plugins/skills-aq/skills/<skill>/SKILL.md`, commitear y pushear a
`main`. El plugin no fija versión (usa el commit como referencia), así que no hay que subir ningún
número. Cada compañero levanta los cambios corriendo:

```bash
/plugin marketplace update
/plugin update
```
