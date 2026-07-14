# Canonical Prompts

Three prompts sent in strict sequence for each stage. Send them verbatim (only substitute `{stage}`); they are calibrated to point the sub-agent at the documentation instead of loading context into it.

The canonical prompts are in Spanish (the language they were authored and tested in). An English equivalent follows each one for projects documented in English; keep whichever language matches the project's documentation.

## Prompt 1 — Project Familiarization

**When:** the first message to every new sub-agent, always, even when the stage looks simple.

```txt
Comienza revisando la documentación del proyecto para familiarizarte con el código, la arquitectura y el diseño general.

Tu primera tarea es realizar una auditoría del proyecto y analizar su estructura interna, capacidades actuales y detalles de implementación. Una vez completada la revisión, proporciona un resumen conciso pero exhaustivo de tus hallazgos, incluyendo patrones arquitectónicos relevantes, dependencias o áreas que puedan requerir atención.

Los documentos más importantes son README.md y AGENTS.md. Cualquier documentación adicional relevante o convenciones se referencia desde esos archivos.
```

English equivalent:

```txt
Start by reviewing the project documentation to familiarize yourself with the code, the architecture, and the overall design.

Your first task is to audit the project and analyze its internal structure, current capabilities, and implementation details. Once the review is complete, provide a concise but exhaustive summary of your findings, including relevant architectural patterns, dependencies, or areas that may need attention.

The most important documents are README.md and AGENTS.md. Any additional relevant documentation or conventions are referenced from those files.
```

**Expect back:** a summary showing the sub-agent understands the architecture, the conventions, and **where** to find information (not that it read everything).

**Warning signs** (do not send Prompt 2 yet; correct with targeted instructions):

- The summary contradicts the documentation or invents nonexistent structure.
- It does not mention the project's key documents (a sign it did not read them).
- It started modifying code instead of auditing.

## Prompt 2 — Stage Context

**When:** after a satisfactory Prompt 1. `{stage}` is the **oldest pending** stage/phase in the plan (e.g., stage 0 before stage 1). If the name is ambiguous across documents, specify which plan and which document.

```txt
Excelente. Antes de continuar, proporciona un resumen conciso del plan de implementación de la {stage}, incluyendo los objetivos principales, los cambios arquitectónicos y los componentes que se introducirán, refactorizarán o modificarán.

Incluye también cualquier información faltante, decisiones sin resolver, dependencias o datos adicionales que necesites de mi parte antes de que la implementación pueda avanzar de forma eficiente, pero solo si es necesario.
```

English equivalent:

```txt
Excellent. Before continuing, provide a concise summary of the implementation plan for {stage}, including the main objectives, the architectural changes, and the components that will be introduced, refactored, or modified.

Also include any missing information, unresolved decisions, dependencies, or additional data you need from me before implementation can proceed efficiently, but only if necessary.
```

**Expect back:** a faithful summary of the stage plan plus a (ideally empty or short) list of questions and unresolved decisions.

**With the returned questions, the orchestrator must:**

1. Resolve autonomously whatever it can (documentation, technical judgment, targeted research).
2. Escalate to the user only decisions that belong to them (product, scope, preferences) or what the orchestrator genuinely cannot resolve.
3. Translate the user's answer into **concrete, self-contained instructions** for the sub-agent before continuing.

**Warning signs:** the summary does not match the documented plan; the sub-agent confused the stage (e.g., between nested plans); it proposes redesigning the plan instead of summarizing it.

## Prompt 3 — Stage Execution

**When:** only once every question from Prompt 2 is resolved and communicated.

```txt
Puedes proceder con el plan aprobado para la {stage} actual.

Mantén la implementación simple, legible y fácil de refactorizar. Prefiere código breve y directo por encima de complejidad innecesaria, sin dejar de garantizar que sea rápido, confiable, robusto y cohesivo y consistente con el resto del proyecto y sus estrategias de diseño.

Documenta cada cambio significativo e hito en `PLAN.md`. Mantén `README.md` actualizado siempre que el código cambie de una forma que afecte la configuración, el uso, el comportamiento u otras expectativas documentadas.

Actualiza `AGENTS.md` solo cuando sea estrictamente necesario, y únicamente si no hacerlo dejaría a futuros agentes sin contexto esencial.

Además, al final propón un mensaje de commit breve que acompañaría un commit para esta fase. Puedes leer los anteriores para inspirarte en el formato.
```

English equivalent:

```txt
You may proceed with the approved plan for the current {stage}.

Keep the implementation simple, readable, and easy to refactor. Prefer short, direct code over unnecessary complexity, while still guaranteeing it is fast, reliable, robust, and cohesive and consistent with the rest of the project and its design strategies.

Document every significant change and milestone in `PLAN.md`. Keep `README.md` up to date whenever the code changes in a way that affects configuration, usage, behavior, or other documented expectations.

Update `AGENTS.md` only when strictly necessary, and only if not doing so would leave future agents without essential context.

Finally, propose a brief commit message that would accompany a commit for this phase. You may read previous ones for inspiration on the format.
```

**Expect back:** a condensed summary of what was implemented, evidence of real verification (tests run, end-to-end runs), updated documentation, and the proposed commit message.

**Warning signs** (request correction before closing the stage):

- Reports success without verification evidence.
- Did not update `PLAN.md`/`README.md` when the change warranted it.
- Committed or pushed on its own (forbidden: only the message is proposed).
- Went beyond the stage's scope.
