# IMPLEMENTATION_ALIGNMENT — Civilization Evolutions

> Relación entre el **código existente** y los **GDD**. Análisis, **no** se modificó código. El código está en evolución activa (hay una sesión paralela implementando el sistema de recursos del mapa), así que esto es una **foto** del estado; antes de tocar un archivo, leé su estado actual en disco. Diseño: ver [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md) y [SYSTEM_ARCHITECTURE_MAP.md](SYSTEM_ARCHITECTURE_MAP.md).

## Dónde vive el código

```
civilizations-evolution/
├── SimCore/                         Núcleo de simulación C++17 motor-agnóstico (compila y testea fuera de UE)
│   ├── include/simcore/ + src/      Types, SimulationClock, Inventory, GameDatabase, World, Simulation, DefaultContent, EventBus
│   ├── app/main.cpp                 Runner de consola (corre la aldea por años)
│   └── tests/                       Suite propia (~20 tests / 87 checks)
└── Source/CivilizationSimulator/    Capa Unreal (presentación + juego)
    ├── SimCoreUnity.cpp             Compila SimCore dentro del módulo UE
    ├── Core/                        CivGameModeBase, CivPlayerController, CivHUD, CivText
    ├── Camera/                      CivCameraPawn
    ├── View/                        CivViewManager, CivBuildingActor, CivTerrain
    ├── Simulation/                  CivSimSubsystem (puente a SimCore + estado UE-side)
    └── Units/                       CivUnit, CivResourceNode
Content/PBR (materiales), Content/Chars (personaje KayKit).
```

## Mapa GDD → implementación

| Sistema (GDD) | Estado | Dónde | Notas / divergencias |
|---|---|---|---|
| **Tiempo y Simulación** | Parcial | `SimCore/SimulationClock`, `CivSimSubsystem` (tick, pausa, velocidad, +mes) | Reloj con día/mes/año/estaciones, pausa/velocidades. Falta: estaciones por región, calendario configurable. Alineado con el principio de reloj independiente del FPS. |
| **Recursos y Materiales** | Parcial | `SimCore` (Types/Inventory/GameDatabase) + `CivResourceNode` + stock UE en `CivSimSubsystem` | Catálogo con ids estables (wood/stone/berries/meat/wheat/flour/bread + water/fruit UE). **Divergencia:** hay **dos** contabilidades — el stock de SimCore y un `ColonyStock` UE-side. Riesgo de duplicación (el GDD exige inventario único). Sin calidad/deterioro/moneda. |
| **Economía** | Mínimo | `SimCore/Simulation` (stocks, comercio entre asentamientos, consumo/salud) | Sin mercados, precios, oferta/demanda, moneda, impuestos ni propiedad. |
| **Producción y Trabajo** | Parcial | `SimCore` (recetas, asignación automática de trabajadores) + `CivBuildingActor` (producción UE por intervalo) | Cadena trigo→molino→harina→panadería→pan en SimCore. **Divergencia:** la producción de los edificios colocados por el menú UE (recolector→bayas, cazador→carne) corre en `CivBuildingActor` (UE), no en SimCore. Sin habilidades/calidad/salarios. |
| **Transporte y Logística** | Parcial | `SimCore` (logística simplificada) + acarreo UE de settlers al town hall | "Nada se consume sin ser entregado" se respeta conceptualmente. El acarreo de `CivUnit` es UE-side. Sin rutas/vehículos/infra. |
| **Construcción y Arquitectura** | Parcial | `SimCore` (defs de edificio + evolución + costo + demolición) + `CivBuildingActor` + menú en `CivPlayerController`/`CivHUD` | Colocación con costo (madera/piedra), evolución por tecnología en SimCore. Meshes placeholder (cubo/cono/cilindro). Falta: construcción libre real, materiales por época, ruinas, patrimonio, cuadrícula opcional. |
| **Población y Sociedad** | Parcial / **divergente** | `SimCore` (población **agregada**, trabajadores, consumo) + `CivUnit` (settlers **individuales** UE) | Coexisten dos modelos: el agregado de SimCore y los 20 actores `CivUnit`. El GDD pide modelo **híbrido unificado**; hoy están separados. Sin familias, clases, necesidades, salud individual, demografía. |
| **Generación del Mundo** | Mínimo | `CivTerrain` (ruido determinista, centro plano + colinas, agua, árboles) | Sin semilla configurable expuesta, biomas, tipos de mapa ni editor. |
| **Geografía/Clima/Medioambiente** | Mínimo | `CivTerrain` (tinte estacional), estaciones del reloj | Colinas + agua + tinte por estación. Sin clima simulado, recursos geológicos, flora/fauna, desastres. (El relieve con colinas se acerca a Geografía "realista", no a la decisión "mapas planos" de Generación — ver **C-02**.) |
| **Tecnología** | Parcial | `SimCore` (árbol tecnológico + autoinvestigación + evolución de edificios) | Simple y lineal-ish, no el modelo orgánico por disciplinas/personajes del GDD. |
| **Interfaz UI/UX** | Parcial (MVP) | `CivHUD` + HUD de `CivViewManager` (fecha/estación, settlers, recursos, barra superior, menú de construcción, tooltips, caja de selección) | Coincide con el "UI mínima de depuración" del plan técnico. Falta UI final, mapas temáticos, accesibilidad. |
| **Entidades y Relaciones** | No (implícito) | ids sueltos en SimCore / UE | No hay modelo compartido de entidades/relaciones persistentes. |
| **Eventos** | Mínimo | `SimCore/EventBus` (eventos internos) | Sin historia emergente, memoria ni cronología. |
| **Cultura · Religión · Política · Administración · Exploración · Diplomacia · IA de civilizaciones · Guerra · Progresión** | No implementados | — | Fuera del prototipo actual. (Townhall con "refugio + flechas + enemigos" quedó propuesto, no implementado.) |

