# AGENT_ORGANIZATION — Civilization Evolutions

> **Propuesta de organización de desarrollo multi-agente.** Define los *workstreams* (equipos de agentes especializados) que pueden trabajar en paralelo sin pisarse ni duplicar sistemas. Es un documento de **gobernanza de desarrollo**, no de diseño de juego: la autoridad de diseño sigue en los GDD (ver [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)) y en el registro de decisiones ([DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md)).
>
> **Estado: PROPUESTA — pendiente de aprobación.** No se crean worktrees ni se lanzan agentes de implementación hasta que el responsable del proyecto apruebe esta estructura.
> Versión 1.0 · 2026-11-04 · Idioma canónico: español (como el resto de la documentación).

## Documentos hermanos

| Documento | Qué define |
|---|---|
| **AGENT_ORGANIZATION.md** (este) | Los 9 workstreams: alcance, responsabilidades, exclusiones, dependencias, entregables, carpetas permitidas/restringidas, criterios de aceptación. |
| [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md) | Interfaces compartidas (tiempo, eventos, identidad, recursos, inventarios, edificios, población, terreno, colocación, acceso UI). Entradas/salidas por sistema. |
| [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) | Matriz archivo/carpeta → dueño único. Protocolo de archivos compartidos y de `.uasset`. |
| [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md) | Tablero de tareas por hito, con dependencias, prioridad, archivos, criterios y estado. |
| [GIT_WORKFLOW.md](GIT_WORKFLOW.md) | Ramas, worktrees, commits, integración, conflictos, tests, entrega. Dos repos (docs + UE). |
| [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md) | Proceso central de integración a cargo del Director Técnico. |

## Por qué esta organización (y qué la condiciona)

El proyecto ya tiene una base real (no se parte de cero). Antes de paralelizar hay que respetar tres hechos:

1. **Dos repositorios git distintos** (ver [GIT_WORKFLOW.md](GIT_WORKFLOW.md)):
   - **Repo de documentación/diseño** (raíz `Civilization Evolutions/`): GDD, blueprint Phase 2, y **estos documentos de organización**.
   - **Repo del proyecto Unreal** (`civilizations-evolution/`): SimCore + capa UE + assets. Tiene su propio historial (hasta `HANDOFF`), un remoto `origin/main`, **8 commits sin pushear** y **cambios sin commitear** (RISK-12). `civilizations-evolution/` está en el `.gitignore` del repo de docs, así que **no aparece** en los worktrees del repo de docs.
2. **El núcleo (`SimCore/`) es pequeño y está fuertemente acoplado** (8 pares header/impl + `data/*.json`). `Simulation.cpp` y `World.cpp` son *hotspots* que hoy tocan economía, construcción y población a la vez. Paralelizar sobre ellos sin partirlos primero causa conflictos garantizados → ver tarea **M0** del task board y la regla de archivos compartidos en [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md).
3. **Decisiones de diseño ya tomadas** que ningún agente puede revertir en silencio: entre ellas **Modelo A — "el mundo UE es canónico"** (stock único en `World::colony()` dentro de SimCore; la capa UE gobierna hoy hambre/salud/muerte) y DR-01..DR-12 (ver registro). Cambiarlas requiere pasar por el Director Técnico y actualizar `DESIGN_DECISION_REGISTER.md` / `AUDIT_REPORT.md`.

## Principios de la organización (no negociables)

