# AGENT_TASK_BOARD — Civilization Evolutions

> **Tablero de tareas del desarrollo multi-agente.** Organizado por **hito**. Cada tarea trae: ID, Agente (workstream), Descripción, Dependencias, Prioridad, Archivos permitidos, Archivos restringidos, Entregables, Criterios de aceptación, Estado, Bloqueos. Las listas completas de archivos por workstream están en [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md); los contratos en [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md).
>
> Versión 1.0 · 2026-11-04 · Estado: PROPUESTA (ninguna tarea arranca hasta aprobación).
> Prioridad: **P0** (crítico/bloqueante) → **P3** (diferible). Estado: `todo` · `in-progress` · `blocked` · `review` · `done`.
> Origen: se mapea al backlog real del repo UE (`Docs/BACKLOG.md`, P0–P7/DT) y al [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) (E0–E6).

## Hitos

| Hito | Nombre | Objetivo | Depende de |
|---|---|---|---|
| **M0** | Preparación para paralelizar | Contratos publicados + SimCore partido por dominio + Git/LFS + mocks | — |
| **M1** | Base jugable coherente | Cerrar el loop: un camino de colocación, obra real, recursos coherentes | M0 |
| **M2** | Población en SimCore (Task F) | Invertir autoridad Modelo A: SimCore conduce pop; paneles reales | M0, M1 |
| **M3** | Mundo y contenido data-driven | Biomas/semilla + migrar contenido a datos | M0 |
| **M4** | UI real + instrumentación | Sustituir mock data; eventos→UI; herramientas de debug | M1, M2 |
| **∞** | Transversales continuos | Arte y QA a lo largo de todos los hitos | M0 |

> **Regla de arranque (brief §7):** no se arrancan todos los sistemas antes de definir sus dependencias. Las **tareas iniciales que pueden empezar ya** (tras aprobar M0) están marcadas **[ARRANQUE]**.

---

## M0 — Preparación para paralelizar (lo lidera WS-TD)

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **M0-TD-01** | WS-TD | Publicar y versionar los contratos ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)) y el mapa de propiedad; ratificar Modelo A y DR-01..12 como base | — | P0 | review | aprobación del responsable |
| **M0-TD-02** | WS-TD | **Refactor M0:** partir `SimCore/Simulation.cpp` y `World.cpp` en pasos de dominio (Production/Logistics/Trade→ECON, Population→POP, Construction→BUILD, orquestador/clock/eventos/save-load→CORE) sin agregar abstracciones vacías | M0-TD-01 | P0 | todo | — |
| **M0-TD-03** | WS-TD | Git: ramas por workstream + worktrees; crear `.gitattributes` con LFS para `*.uasset`/`*.umap` (**requiere aprobación**: es cambio de config) y migrar binarios | M0-TD-01 | P0 | todo | aprobación config |
| **M0-CORE-01** | WS-CORE | Commitear el trabajo post-HANDOFF sin commitear (RISK-12) y pushear `origin/main` del repo UE; dejar árbol limpio antes de ramificar | — | P0 | todo | — |
| **M0-QA-01** | WS-QA | Reinstalar/verificar CMake; correr `simcore_tests` (baseline verde) y documentar cómo correrlos en CI | — | P0 | todo | — |

**Detalle (archivos / entregables / aceptación):**
- **M0-TD-02** · *Permitidos:* `SimCore/{Simulation,World}.{h,cpp}` (coordinando con CORE/ECON/BUILD/POP), nuevos archivos de paso por dominio · *Restringidos:* presentación UE · *Entregable:* SimCore con un archivo por dominio y `simcore_tests` verde · *Aceptación:* cada workstream puede editar su paso sin tocar el de otro; tests pasan igual que antes (sin cambio de comportamiento).
- **M0-CORE-01** · *Permitidos:* repo UE completo (es un commit de estado, coordinado con TD) · *Entregable:* `git status` limpio, `origin/main` al día · *Aceptación:* no hay cambios sin commitear antes de abrir ramas.
- **M0-TD-03** · *Entregable:* `GIT_WORKFLOW.md` operativo + `.gitattributes` + ramas base · *Aceptación:* un `.uasset` nuevo entra por LFS; dos worktrees no se pisan.

---

