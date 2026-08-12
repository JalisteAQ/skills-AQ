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
| `optimizar-prompt` | Verifica un prompt contra las buenas prácticas oficiales de Anthropic y lo entrega cumpliendo todos los checks; pregunta lo que falte, jamás infiere |
| `generar-especificaciones` | Entrevista en lenguaje de negocio para convertir una idea en un spec markdown más un prompt optimizado que lo referencia, listo para desarrollar en otra sesión |

## Agregar o corregir un skill

Editar o agregar la carpeta en `plugins/skills-aq/skills/<skill>/SKILL.md`, commitear y pushear a
`main`. El plugin no fija versión (usa el commit como referencia), así que no hay que subir ningún
número. Cada compañero levanta los cambios corriendo:

```bash
/plugin marketplace update
/plugin update
```