1. **Un dueño por dato y por archivo.** Cada concepto compartido tiene **un** workstream dueño (fuente de verdad); los demás **leen** vía contrato. Cada archivo de código tiene **un** dueño en [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md). Sin propiedad compartida.
2. **Contratos antes que implementación.** Nadie implementa contra un sistema ajeno hasta que su interfaz esté en [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md). Mientras el dueño no publique el contrato real, los consumidores trabajan contra **stubs/mocks** acordados.
3. **Separación simulación/presentación** (plan técnico §4.3, RISK-01). La lógica vive en SimCore; la capa UE presenta y traduce órdenes. Un workstream de presentación **no** mete reglas de simulación.
4. **Sin duplicación** (DC-07/DC-15, RISK-02). No se crea un segundo modelo de un concepto que ya tiene dueño (recursos, entidades, inventario, reloj).
5. **Data-driven donde corresponde** (plan técnico §4.2, RISK-04). El contenido nuevo (recursos/recetas/edificios) se agrega como **datos** (`SimCore/data/*.json` / futuros Data Assets), no hardcodeado.
6. **Verde y commiteado.** Todo cambio compila y pasa tests antes de pedir integración; se commitea en checkpoints verdes (RISK-12).
7. **No romper lo que anda.** No se reescribe gameplay existente sin decisión explícita del Director Técnico; se extiende respetando el estado en disco.

## Modelo de roles

- **Director Técnico (WS-TD):** único integrador. Revisa contratos e implementaciones, valida dependencias, integra ramas, corre compilación/tests, actualiza estado del tablero y registra decisiones. **No** implementa gameplay de dominio.
- **Workstreams de dominio (WS-CORE, WS-WORLD, WS-BUILD, WS-ECON, WS-POP):** implementan su sistema en SimCore y su *wiring* en la capa UE, dentro de sus archivos permitidos.
- **Workstreams transversales de presentación/calidad (WS-UI, WS-ART, WS-QA):** cruzan varios dominios pero con límites estrictos (UI lee, nunca escribe sim; ART toca solo assets/visual; QA agrega tests e instrumentación, no cambia reglas).

---

# Los 9 Workstreams

> Formato: **Alcance · Responsabilidades · Exclusiones · Dependencias · Entregables · Carpetas/archivos permitidos · Archivos restringidos · Criterios de aceptación.**
> Las rutas `civilizations-evolution/...` viven en el **repo UE**; las rutas de raíz, en el **repo de docs**.

## WS-TD — Director Técnico y Arquitectura

- **Alcance:** gobernanza técnica, contratos compartidos, integración y coherencia arquitectónica de todo el proyecto.
- **Responsabilidades:**
  - Mantener [SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md), [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md), [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md), [GIT_WORKFLOW.md](GIT_WORKFLOW.md), [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md).
  - Liderar la refactorización **M0** que parte `SimCore/Simulation.*` y `SimCore/World.*` en módulos de dominio con límites claros (habilita el paralelismo seguro).
  - Revisar e integrar ramas; correr `simcore_tests` y el build UE; resolver conflictos de contrato.
  - Registrar decisiones de arquitectura en `DESIGN_DECISION_REGISTER.md` y contradicciones en `AUDIT_REPORT.md`.
- **Exclusiones:** no implementa features de dominio (economía, población, mundo, etc.); no escribe arte ni UI final.
- **Dependencias:** ninguna lo bloquea; **todos** dependen de que publique y versione los contratos.
- **Entregables:** contratos versionados; mapa de propiedad; tablero actualizado; refactor M0; informes de integración por hito.
- **Permitidos:** todos los documentos de organización (raíz); `SimCore/include/simcore/*.h` (**solo** firmas/contratos, coordinando con dueños); `civilizations-evolution/CivilizationSimulator.Build.cs` y config de build **solo** para integración.
- **Restringidos:** cuerpos de implementación (`.cpp`) de dominios ajenos salvo integración; GDD originales y `BACKUP_PRE_AUDIT_2026-11-03/` (no se editan).
- **Aceptación:** contratos publicados y estables; build verde tras cada integración; cero duplicación de conceptos compartidos; decisiones registradas.

## WS-CORE — Núcleo de Motor y Simulación

