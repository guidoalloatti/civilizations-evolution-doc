# AGENT_OWNERSHIP_MAP — Civilization Evolutions

> **Mapa de propiedad de archivos y carpetas.** Cada ruta tiene **un** workstream dueño (quien edita). Los demás **leen**. Evita escrituras simultáneas y conflictos. Complementa [AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md) (quién es cada workstream) y [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) (qué expone cada dueño).
>
> Versión 1.0 · 2026-11-04 · Estado: PROPUESTA.
> Rutas `civilizations-evolution/...` → **repo UE**. Rutas de raíz → **repo de docs**. (El repo UE está en el `.gitignore` del repo de docs.)

## Leyenda

- **Dueño:** único workstream que **edita** el archivo.
- **Lectores:** quienes dependen de él (vía contrato); no lo editan.
- 🔒 **Restringido global:** nadie lo edita sin aprobación del Director Técnico.
- 🤝 **Compartido (protocolo):** varios podrían necesitar tocarlo → se aplica el **protocolo de archivo compartido** (abajo). Marcado aquí como 🤝.
- 🎨 **Binario `.uasset`:** no fusionable textualmente → **protocolo binario** (abajo).

---

## A. Repo de documentación (raíz)

| Ruta | Dueño | Lectores | Nota |
|---|---|---|---|
| `AGENT_ORGANIZATION.md`, `AGENT_OWNERSHIP_MAP.md`, `SYSTEM_CONTRACTS.md`, `AGENT_TASK_BOARD.md`, `GIT_WORKFLOW.md`, `INTEGRATION_STRATEGY.md` | **WS-TD** | todos | Gobernanza. Solo TD edita; otros proponen por PR/mensaje. |
| `CLAUDE.md`, `DOCUMENTATION_INDEX.md`, `DOCUMENTATION_READING_GUIDE.md` | **WS-TD** | todos | Índice y entrada. |
| `DESIGN_DECISION_REGISTER.md`, `AUDIT_REPORT.md`, `IMPLEMENTATION_RISK_REGISTER.md` | **WS-TD** | todos | Decisiones/riesgos. Un workstream que toma una decisión la **propone**; TD la registra. |
| `SHARED_DATA_MODEL.md`, `SYSTEM_DEPENDENCY_MAP.md`, `SYSTEM_ARCHITECTURE_MAP.md`, `DEVELOPMENT_ROADMAP.md`, `SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md`, `CROSS_SYSTEM_VALIDATION_SCENARIOS.md`, `DESIGN_CONSISTENCY_AUDIT.md`, `HISTORICAL_EVOLUTION_FRAMEWORK.md` | **WS-TD** | todos | Blueprint Phase 2 (derivado de GDD). |
| `IMPLEMENTATION_ALIGNMENT.md` | **WS-TD** (con insumo de WS-QA) | todos | Foto código↔diseño; actualizar tras cada hito. |
| `docs/00-project/GDD_MAESTRO.md`, `docs/01-systems/*`, `docs/02-technical/*`, `docs/README.md` | 🔒 **GDD — autoridad de diseño** | todos | **No se editan** por decisión de desarrollo; cambios de diseño son decisión del responsable (DR-08). |
| `BACKUP_PRE_AUDIT_2026-11-03/**` | 🔒 **Inmutable** | — | Respaldo. Nunca se edita. |

## B. Repo UE — SimCore (`civilizations-evolution/SimCore/`)

> SimCore es chico y acoplado. Hasta la refactorización **M0** (partir `Simulation`/`World` en módulos de dominio), varios archivos son **compartidos** y se editan con protocolo. Post-M0, la columna "Dueño (post-M0)" aplica.