## M1 — Base jugable coherente (cerrar el loop)

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **M1-BUILD-01** | WS-BUILD | Unificar los **dos caminos de colocación** (§6/§9 de contratos): colocación vía `tryPlaceBuilding` + reactivar `CivViewManager::SyncBuildings`; retirar el *spawn* directo duplicado | M0-TD-02 | P0 | todo | M0 |
| **M1-BUILD-02** | WS-BUILD | **Obra real**: progreso + trabajadores llevando materiales + se detiene si faltan (SimCore ya lo modela; reflejarlo en UE; hoy aparece instantáneo — backlog P1.9) | M1-BUILD-01 | P1 | todo | M1-BUILD-01 |
| **M1-ECON-01** | WS-ECON | Verificar/mantener el **stock único** (Modelo A) tras M0; que recolección↔producción↔costo↔consumo usen `World::colony()` sin regresar al doble inventario | M0-TD-02 | P0 | todo | M0 |
| **M1-ECON-02** | WS-ECON | **Regeneración de renovables** (agua rápido, frutal anual ligado al reloj) en el modelo correcto; depósitos/almacenes como destino de acarreo (P1.7/P1.8) | M1-ECON-01 | P1 | todo | — |
| **M1-ECON-03** | WS-ECON | Clay en recetas/construcción (cerámica, `masonry`) — hoy solo se recolecta (P1.10) | M1-ECON-01 | P2 | todo | — |
| **M1-WORLD-01** | WS-WORLD | Dueño del acarreo de nodos: estabilizar `CivResourceNode` (water/clay nuevos) y la entrega al town hall contra el stock único | M1-ECON-01 | P1 | todo | — |

**Detalle:**
- **M1-BUILD-01** · *Permitidos:* `View/CivBuildingActor.*`, `View/CivViewManager.*` (`SyncBuildings`, con WORLD), `Core/CivPlayerController.*` (coordinado con UI), paso de construcción en SimCore · *Restringidos:* `Inventory`/recetas de ECON, `CivText` salvo labels · *Entregable:* un solo camino de colocación con id de SimCore · *Aceptación:* colocar crea un `buildingId`, descuenta del stock único, y el actor 3D se reconcilia por `GetBuildingViews()`; sin doble modelo de edificio.
- **M1-ECON-02** · *Permitidos:* paso de economía en SimCore, `Inventory.*`, `data/*.json` · *Entregable:* renovables que vuelven; almacén como destino · *Aceptación:* un frutal agotado se repone en ~1 año sim; el acarreo llega a un almacén además del town hall; sin segundo stock.
- **M1-WORLD-01** · *Permitidos:* `Units/CivResourceNode.*`, `View/CivViewManager.*` (spawns) · *Aceptación:* los 5 tipos de nodo entregan al inventario único; renovables no se destruyen.

---

## M2 — Población en SimCore (Task F)

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **M2-POP-01** | WS-POP | **Invertir autoridad (Modelo A→SimCore):** mover hambre/salud/muerte de `CivUnit` a SimCore (`nutritionPerCitizenPerDay>0`), `CivUnit` pasa a reflejar; coordinar con CORE/ECON | M1-ECON-01 | P0 | blocked | decisión de contrato (WS-TD) |
| **M2-POP-02** | WS-POP | Envejecimiento con el reloj + natalidad/mortalidad básica; consumo de comida por persona vía inventario único | M2-POP-01 | P1 | todo | M2-POP-01 |
| **M2-POP-03** | WS-POP | Familias y hogares como entidades (familia≠hogar); vivienda con capacidad (DR-07/SHARED_DATA_MODEL) | M2-POP-02 | P2 | todo | — |
| **M2-UI-01** | WS-UI | Paneles de info reales (individuo: nombre/edad/oficio/necesidades/salud; edificio: producción/trabajadores/almacén) leyendo contratos | M2-POP-01, M1-BUILD-01 | P1 | todo | — |
| **M2-UI-02** | WS-UI | Asignación de trabajadores a edificios desde la UI (clic settler → asignar) | M1-BUILD-01 | P2 | todo | — |

**Detalle:**
- **M2-POP-01** · *Permitidos:* paso de población en SimCore, `Units/CivUnit.*` (quitar lógica de sim, dejar reflejo) · *Restringidos:* `Inventory`/`GameDatabase` (ECON), `SimulationClock` (CORE) · *Entregable:* SimCore conduce hambre/salud/muerte; UE refleja · *Aceptación:* un `Citizen` envejece y puede morir de hambre **sin** doble descuento del stock; población agregada coherente con el total; save/load conserva identidad. · **Bloqueo:** es cambio de contrato (§7) → WS-TD lo valida antes.
- **M2-UI-01** · *Permitidos:* `UI/CivMainHUD.*`, `Core/CivHUD.*`, `CivText.*` (labels) · *Restringidos:* cualquier `.cpp` de SimCore · *Aceptación:* los paneles muestran datos reales del subsystem (no mock); la UI no calcula estado autoritativo.

---

## M3 — Mundo y contenido data-driven

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **M3-WORLD-01** | WS-WORLD | **[ARRANQUE]** Semilla configurable expuesta en UI + mundo reproducible (RISK-13); API de *terrain queries* (`HeightAt` ya existe; añadir `IsWalkable`/`BiomeAt`) | M0 | P1 | todo | — |
| **M3-WORLD-02** | WS-WORLD | Biomas y tipos de suelo; ríos por topografía; distribución de recursos por geología (no puro azar) — C-02/C-03 pendientes de confirmar | M3-WORLD-01 | P2 | blocked | C-02/C-03 (AUDIT) |
| **M3-ECON-01** | WS-ECON | **Data-driven:** cargar realmente `data/*.json` (o Data Tables/Assets) en vez de hardcode en `DefaultContent.cpp` (RISK-04, DT.27) | M0-TD-02 | P2 | todo | — |