- **Alcance:** fundaciones de SimCore (Nivel 0): reloj, bus de eventos, identidad de entidades, orquestación de simulación, serialización (guardar/cargar) y el puente UE↔SimCore.
- **Responsabilidades:** `SimulationClock` (día/mes/año/estación, pausa/velocidades, independiente del FPS, DR-09); `EventBus` (eventos tipados, DC-14); tipos e **identidad persistente** de entidades (`Types.h`, ids estables, DC-15); orquestador `Simulation` (registro y tick de subsistemas tras M0); `World` como contenedor raíz; serialización/versionado (RISK-08/16); `CivSimSubsystem` como único puente.
- **Exclusiones:** no define reglas de economía/construcción/población (solo las coordina); no hace render ni UI.
- **Dependencias:** ninguna (Nivel 0). Es prerequisito de todos.
- **Entregables:** reloj + eventos + identidad estables; API de registro de subsistemas tras M0; save/load con versión de esquema; `CivSimSubsystem` expone lectura de estado para UI.
- **Permitidos:** `SimCore/include/simcore/{Types,SimulationClock,EventBus,World,Simulation}.h` + sus `.cpp`; `SimCore/app/main.cpp`; `civilizations-evolution/Source/CivilizationSimulator/Simulation/CivSimSubsystem.*`.
- **Restringidos:** `Inventory.*`, `GameDatabase.*`, `DefaultContent.*`, `data/*.json` (de ECON/BUILD); actores de presentación (`View/`, `Units/`, `UI/`).
- **Aceptación:** el reloj avanza y se pausa/acelera sin depender de `Tick()`; un sistema reacciona a un evento sin acoplarse al emisor; una entidad conserva su id tras guardar/cargar; `simcore_tests` verde.

## WS-WORLD — Generación del Mundo y Geografía

- **Alcance:** estado del mundo físico: terreno procedural, altura, agua, biomas, semilla reproducible, clima/estaciones (visual y, por fase, su efecto en producción — C-02/C-03).
- **Responsabilidades:** generación determinista por semilla (DR-02); consultas de terreno (altura, caminable, bioma) vía contrato; distribución de recursos naturales por geología (coordinada con ECON para el catálogo); tinte/entorno estacional atado al reloj de WS-CORE.
- **Exclusiones:** no define el catálogo de recursos (lo hace ECON); no coloca edificios (BUILD); no implementa el reloj (CORE).
- **Dependencias:** WS-CORE (reloj, eventos, tipos). Provee a BUILD (colocación válida), ECON/PROD (dónde hay recursos), UI (minimapa).
- **Entregables:** `CivTerrain` con semilla configurable expuesta; API de *terrain queries* del contrato; nodos de recurso natural ubicados por reglas (no puro azar).
- **Permitidos:** `civilizations-evolution/Source/CivilizationSimulator/View/CivTerrain.*`; módulo de geografía/mundo en SimCore (p. ej. `SimCore/.../World`-geo tras M0, **coordinado con CORE**); `Content/Maps/` (nuevo).
- **Restringidos:** `CivBuildingActor.*` (BUILD), lógica de inventario/recetas (ECON), `SimulationClock.*` (CORE).
- **Aceptación:** misma semilla ⇒ mismo mundo (RISK-13); las *terrain queries* responden según contrato; los recursos naturales siguen una distribución geográfica plausible.

## WS-BUILD — Edificios y Construcción