| Ruta | Dueño (hoy) | Dueño (post-M0) | Lectores | Nota |
|---|---|---|---|---|
| `include/simcore/Types.h`, `src/Types.cpp` | **WS-CORE** | WS-CORE | todos | IDs, enums, nombres. 🤝 añadir un enum nuevo avisa a TD. |
| `include/simcore/SimulationClock.h`, `src/SimulationClock.cpp` | **WS-CORE** | WS-CORE | todos | Reloj. |
| `include/simcore/EventBus.h` | **WS-CORE** | WS-CORE | todos | Bus + `EventType`. Añadir evento = cambio de contrato. |
| `include/simcore/World.h`, `src/World.cpp` | 🤝 **WS-CORE** (coordina) | WS-CORE (contenedor) | ECON/BUILD/POP | Contenedor raíz + `colony()` + alta/lookup de instancias + estado de tech. **Hotspot**: lo tocan varios dominios → protocolo compartido. |
| `include/simcore/Simulation.h`, `src/Simulation.cpp` | 🤝 **WS-CORE** (coordina) | **partir** en pasos de dominio | ECON/BUILD/POP | Orquesta producción/logística/población/research/trade + save/load. **Hotspot #1**. M0 lo divide (ver abajo). |
| `include/simcore/Inventory.h`, `src/Inventory.cpp` | **WS-ECON** | WS-ECON | BUILD/POP/UI | Inventario. |
| `include/simcore/GameDatabase.h`, `src/GameDatabase.cpp` | **WS-ECON** | WS-ECON (+BUILD para `BuildingDef`) | todos | Catálogo. `BuildingDef` lo consume BUILD; ECON dueño del contenedor. 🤝 |
| `include/simcore/DefaultContent.h`, `src/DefaultContent.cpp` | **WS-ECON** | WS-ECON/BUILD | CORE (escenarios) | Catálogo + escenarios (`makeColonyStart`, `makeDefaultScenario`). 🤝 BUILD añade defs de edificio aquí → coordina con ECON. |
| `data/resources.json`, `data/recipes.json` | **WS-ECON** | todos | — | Datos. |
| `data/buildings.json` | **WS-BUILD** | ECON | — | Datos de edificios. |
| `app/main.cpp` | **WS-QA** (con CORE) | — | Runner de consola / escenarios de prueba. |
| `tests/**` (`test_main.cpp`, `simple_test.h`) | **WS-QA** | todos | Suite. QA agrega tests por sistema. |
| `CMakeLists.txt` | 🔒 **WS-TD** | — | Build de SimCore. |

### Refactor M0 (habilita el paralelismo) — propuesto, lo lidera WS-TD

Partir `Simulation.cpp`/`World.cpp` (hoy *hotspots* compartidos) en pasos de dominio con límites claros, para que cada workstream tenga **su** archivo:
- `ProductionStep`/`LogisticsStep`/`TradeStep` → **WS-ECON**
- `PopulationStep` → **WS-POP**
- `ConstructionStep`/evolución → **WS-BUILD**
- Orquestador + reloj + eventos + save/load + registro de pasos → **WS-CORE**

Hasta que M0 esté integrado, **toda** edición de `Simulation.cpp`/`World.cpp` va por el protocolo de archivo compartido (serializada, un workstream a la vez, coordinada por TD). Esto respeta "no crear abstracciones vacías prematuras": M0 **no** agrega capas nuevas, solo separa responsabilidades ya existentes.

## C. Repo UE — Capa Unreal (`civilizations-evolution/Source/CivilizationSimulator/`)

