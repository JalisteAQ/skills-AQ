---
name: generar-especificaciones
description: Usar cuando quieran convertir una idea en especificaciones - entrevista en lenguaje de negocio y entrega un spec en markdown más un prompt optimizado que lo referencia, para desarrollar en otra sesión
argument-hint: "la idea a especificar (opcional)"
---

# Generar especificaciones desde una idea

Convertir la idea de un usuario en dos entregables, siempre ambos: (1) un documento de
especificaciones en markdown guardado en disco, y (2) un prompt optimizado, listo para copiar y
pegar, que referencia ese spec para que otra sesión comience el desarrollo desde ahí.

## A quién le hablas

A un usuario de negocio (citizen user), no a un programador. Toda la interacción va **en español**
y en lenguaje corriente: nada de jerga técnica ni de código. Las preguntas se formulan en términos
del negocio ("¿cómo sabrías que quedó bien?"), nunca del oficio ("define criterios de aceptación").

## Flujo

1. **Recibir la idea.** Si el usuario pasó un argumento al invocar el skill, esa es la idea. Si no,
   pedirla: "Cuéntame tu idea, con tus palabras".
2. **Entrevistar.** Una pregunta a la vez, con alternativas concretas para elegir cuando se pueda.
   No pasar a la siguiente pregunta sin respuesta, y jamás rellenar un vacío por inferencia propia.
   Cubrir como mínimo:
   - **Propósito:** qué problema resuelve y por qué ahora.
   - **Para quién:** quiénes lo van a usar.
   - **Alcance:** qué sí debe hacer y qué NO (la frontera explícita).
   - **Criterios de éxito:** cómo se ve el resultado cuando quedó bien, en términos observables.
   - **Restricciones:** plazos, presupuesto, reglas del negocio que no se pueden romper.
   - **Sistemas y datos involucrados:** de dónde sale la información y por dónde se entrega.
3. **Redactar el spec** y guardarlo en el directorio de trabajo del usuario, en
   `especificaciones/AAAA-MM-DD-<tema>.md`, con la fecha de hoy en forma absoluta (`2026-08-12`,
   nunca "hoy"). No asumir que hay repo git. Nunca sobrescribir un archivo existente: si el nombre
   choca, afinar el tema hasta que sea único. No commitear por cuenta propia.
4. **Generar el prompt y optimizarlo.** Redactar el borrador del prompt que referencia la ruta
   absoluta del spec, e invocar el skill `optimizar-prompt` (vía la tool Skill) para verificarlo
   contra todos los checks de buenas prácticas de Anthropic. Las preguntas que surjan de esa
   verificación se le hacen al usuario en el mismo estilo de la entrevista. No entregar el prompt
   hasta que pase todos los checks.
5. **Entregar** en el chat (no solo en el archivo):
   - La ruta absoluta del spec guardado.
   - El bloque de prompt optimizado, listo para copiar y pegar en la sesión nueva.

## Contenido del spec (específico a la idea, no una plantilla genérica)

Sin secciones vacías "por si acaso". Solo lo que esta idea necesita, como mínimo:

1. **Título + fecha absoluta.**
2. **Qué se pidió**, con las palabras del usuario, sin paráfrasis.
3. **Propósito y para quién.**
4. **Alcance:** qué sí y qué no, en dos listas cortas.
5. **Criterios de éxito observables**, uno por línea.
6. **Restricciones.**
7. **Sistemas y datos involucrados.**
8. **Decisiones tomadas durante la entrevista y su razón**, una línea cada una.

Escribir el spec para que un agente que nunca estuvo en la conversación pueda desarrollarlo sin
preguntarle nada a nadie. Tapar cualquier secreto o dato sensible antes de guardar.

## Forma del prompt final

El prompt lo lee un agente en una sesión nueva, así que:

- Es autocontenido: no asume que la sesión nueva vio esta conversación.
- Indica la ruta absoluta del spec y pide leerlo completo antes de cualquier otra cosa.
- Resume la idea en 1-2 líneas, para que quien lo pegue sepa qué está pegando sin abrir el archivo.
- No repite el contenido del spec: el spec es la fuente, el prompt es el puntero.
- Pide confirmar el entendimiento antes de desarrollar: el agente debe decir qué entendió del spec
  antes de tocar código o correr comandos, nunca leer y partir a ejecutar directo.

Forma de referencia (adaptar al caso, no copiar literal):

```
Eres el desarrollador de <idea en 1-2 líneas>. Parte leyendo completo <ruta absoluta al spec>,
que contiene las especificaciones acordadas con el usuario. Antes de desarrollar cualquier cosa,
dime qué entendiste del spec y cómo vas a verificar que el resultado cumple sus criterios de éxito.
```

## Al terminar

Pásale esta prueba al conjunto: ¿alguien que nunca estuvo en esta conversación puede pegar el
prompt en una sesión nueva y esa sesión sabe qué construir, con qué límites y cómo verificar que
quedó bien, sin preguntarle nada a nadie? Si la respuesta es no, seguir editando antes de entregar.