- **Alcance:** representación lógica de edificios (separada del mesh), colocación con validación y costo, obra con progreso/trabajadores/materiales, demolición, ruinas, evolución por tecnología/época.
- **Responsabilidades:** defs de edificio data-driven (`data/buildings.json`); estado de obra (DC-03); costo en recursos (vía inventario de ECON); evolución de edificio (coordinada con Tecnología en ECON/CORE); `CivBuildingActor` como representación (mesh lo pone ART); menú/colocación en coordinación con UI.
- **Exclusiones:** **no** define recetas de producción (ECON); no asigna trabajadores a la economía (ECON/POP); no modela el mesh/material (ART); no implementa el panel de construcción (UI, con datos de BUILD).
- **Dependencias:** WS-CORE (entidades/eventos), WS-ECON (recursos/inventario para costo y almacenamiento), WS-WORLD (colocación válida sobre terreno). DC-03.
- **Entregables:** colocación con feedback válido/ inválido + rotación + confirmación; obra real reflejada en UE (hoy aparece instantáneo — backlog P1.9); ruinas/abandono; API de *construction placement* del contrato.
- **Permitidos:** módulo de construcción en SimCore (defs/estado de obra/evolución tras M0); `SimCore/data/buildings.json`; `civilizations-evolution/Source/CivilizationSimulator/View/CivBuildingActor.*`.
- **Restringidos:** `Inventory.*`/`GameDatabase.cpp` de recetas (ECON los posee; BUILD **lee**), `CivTerrain.*` (WORLD), meshes/materiales (ART), `CivHUD/CivMainHUD` (UI).
- **Aceptación:** colocar un edificio descuenta recursos del **stock único**; no se coloca sin recursos ni fuera de terreno válido; demolición y ruinas coherentes; sin segundo inventario.

## WS-ECON — Economía, Recursos y Producción

- **Alcance:** catálogo único de recursos/materiales, inventarios (stock único `World::colony()`, Modelo A), recetas y cadenas de producción, asignación de trabajadores a la producción, logística física simplificada, comercio.
- **Responsabilidades:** **dueño del catálogo de recursos** (DC-07) y de los inventarios; recetas (`data/recipes.json`) y su ejecución en SimCore; "nada se consume sin entrega" (DC-06); regeneración de renovables (backlog P1.7, coordinada con WORLD); comercio entre asentamientos; precios/moneda **solo cuando el roadmap lo habilite** (E2+).
- **Exclusiones:** no define edificios (BUILD; ECON provee la función productiva como dato); no modela demografía (POP); no hace UI de recursos (UI lee).
- **Dependencias:** WS-CORE (tipos/eventos/tick), WS-POP (trabajadores disponibles), WS-BUILD (dónde se produce/almacena), WS-WORLD (recursos naturales). DC-05/DC-06/DC-07.
- **Entregables:** contrato de **resource definitions** e **inventories**; cadena trigo→molino→harina→pan funcionando sobre el stock único; recolección↔producción↔consumo hablando el mismo inventario; API de *resource/inventory access* para UI.
- **Permitidos:** `SimCore/include/simcore/{Inventory,GameDatabase,DefaultContent}.h` + `.cpp`; `SimCore/data/{resources,recipes}.json`; módulo de economía/producción en SimCore tras M0.
- **Restringidos:** `SimulationClock/EventBus/World`(contenedor) de CORE; `data/buildings.json` (BUILD; ECON lee tipos de edificio requeridos); actores UE de presentación.
- **Aceptación:** un único inventario por ubicación (sin `ColonyStock` duplicado — ya cumplido, no regresar); la producción valida insumos y no produce sin ellos; agregar un recurso/receta es editar datos, no código.

## WS-POP — Población y Sociedad

- **Alcance:** individuos con identidad persistente (Citizen), familias y hogares (familia ≠ hogar), necesidades, salud, envejecimiento, demografía (natalidad/mortalidad/migración), empleo como habilidad.
- **Responsabilidades:** modelar la población en **SimCore** (Task F, invertir la autoridad hoy en UE: que SimCore conduzca hambre/salud/envejecimiento — ver nota Modelo A); mapear 1:1 `Citizen`↔`CivUnit`; transición individual↔agregado (LOD, DR-10/DR-06) a futuro; profesiones/habilidades.
- **Exclusiones:** no define recetas ni inventarios (ECON); no asigna render del personaje (ART); no construye viviendas (BUILD; POP consume capacidad de vivienda).
- **Dependencias:** WS-CORE (identidad/tiempo), WS-BUILD (vivienda/puestos de trabajo), WS-ECON (consumo de comida/empleo). DC-04, DR-10.
- **Entregables:** contrato de **population**; `Citizen` con nombre/edad/hogar/oficio/necesidades/salud en SimCore; demografía básica atada al reloj; reflejo en `CivUnit` sin lógica de sim en UE.
- **Permitidos:** módulo de población en SimCore tras M0; `civilizations-evolution/Source/CivilizationSimulator/Units/CivUnit.*` (reflejo/estado, sin reglas de sim).
- **Restringidos:** `Inventory/GameDatabase` (ECON), `CivBuildingActor` (BUILD), mallas/animaciones del personaje (ART), `SimulationClock` (CORE).
- **Aceptación:** un `Citizen` conserva identidad e historia tras guardar/cargar; envejece con el reloj; el consumo pasa por el inventario único (no doble descuento); la población agregada es coherente con el total (regla de integración del Maestro).