| Ruta | Dueño | Lectores | Nota |
|---|---|---|---|
| `Simulation/CivSimSubsystem.{h,cpp}` | 🤝 **WS-CORE** | **todos** (UE) | **Único puente SimCore↔UE.** Es el contrato de acceso UI. Un consumidor que necesita un getter nuevo lo **pide a CORE**. |
| `SimCoreUnity.cpp` | 🤝 **WS-CORE** (con dueños SimCore) | — | Compila SimCore dentro del módulo. Se edita al agregar/quitar un `.cpp` de SimCore. |
| `Core/CivGameModeBase.{h,cpp}` | **WS-UI** (bootstrap de presentación) | — | Spawnea ViewManager + crea el HUD UMG. |
| `Core/CivPlayerController.{h,cpp}` | 🤝 **WS-UI** (dueño) con interés de BUILD | — | Input/selección/órdenes/menú de construcción/time controls. **Hotspot**: cruza input→presentación→sim. BUILD coordina cambios de colocación aquí. |
| `Core/CivHUD.{h,cpp}` | **WS-UI** | — | Canvas: caja de selección, menú, panel info, tooltip, debug F1. |
| `Core/CivText.{h,cpp}` (`CivLoc`) | 🤝 **WS-UI** (dueño) | todos (añaden labels) | **i18n — hotspot de merge.** Todo string de UI/edificio/recurso/tech vive aquí. Protocolo compartido: añadir una clave es un cambio chico y aislado; agrupar por commits pequeños. |
| `UI/CivMainHUD.{h,cpp}`, `UI/CivUIStyle.h` | **WS-UI** | — | HUD UMG principal + tokens de estilo. Sustituir mock data (Stage 4). |
| `Camera/CivCameraPawn.{h,cpp}` | **WS-UI** | — | Cámara RTS. **Boundary más limpio**: sin SimCore. |
| `View/CivViewManager.{h,cpp}` | 🤝 **WS-WORLD** (entorno) con BUILD (`SyncBuildings`) | — | Orquesta entorno 3D + spawns + re-tinte estacional. `SyncBuildings` (dormido) lo reactiva BUILD. Coordinar WORLD↔BUILD. |
| `View/CivTerrain.{h,cpp}` | **WS-WORLD** | — | Terreno procedural + queries + estaciones. |
| `View/CivBuildingActor.{h,cpp}` | 🤝 **WS-BUILD** (lógica/estado) con ART (mesh) | — | Actor de edificio. BUILD dueño del comportamiento; ART solo cambia mesh/material vía propiedades expuestas. |
| `Units/CivUnit.{h,cpp}` | 🤝 **WS-POP** (estado/sim) con ART (mesh/anim) | — | Settler. POP dueño del estado/máquina; ART solo visual. **Hoy escribe al stock único** (hambre/acarreo) — Modelo A. |
| `Units/CivAnimal.{h,cpp}` | 🤝 **WS-WORLD**/**WS-ECON** (fauna/caza) con ART | — | Animal salvaje. Decidir dueño en el tablero según la tarea de caza (propuesto: WS-ECON porque alimenta recursos). |
| `Units/CivResourceNode.{h,cpp}` | **WS-WORLD** (con ECON para el id de recurso) | — | Nodo recolectable. Núcleo del "sistema de recursos del mapa". |
| `CivilizationSimulator.{cpp,h}` | 🔒 **WS-TD** | — | Módulo primario. Trivial. |
| `CivilizationSimulator.Build.cs`, `*.Target.cs` | 🔒 **WS-TD** | — | **Config de build.** Añadir dependencia de módulo pasa por TD. |

## D. Repo UE — Config, Content, Docs

| Ruta | Dueño | Nota |
|---|---|---|
| `Config/DefaultInput.ini` | 🤝 **WS-UI** (dueño) | **Hotspot de merge.** Todo bind de input nuevo lo edita UI; otros workstreams piden el bind. |
| `Config/DefaultEngine.ini`, `Config/DefaultGame.ini` | 🔒 **WS-TD** | Config de proyecto/motor. |
| `CivilizationSimulator.uproject` | 🔒 **WS-TD** | Plugins/módulos. |
| `Content/Chars/**` 🎨 | **WS-ART** | Personaje KayKit + anims. Binario. |
| `Content/PBR/**` 🎨 | **WS-ART** | Materiales/texturas. Binario. |
| `Content/Maps/**` 🎨 (nuevo) | **WS-WORLD** (si se crea un `.umap`) | Hoy **no hay `.umap`** (entorno procedural). Si se introduce, dueño único. |
| `Content/UI/**` 🎨 (nuevo) | **WS-UI** | Hoy el HUD es 100% código; si se agregan `.uasset` de UI, dueño único. |
| `Content/Developers/**` | por-usuario | Carpeta de desarrollo personal; fuera de integración. |
| `ArtStaging/**` | **WS-ART** | Crudos pesados. **Fuera de git** (`.gitignore`). |
| `Docs/ASSETS.md`, `Docs/UI_STYLE_GUIDE.md` | **WS-ART** / **WS-UI** | Pipeline de assets / guía de estilo. |
| `Docs/BACKLOG.md` | 🤝 **WS-TD** (espejado en el tablero) | Backlog histórico del repo UE; el tablero oficial es `AGENT_TASK_BOARD.md`. |
| `Docs/00_TECHNICAL.md` (= PLAN_TECNICO) | 🔒 autoridad técnica | Copia del plan; no editar sin aprobación. |

---

## Protocolo de archivo compartido (🤝)

Para los *hotspots* que varios workstreams podrían necesitar (`Simulation.cpp`, `World.cpp` pre-M0, `CivText.cpp`, `DefaultInput.ini`, `CivSimSubsystem.*`, `GameDatabase.cpp`, `CivPlayerController.cpp`):

1. **Un escritor a la vez.** El tablero ([AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md)) marca quién tiene "tomado" el archivo para la tarea activa.
2. **Cambios chicos y atómicos**, commiteados rápido en verde, para minimizar la ventana de conflicto.
3. **Pedir, no invadir:** si necesitás algo en un archivo de otro (un getter en el subsystem, una clave en `CivText`, un bind en `DefaultInput`), lo pedís al dueño o abrís un cambio mínimo coordinado con él; no reescribís su archivo.
4. **Preferir extensión aditiva:** agregar un caso a un `switch`/diccionario (nueva clave de `CivLoc`, nuevo `case` de categoría) rara vez choca; **reordenar/renombrar** sí → avisar.
5. **M0 primero** donde aplique: una vez partidos `Simulation`/`World`, la mayoría de estos dejan de ser compartidos.

## Protocolo de binarios `.uasset` (🎨)

Los `.uasset` **no se fusionan** textualmente (RISK-12). Hay ~49 hoy, todo en `Content/Chars` y `Content/PBR`; **no hay `.umap` ni `.uasset` de UI** (el nivel y el HUD son procedurales/código) → hoy **no hay un asset compartido grande sobre el que chocar**. Reglas:

1. **Partición por subcarpeta:** `Chars/` y `PBR/` son de **WS-ART**; futuros `Maps/` de WORLD, `UI/` de UI. Un `.uasset` = un dueño por tarea.
2. **Nunca dos agentes editan el mismo `.uasset` a la vez.** El tablero asigna el asset a una sola tarea.
3. **Configurar Git LFS antes de sumar binarios pesados.** Hoy LFS está **instalado pero no configurado** (no hay `.gitattributes`). WS-TD debe crear `.gitattributes` con `*.uasset`/`*.umap` en LFS **con aprobación** (es cambio de configuración) y migrar los existentes. Hasta entonces, mantener los binarios chicos y con dueño único.
4. **Bloqueo recomendado:** si se adopta LFS, usar *file locking* de LFS para `.uasset`/`.umap` (`git lfs lock`) en los assets compartidos.
5. **Commits de arte separados** de los de código, para revertir sin tocar lógica.

## Quién puede tocar qué (resumen de una línea)

- **WS-TD:** docs de gobernanza + build/config (🔒). No gameplay.
- **WS-CORE:** reloj/eventos/tipos/World(contenedor)/orquestador/save-load + `CivSimSubsystem` + `SimCoreUnity`.
- **WS-ECON:** `Inventory`/`GameDatabase`/`DefaultContent`/`data/resources|recipes.json` + pasos de economía/producción/logística post-M0.
- **WS-BUILD:** `data/buildings.json` + paso de construcción post-M0 + `CivBuildingActor` (lógica) + `SyncBuildings`.
- **WS-WORLD:** `CivTerrain` + `CivResourceNode` + geo/mundo en SimCore + `CivViewManager` (entorno).
- **WS-POP:** paso de población post-M0 + `CivUnit` (estado).
- **WS-UI:** `CivHUD`/`CivMainHUD`/`CivUIStyle`/`CivText`/`CivPlayerController`/`CivCameraPawn`/`CivGameModeBase` + `DefaultInput.ini`.
- **WS-ART:** `Content/Chars`+`Content/PBR`+`ArtStaging` + mesh/material de actores vía propiedades.
- **WS-QA:** `SimCore/tests` + `app/main.cpp` + tests de automation UE + CI.
