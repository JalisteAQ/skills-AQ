---
name: genera-handoff
description: Usar cuando pidan generar un handoff de sesión - produce el documento de retoma en disco y el prompt para pegar en la siguiente sesión
argument-hint: "foco o tarea para la siguiente sesión (opcional)"
disable-model-invocation: true
---

# Generar un handoff de sesión

Dos entregables, siempre ambos: (1) un documento en disco que otro agente sin contexto previo pueda
leer y desde el cual retomar la tarea sin preguntarle nada a nadie, y (2) un prompt corto, listo para
copiar y pegar, que arranca esa siguiente sesión.

## Idioma del documento de handoff

Escribe el documento de handoff en el idioma en que efectivamente se dio la sesión actual con el
usuario, inferido de la conversación. No usar un idioma fijo por defecto ni preguntar: solo seguir el
que ya está en uso.

## Antes de escribir

1. Si el usuario pasó un argumento al invocar el skill, trátalo como el foco de la siguiente sesión: qué
   debería hacer primero esa sesión, y ajusta el documento a eso.
2. Cada afirmación del documento debe tener de dónde sale (archivo, commit, comando corrido): nada de
   deixis de sesión ("como vimos", "la sesión anterior") que no signifique nada para quien nunca estuvo
   en esta conversación.
3. Detecta el repo activo (`git rev-parse --show-toplevel` en el directorio de trabajo actual):
   - Si hay repo: el documento va en `<repo>/docs/handoffs/YYYY-MM-DD-<slug-de-la-tarea>.md`, con la
     fecha de hoy en forma absoluta (`2026-08-05`, nunca "hoy"). El slug es específico a la tarea
     justamente para esto: nunca sobrescribir un handoff existente. Si ya hay un archivo en esa ruta
     (dos handoffs el mismo día para tareas parecidas, o un `HANDOFF.md` genérico que alguien dejó en
     otro lado), afinar el slug hasta que el nombre sea único en vez de pisarlo.
   - Si NO hay repo (una sesión suelta en un directorio sin git): usa el scratchpad de la sesión actual,
     y dilo explícitamente en el chat, esa ruta es efímera y ninguna otra sesión la va a ver a menos que
     alguien la mueva.
4. No commitear el archivo por cuenta propia. Se entrega escrito; el commit lo decide quien lo pidió.

## Contenido del documento (específico a la tarea, no una plantilla genérica)

Sin secciones vacías "por si acaso". Solo lo que esta tarea puntual necesita para poder retomarse, como
mínimo:

1. **Título + fecha absoluta.**
2. **Reglas duras que aplican**, si hay alguna crítica para no romper algo al retomar (con qué cuenta
   pushear, repo/rama correcto, modo solo lectura, etc.), solo si aplica, por sobre todo lo demás.
3. **Qué se pidió**, con las palabras de quien lo pidió, sin paráfrasis.
4. **Estado actual accionable**: qué está hecho, qué falta, y la próxima acción concreta (el próximo
   comando, archivo o punto de decisión, no "seguir explorando").
5. **Contexto necesario**: repos, rutas absolutas, ramas, comandos ya corridos y su resultado,
   herramientas/MCPs relevantes para esta tarea.
6. **Decisiones tomadas y su razón**, una línea cada una, sin narrar cómo se llegó a ellas.
7. **Pendientes**, cada uno con su experimento o próximo paso, costo estimado si aplica, y estado
   (`no corrido`, `parcial`).
8. **Punteros absolutos**: archivos (`archivo.py:123`), commits, PRs, ramas, memorias relevantes
   (`[[nombre-de-memoria]]` donde aplique).
9. **Skills sugeridos** para la siguiente sesión, con la razón de cada uno.
10. Tapar cualquier secreto, token o credencial antes de guardar.

No duplicar contenido que ya vive en otro artefacto (spec, plan, PR, commit, issue): apuntar a él por
ruta o URL, no copiarlo.

Apuntar a no más de ~150 líneas. Pasado ese largo, el agente que retoma lee peor, no mejor: si el
documento se está yendo largo, es señal de que algo debería ser un puntero a otro artefacto en vez de
contenido copiado.

## Al terminar de escribir

Pásale esta prueba al documento: ¿alguien que nunca estuvo en esta sesión entiende qué responde el
documento y puede ejecutar la primera acción útil sin preguntarle nada a nadie? Si la respuesta es no,
seguir editando antes de entregarlo.

## Entrega

En el chat (no solo en el archivo), entrega dos cosas:

1. La ruta absoluta del documento guardado.
2. Un bloque de prompt listo para copiar y pegar en la siguiente sesión. El handoff es para que lo lea
   un agente, no una persona, así que ese prompt:
   - Es autocontenido: no asume que la siguiente sesión vio esta conversación.
   - Indica explícitamente la ruta absoluta del handoff y pide leerlo primero.
   - Resume la tarea en 1-2 líneas, para que quien lo pegue sepa qué está pegando sin abrir el archivo.
   - No repite el contenido completo del documento: el documento es la fuente, el prompt es el puntero.
   - Pide confirmar el entendimiento antes de ejecutar nada: el agente que retoma debe decir qué
     entendió del handoff antes de tocar código o correr comandos, nunca leer y partir a ejecutar
     directo.

Forma de referencia para ese bloque (adaptar al caso, no copiar literal):

```
Hola, estamos avanzando en <tarea>. Para eso parte leyendo <ruta absoluta al handoff>, porque
<qué se está haciendo>. Antes de ejecutar cualquier cosa, dime qué entendiste.
```