## WS-UI — Interfaz e Interacción

- **Alcance:** HUD, paneles de información (individuo/edificio), menú de construcción, panel de debug, i18n, cámara RTS y traducción de órdenes del jugador. **Lee** estado de simulación; **nunca** escribe reglas.
- **Responsabilidades:** `CivHUD`/`CivMainHUD`/`CivUIStyle`; `CivText` (EN por defecto + ES); `CivPlayerController` (selección/órdenes/menús); `CivCameraPawn`; minimapa, notificaciones, tooltips; panel de debug (Task H). DC-12.
- **Exclusiones:** no implementa simulación; no define assets/materiales (ART); no inventa datos (consume contratos de CORE/ECON/BUILD/POP/WORLD).
- **Dependencias:** contratos de **UI data access** de todos los dominios; WS-CORE (puente `CivSimSubsystem`). Puede avanzar **ya** con **mock data** del contrato.
- **Entregables:** HUD con fecha/estación/recursos/población; paneles de info con datos reales; menú y panel de construcción (datos de BUILD); panel de debug; i18n completo.
- **Permitidos:** `civilizations-evolution/Source/CivilizationSimulator/UI/*`; `Core/CivHUD.*`, `Core/CivText.*`, `Core/CivPlayerController.*`; `Camera/CivCameraPawn.*`; `Content/UI/` (nuevo).
- **Restringidos:** cualquier `.cpp` de SimCore; `CivSimSubsystem` salvo **lectura** (coordinar con CORE si falta un getter); actores de dominio salvo para **mostrar** su estado.
- **Aceptación:** la UI funciona contra mocks y luego contra datos reales sin cambiar su estructura; no escribe estado de simulación; i18n sin textos hardcodeados.

## WS-ART — Arte, Entorno y Calidad Visual

- **Alcance:** pipeline de assets (import headless desde `ArtStaging/` a `Content/`), meshes y materiales por edificio/época/personaje/naturaleza, iluminación (Lumen/SM6), agua, VFX. **Visual, sin lógica.**
- **Responsabilidades:** reemplazar placeholders por modelos reales (KayKit, Quaternius, Stylized Nature — backlog P0); materiales PBR; agua real; escala/`MeshYawOffset`; iluminación/post-proceso. Solo cambia **qué mesh/material** usa un actor, no su comportamiento.
- **Exclusiones:** no toca reglas de simulación ni estructura de actores lógicos; no modifica contratos.
- **Dependencias:** nombres/slots de mesh que exponen BUILD (`CivBuildingActor`), POP (`CivUnit`) y WORLD (`CivTerrain`). Puede trabajar **ya** sobre los slots existentes.
- **Entregables:** assets importados a `Content/` con convención; materiales PBR aplicados; documento de pipeline (`Docs/ASSETS.md` ya existe — extenderlo).
- **Permitidos:** `civilizations-evolution/Content/**` (meshes/materiales/texturas), `civilizations-evolution/ArtStaging/**` (crudos, **fuera de git**), `Docs/ASSETS.md`, `Docs/UI_STYLE_GUIDE.md` (coordinando con UI). Edición de **asignación de asset** (no lógica) en `CivBuildingActor`/`CivUnit`/`CivTerrain` **solo** vía propiedades expuestas y coordinada con el dueño del archivo.
- **Restringidos:** todo SimCore; la lógica de cualquier actor; contratos. **Regla `.uasset`:** ningún otro workstream edita los mismos `.uasset` que ART tenga asignados en el tablero (ver protocolo binario en [GIT_WORKFLOW.md](GIT_WORKFLOW.md)).
- **Aceptación:** el juego se ve mejor sin cambiar comportamiento ni tests; los `.uasset` tienen dueño único por tarea; imports reproducibles por script.

