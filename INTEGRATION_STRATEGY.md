# INTEGRATION_STRATEGY — Civilization Evolutions

> **Proceso central de integración.** Cómo el **Director Técnico (WS-TD)** reúne el trabajo de los workstreams manteniendo contratos, dependencias y build verde. Complementa [GIT_WORKFLOW.md](GIT_WORKFLOW.md) (mecánica git) y [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) (qué se valida).
>
> Versión 1.0 · 2026-11-04 · Estado: PROPUESTA.

## Principio

**Un solo integrador: WS-TD.** Nadie promueve a `main` por su cuenta. La integración es el punto donde se protege la coherencia arquitectónica (separación sim/presentación, no duplicación, contratos estables) que el paralelismo pone en riesgo.

## Responsabilidades del Director Técnico

1. **Revisar interfaces.** Que el cambio respete los contratos de [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) y, si los cambia, que sea deliberado, versionado y avisado a los consumidores.
2. **Revisar la implementación.** Que viva en el archivo/dominio correcto ([AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md)); que no meta lógica de simulación en la presentación (RISK-01); que no duplique conceptos (RISK-02); que no agregue abstracciones vacías (RISK-11).
3. **Validar dependencias.** Que las dependencias de la tarea estén `done` y que no rompa a consumidores río abajo.
4. **Integrar ramas** completas en `integration/<repo>` (UE y/o docs).
5. **Compilar y testear** tras cada integración (SimCore + build UE + tests).
6. **Actualizar estado** del tablero ([AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md)) y la foto `IMPLEMENTATION_ALIGNMENT.md`.
7. **Registrar decisiones** de arquitectura en `DESIGN_DECISION_REGISTER.md` y contradicciones en `AUDIT_REPORT.md` (DR-08: la autoridad de diseño es de los GDD; TD no reescribe diseño confirmado, lo documenta y escala).

## Flujo de integración (por solicitud)

```mermaid
flowchart TB
    A["Agente: rama lista (build+tests verdes, al día con integration/)"] --> B{"WS-TD: revisión"}
    B -->|contrato roto / fuera de dominio / duplica| R["Devolver con notas"] --> A
    B -->|ok| C["Merge a integration/<repo>"]
    C --> D["Build SimCore + UE"]
    D -->|falla| R
    D -->|verde| E["Tests (simcore_tests + automation)"]
    E -->|falla| R
    E -->|verde| F["Promover a main + tablero 'done' + alignment"]
```

**Orden por dependencias (bottom-up del grafo, [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md)):** primero fundaciones (CORE: tiempo/eventos/identidad/save-load), luego ECON (recursos/inventario) y WORLD, luego BUILD/POP, y UI/ART por encima. No integrar un consumidor antes que el contrato del que depende.

## Cadencia

- **Integración continua por tarea:** cada tarea verde se integra apenas pasa revisión; no se acumulan ramas largas (reduce conflictos, RISK-12).
- **Checkpoint por hito:** al cerrar M0/M1/M2/M3/M4, WS-TD corre una validación completa (build + toda la suite + un escenario de [CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md)) y actualiza el roadmap/alignment.
- **M0 es el gran desbloqueo:** hasta que la refactorización M0 (partir `Simulation`/`World`) y los contratos estén integrados, el paralelismo real es limitado; WS-TD prioriza M0.

## Gates de aceptación de una integración

Una rama se promueve solo si **todo** se cumple:

- [ ] Build de SimCore verde; `simcore_tests` verde (87+ checks).
- [ ] Build del editor UE verde (editor cerrado).
- [ ] Respeta los archivos permitidos de la tarea; no tocó restringidos.
- [ ] No rompe contratos; si los cambia, están versionados y avisados.
- [ ] Sin duplicación de conceptos compartidos (recurso/entidad/inventario/reloj).
- [ ] Sin lógica de simulación nueva en la capa de presentación (salvo lo ya acordado por Modelo A, en vías de inversión por Task F).
- [ ] Determinismo intacto si tocó reloj/serialización (`save_load_roundtrip`).
- [ ] Tarea con tests nuevos cuando agrega reglas (coordinado con WS-QA).
- [ ] Decisiones/riesgos registrados si corresponde.

## Manejo de cambios de contrato

1. El workstream que necesita cambiar un contrato **lo propone a WS-TD** (no lo cambia en el archivo del dueño por su cuenta).
2. WS-TD evalúa impacto en consumidores (tabla dueño→consumidores de [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)).
3. Se versiona el contrato y se **avisa a todos los consumidores** antes de integrar.
4. Los consumidores adaptan sus mocks/implementaciones; recién entonces se integra el cambio.
5. Caso vivo: **M2-POP-01** (invertir autoridad Modelo A: que SimCore conduzca hambre/salud) toca los contratos §5 y §7 → está `blocked` hasta que WS-TD valide y avise a ECON/UI.

## Resolución de conflictos entre workstreams

- **De archivo (texto):** el agente los resuelve en su rama tras traer `integration/`; si es un 🤝 compartido, con el dueño.
- **De contrato:** WS-TD decide; prevalece el diseño de los GDD y las decisiones confirmadas.
- **De diseño (ambigüedad/choque de GDD):** no se inventa respuesta; WS-TD lo documenta como no resuelto en `AUDIT_REPORT.md` y **pregunta al responsable del proyecto** (regla 1 de `CLAUDE.md`).
- **Binario:** el dueño del asset regenera; WS-TD elige (ver [GIT_WORKFLOW.md](GIT_WORKFLOW.md) §5).

## Registro de decisiones de arquitectura

Cada decisión que tome WS-TD durante la integración (p. ej. "la caza alimenta recursos vía ECON, por eso `CivAnimal` es de ECON") se anota con: fecha, contexto, decisión, alternativas, reversibilidad, y el DR/RISK/C relacionado. Destino: `DESIGN_DECISION_REGISTER.md` (decisiones) / `AUDIT_REPORT.md` (contradicciones abiertas) / `IMPLEMENTATION_RISK_REGISTER.md` (riesgos que cambian de estado).

## Qué protege esta estrategia (resumen)

- **Coherencia > velocidad puntual:** una rama verde que rompe un contrato **no** entra.
- **Fuente única de verdad** por dato (no duplicar).
- **Separación sim/presentación** como criterio de merge, no como buena intención.
- **Trazabilidad:** todo lo integrado tiene task-id, pruebas y, si aplica, decisión registrada.
