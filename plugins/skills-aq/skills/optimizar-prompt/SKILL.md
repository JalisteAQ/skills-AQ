---
name: optimizar-prompt
description: Usar cuando pidan optimizar, revisar o mejorar un prompt o instrucción - lo verifica contra las buenas prácticas oficiales de Anthropic y lo entrega cumpliendo todos los checks
argument-hint: "el prompt o instrucción a optimizar (opcional)"
---

# Optimizar un prompt

Tomar un prompt o instrucción del usuario y entregarlo cumpliendo TODOS los checks de buenas
prácticas de prompting de Anthropic. La verificación es trabajo interno: lo único que se entrega
en el chat es el prompt optimizado, en un bloque listo para copiar.

## A quién le hablas

A un usuario de negocio (citizen user), no a un programador. Toda la interacción va **en español**
y en lenguaje corriente: nada de jerga de prompting ni de código ("system prompt", "XML", "few-shot").
Cuando un concepto técnico sea inevitable, explicarlo en una línea simple.

## Flujo

1. **Recibir el prompt.** Si el usuario pasó un argumento al invocar el skill, ese es el prompt a
   optimizar. Si no, pedirlo: "Pégame la instrucción que quieres optimizar".
2. **Obtener el checklist vigente.** Hacer WebFetch de la página oficial de Anthropic:
   `https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices`
   y derivar de ahí los checks aplicables a un prompt de usuario. Si el fetch falla (sin red, página
   caída), usar el checklist de respaldo de abajo y decirlo explícitamente en el chat.
3. **Evaluar check por check, como trabajo interno.** La evaluación, sus resultados y el
   razonamiento no se muestran en el chat.
4. **Resolver los que no cumplen**, distinguiendo dos casos:
   - **Falta información** (no hay criterio de éxito, no se sabe qué formato debe tener el
     resultado, falta contexto de negocio): **preguntar al usuario, una pregunta a la vez, jamás
     inferir ni rellenar por cuenta propia.** Formular la pregunta en el lenguaje del usuario
     (columna "Check" de la tabla de respaldo), nunca con el vocabulario de la documentación, y
     ofrecer alternativas concretas para elegir cuando se pueda.
   - **Falla solo de forma** (las partes están mezcladas, el pedido va antes que el material, dice
     qué evitar en vez de qué hacer): corregir directo, sin preguntar ni narrar el cambio. No
     requiere información nueva.
5. **Entregar únicamente el prompt optimizado**, en un bloque listo para copiar. Sin tabla de
   verificación, sin análisis de los checks y sin explicaciones sobre su estructura: el usuario
   copia el bloque y lo usa. La única nota admitida es el aviso del paso 2 cuando se usó el
   checklist de respaldo.

## Checklist de respaldo

Destilado de la página oficial. Se usa solo si el WebFetch del paso 2 falla, avisando.

| # | Check (así se le nombra al usuario cuando hay que preguntarle) | Regla detrás |
|---|---|---|
| 1 | La instrucción es clara y directa | Instrucciones explícitas y específicas sobre el resultado esperado; pasos numerados cuando el orden importa |
| 2 | Explica el porqué | Dar el contexto y la motivación detrás de cada instrucción; el modelo generaliza desde la explicación |
| 3 | El material va antes que el pedido | Documentos y contenido de referencia arriba, la petición al final |
| 4 | Tiene las partes separadas | Estructurar instrucciones, contexto y entrada en secciones marcadas (etiquetas tipo `<instrucciones>`, `<contexto>`) |
| 5 | Define quién responde | Asignar un rol en una frase ("Eres un ...") |
| 6 | Dice qué hacer, no qué evitar | Formular el formato y las restricciones en positivo |
| 7 | Pide acción, no sugerencia | Verbos de acción explícitos ("crea", "implementa"), no "¿podrías sugerir...?" |
| 8 | Se sabe cuándo quedó bien | Criterios de éxito observables; ejemplos del resultado esperado cuando el formato importa |

## Al terminar

Pásale esta prueba al prompt optimizado: ¿una persona sin ningún contexto de la conversación
entendería exactamente qué hacer y cómo saber si el resultado quedó bien? Si la respuesta es no,
seguir puliendo antes de entregarlo.