## Qué funciona hoy (loop jugable)

Arranque con **town hall** + **20 settlers animados (KayKit)**. El jugador **selecciona** (clic / caja), **mueve** (clic derecho), y ordena **recolectar** árboles/rocas/agua; el settler junta y (en progreso) **acarrea al town hall**. Con recursos, **construye** choza/cazador/recolector desde el menú; los edificios **producen** con el tiempo. HUD con fecha/estación, barra de recursos y población activa/máxima. Cámara RTS con tilt/rotación/paneo. Visual: césped/roca/corteza **PBR** + **Lumen/SM6**. i18n **inglés por defecto + español**.

## Alineación con los principios de arquitectura (plan técnico §4)

- ✅ **Separación simulación/presentación:** fuerte en el núcleo original — `SimCore` es C++ puro, testeado, motor-agnóstico; la capa UE lo envuelve.
- ⚠️ **Erosión de esa separación (riesgo principal):** la funcionalidad nueva de colonia (settlers `CivUnit`, nodos `CivResourceNode`, `ColonyStock` en `CivSimSubsystem`, producción en `CivBuildingActor`) vive **en la capa UE**, no en SimCore. Esto mete **lógica de simulación en la presentación**, la deja **fuera de los tests** de SimCore y crea un **segundo modelo** de población/economía en paralelo al de SimCore.
- ⚠️ **Escalabilidad:** `CivUnit` es un **Actor por habitante**. El plan pide explícitamente **no** crear un Actor por ciudadano y soportar agregación. A 20 va bien; no escala a la visión de grandes poblaciones.
- ◑ **Data-driven:** el contenido (recursos/edificios/recetas/tecnologías) está en **C++** (`DefaultContent`), no en Data Tables/Assets de UE. Es "data-ish" pero no editable como datos desde el editor.
- ✅ **Modularidad / reloj independiente del FPS / configuración:** respetados en SimCore.

## Riesgos arquitectónicos (para decidir antes de seguir)

1. **Doble modelo de población/economía** (SimCore agregado ↔ UE-side individual). Duplica entidades/recursos, contra el Maestro ("no copias por sistema"). Decidir: ¿la colonia/settlers bajan a SimCore (recomendado por testabilidad/escala) o SimCore pasa a ser legado?
2. **Gameplay en la capa UE** reduce cobertura de tests y acopla simulación a actores renderizados.
3. **Un Actor por settler** no escala; faltará un nivel agregado (LOD de simulación) como pide el GDD.
4. **Sin control de versiones (Git)** en el checkout de trabajo, con dos sesiones editando: el plan técnico pide inicializar Git **antes** de gameplay sustancial. Alto riesgo de pérdida/colisión.
5. **Sin modelo de Entidades/Relaciones** compartido: los ids ad-hoc dificultarán propiedad, historia y relaciones futuras.
6. **Contenido hardcodeado** en C++: migrar a datos (Data Tables/Assets) facilitará el diseño data-driven que pide el plan.

## Decisiones pendientes que condicionan la implementación

Antes de profundizar conviene resolver (ver `AUDIT_REPORT.md`): **C-02** (terreno plano vs realista — el código ya usa colinas), **C-03** (alcance del clima/estaciones que afecta agricultura/producción), **C-05** (rol/poder del jugador para el futuro sistema político), **C-06** (límites de escala, que determinan si se mantienen actores por settler o se agrega), y el **modelo común de propiedad/entidades** (C-07 + GDD Entidades).

## Restricciones de este análisis

No se modificó código fuente ni GDDs (es una tarea de comprensión). Las recomendaciones de refactor **no** se aplicaron; son insumos para una decisión de diseño/arquitectura del responsable del proyecto.
