# SYSTEM_CONTRACTS — Civilization Evolutions

> **Contratos de los sistemas compartidos.** Define las interfaces que cruzan workstreams, con su **dueño**, su **API real hoy** (no inventada: tomada del código en disco — `SimCore/` y `Source/CivilizationSimulator/`), sus **entradas/salidas**, sus **consumidores** y las **reglas** que nadie puede violar. Un consumidor **programa contra este contrato**, no contra la implementación del dueño.
>
> Autoridad de diseño: GDD + [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) + [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md). Organización: [AGENT_ORGANIZATION.md](AGENT_ORGANIZATION.md).
> Versión 1.0 · 2026-11-04 · Estado: PROPUESTA. Los contratos marcados *(estable)* reflejan código actual; *(a definir)* requieren diseño del dueño antes de que los consumidores implementen.

## Cómo se usan estos contratos

1. **El dueño publica y versiona el contrato** (firma + semántica) antes de que nadie implemente contra él.
2. **Los consumidores usan solo lo publicado.** Si falta algo (p. ej. un *getter*), lo **piden al dueño**; no lo agregan en archivos ajenos.
3. **Un cambio incompatible de contrato** pasa por el Director Técnico (WS-TD) y se anuncia a todos los consumidores (ver [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md)).
4. **Mientras el contrato esté *(a definir)*,** los consumidores trabajan contra **mocks** acordados (ej.: la UI ya consume datos mock en `UCivMainHUD`).

## Dos espacios de identidad (clave para todo lo demás)

- **IDs de definición/contenido = `std::string` estables** (data-driven, orientados al diseñador): `ResourceId`, `RecipeId`, `BuildingDefId`, tech id. Ej.: `"wood"`, `"farm"`, `"mill_flour"`, `"masonry"`. **Nunca** se referencia por nombre visible (el nombre localizado vive en `CivText`/`CivLoc`).
- **IDs de instancia viva = `int` en runtime**, asignados por `World` (monótonos desde 1): `buildingId`, `citizenId`. `settlementId` es `int` (0 = colonia/asentamiento compartido). Centinela "sin empleo/sin hogar" = `-1`.

> **La frontera SimCore↔UE es un único punto:** `UCivSimSubsystem` (`Simulation/CivSimSubsystem.{h,cpp}`) es **la única** clase UE que incluye headers de SimCore. Toda la capa UE lee/ordena a través de sus `UFUNCTION`. Ese es, de hecho, el **contrato de acceso UI** (§10). Dueño del archivo puente: **WS-CORE**.

---

## 1. Tiempo de simulación *(estable)*

- **Dueño:** WS-CORE. **Fuente:** `simcore::SimulationClock` (`SimulationClock.h`), orquestado por `simcore::Simulation`. DC-08, DR-09.
- **Entradas:** `advanceDays(double)`, `update(double realSeconds)→double` (días sim transcurridos), `pause()/resume()/setSpeed(double)`; `Simulation::dailyTick()` es **la unidad determinista** de avance (1 día sim).
- **Salidas (solo lectura):** `year()`, `month()` (1..12), `dayOfMonth()` (1..30), `dayOfYear()`, `season()→Season{Spring,Summer,Autumn,Winter}`, `totalDays()`, `isPaused()`, `speed()`; helpers `Simulation::dayFraction()` (0..1), `Simulation::seasonalFactor(BuildingCategory,Season)`.
- **Calendario:** 30 días/mes × 12 = **360 días/año**, 4 estaciones de 90 días. `CalendarConfig{daysPerMonth,monthsPerYear,startYear,daysPerRealSecond}`.
- **Puente UE:** `UCivSimSubsystem` expone `Pause/Resume/IsPaused/SetSpeed/AdvanceDays/GetDateString/GetSeason/GetSeasonName/GetDayFraction`.
- **Reglas:** el reloj es **independiente del FPS** (RISK-07); ningún sistema hace trabajo por `Tick()` de render — reacciona a `dailyTick`/eventos. Las velocidades **no** cambian el resultado (determinismo, RISK-13). Hay **un solo reloj** (sin relojes paralelos por sistema).
- **Consumidores:** todos.