## WS-QA — QA, Rendimiento e Integración

- **Alcance:** estrategia de pruebas (suite SimCore + automation UE donde sea práctico), rendimiento, instrumentación para depurar comportamiento emergente (RISK-19), y soporte al Director Técnico en validación de integración.
- **Responsabilidades:** mantener y ampliar `SimCore/tests/`; agregar tests de colocación/costos/recetas/consumo/asignación/pausa/velocidad/producción; medir rendimiento (RISK-03/07); herramientas de depuración determinista (RISK-13/19); checklist de aceptación por hito.
- **Exclusiones:** no cambia reglas de dominio para "arreglar" un test (reporta al dueño); no es el integrador final (eso es WS-TD) pero lo asiste.
- **Dependencias:** lee todos los contratos; coordina con WS-TD en cada integración.
- **Entregables:** suite verde reproducible; tests nuevos por sistema; informe de rendimiento y de reproducibilidad; herramientas/logs de depuración.
- **Permitidos:** `SimCore/tests/**`, `SimCore/app/main.cpp` (escenarios de prueba, coordinando con CORE), tests de automation UE (carpeta nueva `Source/.../Tests` si se habilita), scripts de CI/medición.
- **Restringidos:** implementación de dominios (solo propone/añade tests); contratos (los **usa**, no los define).
- **Aceptación:** `simcore_tests` verde en cada integración; cobertura creciente de las reglas críticas; un bug emergente se puede reproducir con semilla + save.

---

## Matriz de no-solapamiento (resumen)

| Concepto compartido | Dueño único | Leen (vía contrato) |
|---|---|---|
| Reloj / fecha / estación | WS-CORE | todos |
| Eventos (bus tipado) | WS-CORE | todos |
| Identidad/relaciones de entidades | WS-CORE | todos |
| Save/load y versión de esquema | WS-CORE | — |
| Catálogo de recursos/materiales | WS-ECON | BUILD, POP, UI, WORLD |
| Inventarios (stock único) | WS-ECON | BUILD, POP, UI |
| Recetas/producción | WS-ECON | BUILD, UI |
| Definiciones y estado de edificios | WS-BUILD | ECON, POP, UI, ART |
| Colocación/validación en terreno | WS-BUILD (+WORLD para terreno) | UI |
| Terreno / biomas / semilla / queries | WS-WORLD | BUILD, ECON, UI |
| Individuos/familias/hogares/demografía | WS-POP | ECON, UI |
| Mesh/material/asset de cualquier actor | WS-ART | — |
| Lectura de estado para pantalla | WS-UI | — (consume a todos) |
| Tests e instrumentación | WS-QA | — |

Regla de oro: si dos workstreams necesitan el mismo dato, **uno es dueño y el otro lee por contrato**; nunca se copia el dato ni se duplica la lógica.

## Qué NO hacer (restricciones del brief, explícitas)

- No modificar gameplay existente sin decisión del Director Técnico ni sobrescribir trabajo sin commitear (leer el estado en disco primero).
- No cambiar la configuración del proyecto (`Config/`, `*.Build.cs`, `.uproject`, versión de UE) sin aprobación: WS-TD lo gestiona.
- No duplicar sistemas ni crear abstracciones vacías prematuras.
- No asumir que todos trabajan en la misma rama (ver [GIT_WORKFLOW.md](GIT_WORKFLOW.md)).
- No cambiar decisiones confirmadas de los GDD en silencio (DC-*/DR-*): documentar y preguntar.
