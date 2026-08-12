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

## Usar un skill

Dos formas, en cualquier sesión de Claude Code con el plugin instalado:

- **Comando:** escribir `/skills-aq:<nombre>`, con el argumento opcional a continuación. Ejemplo:
  `/skills-aq:generar-especificaciones una app para coordinar turnos de planta`.
- **Lenguaje natural:** pedirlo con sus propias palabras ("optimiza este prompt", "quiero
  especificar una idea", "genera un handoff de la sesión") y Claude invoca el skill solo.

El menú de comandos `/` se arma al iniciar la sesión: después de instalar o actualizar el plugin,
reiniciar Claude Code para que los skills nuevos aparezcan en el autocompletado.

## Agregar o corregir un skill

Editar o agregar la carpeta en `plugins/skills-aq/skills/<skill>/SKILL.md`, commitear y pushear a
`main`. El plugin no fija versión (usa el commit como referencia), así que no hay que subir ningún
número. Cada compañero levanta los cambios corriendo:

```bash
/plugin marketplace update
/plugin update
```