## 2. Comunicación por eventos *(estable, a extender)*

- **Dueño:** WS-CORE. **Fuente:** `simcore::EventBus` (`EventBus.h`), accesible por `Simulation::events()`. DC-14.
- **API:** `subscribe(std::function<void(const Event&)>)`; `emit(const Event&)` (síncrono, en orden, sin prioridades ni *unsubscribe*).
- **`Event`:** `{ EventType type; int intA; std::string strA; double numA; }` (campos de interpretación flexible).
- **`EventType` hoy:** `YearChanged, SeasonChanged, ResourceProduced, ProductionBlocked, BuildingConstructed, BuildingEvolved, TechnologyDiscovered, PopulationChanged, CitizenHired, CitizenDied`.
- **Convención de campos por evento (contrato de payload):**
  | Evento | `intA` | `strA` | `numA` |
  |---|---|---|---|
  | `ResourceProduced` | buildingId | resourceId | cantidad |
  | `ProductionBlocked` | buildingId | buildingDefId | — |
  | `BuildingConstructed` | buildingId | defId | — |
  | `BuildingEvolved` | buildingId | nuevo defId | — |
  | `TechnologyDiscovered` | — | techId | — |
  | `CitizenHired` | buildingId | buildingDefId | — |
  | `CitizenDied` | citizenId | — | — |
  | `PopulationChanged` | delta (±) | — | — |
  | `SeasonChanged` | — | seasonName | — |
  | `YearChanged` | year | — | — |
- **Reglas (DC-14):** el **evento lo origina el sistema dueño del hecho**; Eventos solo registra/notifica; **ningún efecto de juego nace del bus**. Agregar un `EventType` es cambio de contrato → pasa por WS-TD. No convertir el bus en un framework genérico (RISK-11).
- **Consumidores:** todos. *(A definir: puente de eventos SimCore→UE —hoy la UE lee estado por polling en el subsystem, no se suscribe al bus; conectar eventos reales es trabajo de UI "Stage 4" + CORE.)*

## 3. Identificadores persistentes de entidad *(parcial)*

- **Dueño:** WS-CORE (identidad), con el modelo conceptual en [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md). DC-15.
- **Hoy:** `int` de instancia asignados por `World::addBuilding(...)→int` y `World::addCitizen(...)→int`; lookup `World::building(int)→Building*` (búsqueda lineal). **No** hay `World::citizen(int)` público (se itera `citizens()`); existe `findCitizen` interno en `Simulation.cpp`. Contador persistido: `nextBuildingId()/nextCitizenId()` + setters (para save/load).
- **Entradas:** alta de entidad (devuelve id), marca de bajas (`Citizen.alive=false`). **Salidas:** vectores `buildings()`, `citizens()`; agregados `population()`, `employed()`, `housingCapacity()`.
- **Reglas:** id **estable** por vida de la entidad; se **referencia por id**, nunca por nombre/índice de vector; identidad **sobrevive** a guardar/cargar (RISK-16). Sin duplicar identidad en otro sistema (RISK-02).
- **A definir (WS-CORE + WS-POP):** accesor `citizen(int)`; relaciones de primera clase (familia/hogar/linaje) del modelo de datos; política de id al pasar individual↔agregado (DR-10, RISK-10).
- **Consumidores:** todos.

## 4. Definiciones de recursos *(estable)*