**Detalle:**
- **M3-WORLD-01** · *Permitidos:* `View/CivTerrain.*`, geo/mundo en SimCore, `UI/*` solo para exponer la semilla (coordinado con UI) · *Aceptación:* misma semilla ⇒ mismo mundo; *terrain queries* responden por contrato (§8).
- **M3-ECON-01** · *Permitidos:* `GameDatabase.*`, `DefaultContent.*`, `data/*.json`, posible parser sin dependencias pesadas · *Aceptación:* agregar un recurso/receta es editar datos, no recompilar lógica; `simcore_tests` verde. · *Nota:* respetar "no dependencias externas innecesarias" (plan técnico §16).

---

## M4 — UI real + instrumentación

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **M4-UI-01** | WS-UI | Sustituir **mock data** de `CivMainHUD` (Housing/Wellbeing/Weather/notificaciones) por datos reales; conectar eventos SimCore→UI ("Stage 4") | M2, §2-bridge | P1 | todo | puente de eventos (CORE) |
| **M4-CORE-01** | WS-CORE | Puente de eventos SimCore→UE: exponer `EventBus` al subsystem para notificaciones reales (hoy la UE hace polling) | M0-TD-02 | P1 | todo | — |
| **M4-UI-02** | WS-UI | Minimapa + notificaciones configurables + tutorial contextual básico (P6) | M4-UI-01 | P3 | todo | — |
| **M4-QA-01** | WS-QA | Herramientas de depuración determinista (RISK-19): reproducir un bug con semilla + save; logging de eventos | M0-QA-01 | P1 | todo | — |

---

## ∞ — Transversales continuos (Arte y QA)

| ID | Agente | Descripción | Deps | Prio | Estado | Bloqueos |
|---|---|---|---|---|---|---|
| **ART-01** | WS-ART | **[ARRANQUE]** Import headless de `ArtStaging/`→`Content/`; reemplazar placeholders (settlers KayKit escala/yaw, animales Quaternius, nat/edificios MegaKit) — solo asset, sin lógica (P0) | M0-TD-03 (LFS) | P1 | todo | LFS config |
| **ART-02** | WS-ART | Materiales PBR reales (césped/roca/corteza/arcilla) + agua real (plugin UE Water o material) en vez del disco azul | ART-01 | P2 | todo | — |
| **QA-01** | WS-QA | **[ARRANQUE]** Ampliar la suite: tests de colocación/costos/recetas/consumo/asignación/pausa/velocidad/producción por tiempo (DT.28) | M0-QA-01 | P1 | todo | — |
| **QA-02** | WS-QA | Medición de rendimiento (RISK-03/07): costo de N `CivUnit`/nodos; definir umbral de agregación (DR-06) | M2 | P2 | todo | — |
| **UI-ART-01** | WS-UI+WS-ART | Iconos en la barra de recursos; cursor custom; tooltip extendido a animales/edificios (P0.4/P0.5) | — | P3 | todo | coordinación UI↔ART |

---

## Tareas de arranque inmediato (brief §7) — tras aprobar M0

Priorizadas porque **no** dependen de sistemas ajenos aún por definir (operan con contratos estables o mocks):

1. **M0-TD-01/02/03 + M0-CORE-01 + M0-QA-01** — fundaciones (contratos, refactor, git, baseline de tests). *Prerrequisito de casi todo.*
2. **M3-WORLD-01** — generación/semilla reproducible (sistema que opera independiente temprano).
3. **M2-UI-01 / M4-UI-01 con mock data** — la UI ya avanza contra mocks del contrato §10 (no bloquea a nadie).
4. **ART-01/02** — pipeline de assets (visual, sin tocar lógica).
5. **QA-01** — ampliar tests sobre lo que ya existe.

**No arrancar aún:** M2-POP-01 (bloqueado por decisión de contrato Modelo A), M3-WORLD-02 (bloqueado por C-02/C-03), y todo sistema de dominio avanzado (cultura/política/diplomacia/IA/guerra — E5+ del roadmap).

## Convenciones del tablero

- **Un dueño por tarea**; si una tarea toca un archivo 🤝 compartido, se aplica el protocolo de [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md).
- **`blocked`** siempre nombra el bloqueo (decisión, dependencia, aprobación).
- Al cerrar una tarea: verde + tests + PR a integración (ver [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md)); WS-TD mueve a `done` y actualiza `IMPLEMENTATION_ALIGNMENT.md`.
- Cambiar prioridad o dependencia es decisión de WS-TD (registrada aquí).
