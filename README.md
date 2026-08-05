# skills-AQ

Marketplace privado de Claude Code con los skills internos de AquaChile. Repo privado; instalarlo
requiere ser colaborador en GitHub.

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

Editar `plugins/skills-aq/skills/<skill>/SKILL.md`, subir la versión en
`plugins/skills-aq/.claude-plugin/plugin.json`, commitear y pushear a `main`. Cada compañero
levanta el cambio con `/plugin install skills-aq`.