- **Dueño:** WS-ECON (catálogo único). **Fuente:** `GameDatabase` + `ResourceDef` (`GameDatabase.h`); datos espejo en `SimCore/data/resources.json`. DC-07.
- **`ResourceDef`:** `{ ResourceId id; std::string displayName; ResourceCategory category; bool perishable; bool isFood; double foodValue; }`. `ResourceCategory{Natural,Food,Processed,Manufactured,Economic}`.
- **API:** `addResource(def)`, `hasResource(id)`, `resource(id)→const ResourceDef*`, `resources()→const map&`. Alias `ResourceAmount=pair<ResourceId,int>`, `ResourceList=vector<ResourceAmount>`.
- **Catálogo MVP:** `wood, stone, berries, meat, wheat, flour, bread` (+ `water, clay` del lado UE/nodos).
- **Reglas (DC-07):** **un recurso = un id e inventario común**; se agrega por **datos** (editar JSON/catálogo), no hardcode disperso (RISK-04). Nombre visible solo en `CivLoc::Resource(id)` (i18n), nunca como clave.
- **Consumidores:** BUILD (costos), POP (consumo), LOG, UI (barra), WORLD (recursos naturales).
- **Nota de deuda:** el núcleo C++ **no parsea** el JSON hoy (catálogo hardcodeado en `DefaultContent.cpp`); migrar a carga real de datos / Data Assets es tarea de ECON (RISK-04, backlog DT.27).

## 5. Inventarios *(estable — modelo A)*

- **Dueño:** WS-ECON. **Fuente:** `simcore::Inventory` (`Inventory.h`); el **stock único** de la colonia es `World::colony()` (asentamiento 0, capacidad ilimitada). DC-06/DC-07.
- **API `Inventory`:** `count(id)`, `total()`, `capacity()/setCapacity(c)` (`<0`=ilimitado), `freeSpace()`, `has(id,qty)`, `add(id,qty)→int añadido`, `remove(id,qty)→int quitado`, `items()→const map&`.
- **Agregados de stock:** `World::resourceStock(id)` (colonia + inventarios de edificios), `foodStock()`, variantes por `settlementId`.
- **Puente UE:** `GetColonyResource(id)` / `AddColonyResource(id,qty)` en `UCivSimSubsystem` (delegan en `world().colony()`).
- **Reglas:** **una ubicación física por contenedor** (DC-06); **nada se consume sin entrega**; **no** se replica stock en otro sistema (ya se eliminó el `ColonyStock` duplicado — **no reintroducir**).
- **Punto de contención (crítico para multi-agente):** hoy **tres** escritores UE mutan `world().colony()` vía `AddColonyResource` — `ACivUnit` (acarreo + consumo de hambre), `ACivBuildingActor` (producción v1), `ACivPlayerController` (costo de construcción). Al paralelizar, cada escritor pertenece a un workstream distinto (POP/BUILD/UI) → **todos escriben el mismo dato por el mismo contrato**; cambios en la semántica de ese contrato pasan por WS-TD.
- **Consumidores:** BUILD, POP, UI, LOG.

## 6. Edificios *(estable, dos caminos a reconciliar)*

- **Dueño:** WS-BUILD (estructura/estado) + WS-ECON (función productiva como dato). DC-03.
- **Definición (`BuildingDef`, de ECON/BUILD en `GameDatabase`):** `{ id; displayName; category; workerSlots; recipeId; storageCapacity; housingCapacity; buildCost; requiredTechs; evolvesTo; evolveTechs; evolveCost; }`. `BuildingCategory{Housing,Extraction,PrimaryProduction,Transformation,Storage,Service}`.
- **Instancia (`World::Building`):** `{ int id; BuildingDefId defId; int settlementId; double x,y; ProductionPriority priority; Inventory inventory; vector<int> workers; bool batchActive; double batchProgress; bool blocked; string blockReason; }`.
- **API (en `Simulation`):** `canAfford(defId)`, `tryPlaceBuilding(defId,x,y,chargeCost)→int` (−1 si falta tech/material), `demolish(id,refund)`, `canEvolve(id)`, `evolveBuilding(id,chargeCost)`, `assignWorkers()`.
- **Puente UE / DTO de lectura:** `FCivBuildingView{ Id, DefId, DisplayName, X, Y, Workers, Slots, bBlocked, BlockReason, Category, bCanEvolve }`; `UCivSimSubsystem::GetBuildingViews()`, `Demolish`, `EvolveBuilding`, `PlaceBuildingWithCost`.
- **⚠️ Dos caminos de colocación a reconciliar (contrato a unificar — BUILD + UI + CORE):**
  1. **Activo:** `ACivPlayerController::TryPlaceAtCursor` cobra madera/piedra y **spawnea `ACivBuildingActor` directo** (sin id de SimCore).
  2. **Diseñado (dormido):** `Simulation::tryPlaceBuilding` + `CivViewManager::SyncBuildings()` reconcilia actores por `GetBuildingViews()` — **comentado** hoy.
  La construcción "real" (obra con progreso, backlog P1.9) exige unificar a (2). **Mientras tanto, el contrato de colocación lo define BUILD; UI solo pide `PlaceBuildingWithCost`.**
- **Reglas:** colocar descuenta del **stock único**; no se coloca sin recursos ni fuera de terreno válido (§8/§9); evolución por tecnología (habilita, no obliga — DC-09).
- **Consumidores:** ECON (recetas/almacén), POP (vivienda/puestos), UI (menú/panel), ART (mesh).

## 7. Población *(parcial — Task F en curso)*

- **Dueño:** WS-POP. **Fuente:** `World::Citizen` + pasos de población en `Simulation`. DC-04, DR-10.
- **`Citizen`:** `{ int id; string name; int settlementId; double ageDays; string profession; int workBuildingId; int homeBuildingId; double hunger; double health; bool alive; }`.
- **API:** `World::addCitizen(name,ageDays,settlementId)→int`, `citizens()`, `population()`, `employed()`, `housingCapacity()`; `Simulation::assignWorkers()`.
- **Puente UE:** `GetAliveCitizenIds()`, `GetCitizenName/GetCitizenAgeYears/GetCitizenProfession(id)`, `KillCitizen(id)`. Cada `ACivUnit` se mapea **1:1** a un `Citizen` por `CitizenId`.
- **⚠️ Autoridad dividida (Modelo A, decisión registrada):** hoy la **capa UE (`ACivUnit`) gobierna hambre/salud/muerte** y SimCore corre con `nutritionPerCitizenPerDay=0` para no doble-descontar. **Task F (próximos slices)** invierte esto: que **SimCore** conduzca hambre/salud/envejecimiento/familias y la UE lo refleje. Ese cambio es de contrato → coordinado WS-POP + WS-CORE + WS-TD; no lo hace UI unilateralmente.
- **A definir (WS-POP):** familias/hogares/linaje (familia≠hogar), natalidad/mortalidad, necesidades múltiples, clases, transición individual↔agregado (DR-10).
- **Reglas:** la población agregada debe ser **coherente** con el total; el consumo pasa por el inventario único (sin doble descuento); identidad persiste en save/load.
- **Consumidores:** ECON (trabajo/consumo), UI (paneles), Cultura/Religión (futuro).

## 8. Consultas de terreno *(parcial)*

- **Dueño:** WS-WORLD. **Fuente:** `ACivTerrain` (`View/CivTerrain.{h,cpp}`). DR-02, C-02/C-03.
- **Hoy:** `HeightAt(X,Y)→float` (ruido determinista suma-de-senos, centro plano `FlatRadius`, escala `HeightScale`); plano de agua a `WaterLevel`; color de vértice grass→rock por altura (bioma crudo); `ApplySeason(0..3)` re-tiñe terreno/hojas/agua. `Resolution=96`, `Extent=16000`.
- **⚠️ Realidad a tener en cuenta:** actores (units/animales/nodos) hacen *ground-snap* por **line trace**, **no** llaman `HeightAt`. El contrato de "terrain query" hoy es de uso interno; exponerlo como servicio (altura/caminable/bioma por punto) es trabajo de WORLD.
- **A definir (WS-WORLD):** `IsWalkable(x,y)`, `BiomeAt(x,y)`, semilla configurable expuesta en UI, distribución de recursos por geología. Sin esto, BUILD valida colocación por reglas propias/trace.
- **Reglas:** misma semilla ⇒ mismo mundo (RISK-13); el índice de estación lo **empuja** `CivViewManager` (WORLD no llama al reloj directo, lo recibe).
- **Consumidores:** BUILD (colocación), ECON/PROD (recursos naturales), UI (minimapa), ART (texturas por bioma).

## 9. Colocación de construcción *(a unificar — ver §6)*

- **Dueño:** WS-BUILD (reglas de colocación) con WS-WORLD (validez sobre terreno) y WS-UI (gesto del jugador).
- **Entradas:** `defId`, posición `(x,y)`/cursor, rotación, confirmación. **Salidas:** `buildingId` nuevo (o fallo con motivo), feedback válido/ inválido, costo descontado.
- **Contrato objetivo:** `UI` captura el gesto → pide `PlaceBuildingWithCost(defId, world-pos)` → `BUILD` valida (recursos vía §5, terreno vía §8, tech vía §6) y devuelve `buildingId`/motivo → `CivViewManager::SyncBuildings` materializa el `ACivBuildingActor` (reactivar el *sync* dormido) → `ART` provee el mesh por categoría/época.
- **Reglas:** un solo camino de colocación (eliminar el *spawn* directo duplicado al cerrar la obra real); no colocar sin recursos/terreno/tech.
- **Consumidores:** UI, ART, ECON.

## 10. Acceso a datos desde la UI *(estable — es el subsystem)*

- **Dueño del puente:** WS-CORE (`UCivSimSubsystem`). **Consumidor principal:** WS-UI. DC-12.
- **Contrato:** la UI **lee** estado llamando `GI->GetSubsystem<UCivSimSubsystem>()` y nunca escribe reglas de simulación. Superficie de lectura actual (UFUNCTION): `GetPopulation`, `GetEmployed`, `GetColonyResource(id)`, `GetDateString`, `GetSeason/GetSeasonName`, `IsPaused`, `GetDayFraction`, `GetBuildingViews()`, `GetCitizenName/AgeYears/Profession(id)`, `availableTechs`/research reads. Comandos permitidos a la UI: `Pause/Resume/SetSpeed/AdvanceDays`, `PlaceBuildingWithCost`, `Demolish`, `EvolveBuilding`, `SaveGame/LoadGame`.
- **Ruta real de HUD:** `UCivMainHUD` refresca cada 0.2 s (`RefreshData()`); `ACivHUD` (Canvas) y el debug de `CivViewManager` son rutas secundarias.
- **⚠️ Datos mock a sustituir (UI "Stage 4"):** en `UCivMainHUD`, *Housing/Wellbeing/Weather* y las tarjetas de notificación son **placeholders** marcados `*`. Conectarlos a estado real requiere getters nuevos (pedidos a los dueños) y el puente de eventos (§2), no cálculo en la UI.
- **Reglas:** si la UI necesita un dato que no existe en el subsystem, **se agrega un getter del lado del dueño** (CORE expone, el dominio provee), no se calcula estado autoritativo en la UI (RISK-01).
- **Consumidores:** WS-UI (y herramientas de WS-QA para debug).

---

## Resumen: dueño → consumidores por contrato

| # | Contrato | Dueño | Consumidores | Estado |
|---|---|---|---|---|
| 1 | Tiempo de simulación | WS-CORE | todos | estable |
| 2 | Eventos | WS-CORE | todos | estable (puente UE a definir) |
| 3 | Identidad de entidades | WS-CORE (+POP) | todos | parcial |
| 4 | Definiciones de recursos | WS-ECON | BUILD, POP, UI, WORLD | estable |
| 5 | Inventarios (stock único) | WS-ECON | BUILD, POP, UI | estable |
| 6 | Edificios | WS-BUILD (+ECON) | ECON, POP, UI, ART | estable (2 caminos) |
| 7 | Población | WS-POP | ECON, UI | parcial (Task F) |
| 8 | Consultas de terreno | WS-WORLD | BUILD, ECON, UI, ART | parcial |
| 9 | Colocación de construcción | WS-BUILD (+WORLD/UI) | UI, ART, ECON | a unificar |
| 10 | Acceso UI al estado | WS-CORE (puente) | UI, QA | estable |

**Regla transversal:** cualquier cambio **incompatible** en estas firmas o semánticas se propone al Director Técnico, se versiona aquí y se avisa a los consumidores antes de integrar.
