# CLAUDE.md — Civilization Evolutions

> Instrucciones de proyecto para Claude Code. **Leé este archivo al iniciar cada sesión.**
> Es conciso a propósito: la autoridad de detalle vive en los GDD originales. No copies GDDs acá; referencialos.

## Qué es Civilization Evolutions

Juego de **estrategia y simulación histórica en tiempo real**: una comunidad inicial se transforma, a lo largo de siglos, en una civilización con asentamientos, economía, instituciones, cultura e historia propias. Abarca del **Paleolítico al comienzo de la Revolución Industrial** (corte aprox. 1750–1800, pendiente de confirmación — ver `AUDIT_REPORT.md` C-01). La historia real es **referencia de plausibilidad, no guion obligatorio**: la evolución es libre y no lineal. El jugador es una **entidad que guía** la civilización (no un gobernante), con amplia influencia pero respetando la autonomía de población y civilizaciones no jugadoras. Motor: **Unreal Engine 5**, C++ como lenguaje principal.

Documento de visión: [docs/00-project/GDD_MAESTRO.md](docs/00-project/GDD_MAESTRO.md). Idioma canónico de la documentación: **español**.

## Principios de diseño (no negociables)

1. **Simulación integrada, no features sueltas.** Los sistemas comparten datos y responden a eventos comunes. Una tecnología cambia producción, construcción, transporte, cultura, etc.
2. **Evolución histórica libre.** Períodos = referencias, no desbloqueos obligatorios. Dirigida por condiciones/conocimiento/recursos, **no por el año**.
3. **Evolución visible del mundo.** Edificios, ciudades, paisaje y cultura preservan la huella de la historia.
4. **Geografía con consecuencias.** Condiciona recursos, asentamientos, transporte y desarrollo.
5. **Complejidad configurable** (básico / intermedio / avanzado) y **automatización con intervención** del jugador.
6. **Escala híbrida:** individuos en asentamientos chicos → grupos agregados en civilizaciones grandes, conservando coherencia.
7. **Libertad estratégica:** sin una única ruta óptima. La guerra es relevante pero **secundaria** frente a lo civil/social.

## Restricciones de arquitectura (del plan técnico)

- **Separar simulación de presentación.** La lógica NO depende de meshes/actores renderizados ni del `Tick()` de render. Hoy esto se materializa en **`civilizations-evolution/SimCore/`** (núcleo C++17 motor-agnóstico, con tests) envuelto por la capa UE.
- **Data-driven:** recursos, edificios, recetas, profesiones, tecnologías, materiales y períodos definidos por datos (Data Assets / Data Tables / structs), no hardcodeados.
- **Modular:** dominios independientes que se comunican por interfaces/eventos. Evitar clases monolíticas y abstracciones vacías prematuras.
- **Reloj de simulación** independiente del FPS, con pausa/velocidades y progresión día/mes/año/estación.
- **Escalabilidad y rendimiento** como prioridad desde el diseño; no simplificar la arquitectura para que "entre" el prototipo.
- **Identidad compartida:** un recurso/entidad tiene **un** identificador e inventario común; nunca duplicar recursos/relaciones por sistema.

## Dónde está la documentación

```
Civilization Evolutions/            (raíz de trabajo = este repo; hay una copia solo-docs en el Desktop)
├── CLAUDE.md                        ← este archivo
├── DOCUMENTATION_INDEX.md           ← índice navegable de todos los GDD (punto de entrada)
├── SYSTEM_ARCHITECTURE_MAP.md       ← mapa conceptual de sistemas y relaciones
├── IMPLEMENTATION_ALIGNMENT.md      ← qué dice el diseño vs qué hay implementado
├── DOCUMENTATION_READING_GUIDE.md   ← qué leer antes de cada tipo de tarea
├── (Blueprint Phase 2) DESIGN_CONSISTENCY_AUDIT · DESIGN_DECISION_REGISTER · SHARED_DATA_MODEL ·
│       SYSTEM_DEPENDENCY_MAP · CROSS_SYSTEM_VALIDATION_SCENARIOS · DEVELOPMENT_ROADMAP ·
│       SYSTEM_IMPLEMENTATION_SPECIFICATIONS · HISTORICAL_EVOLUTION_FRAMEWORK ·
│       IMPLEMENTATION_RISK_REGISTER   (análisis derivado de los GDD; DR-00..DR-12 confirmadas por delegación 2026-11-04)
├── AUDIT_REPORT.md                  ← contradicciones (C-01..C-08) y pendientes transversales
├── docs/
│   ├── 00-project/GDD_MAESTRO.md    ← visión, pilares, mapa de autoridad, integraciones, vocabulario
│   ├── 01-systems/                  ← 22 GDD de sistema (fuente de detalle por dominio)
│   └── 02-technical/PLAN_TECNICO_v0.1.md  ← arquitectura técnica (separada del diseño)
├── BACKUP_PRE_AUDIT_2026-11-03/     ← respaldo íntegro; NO editar
└── civilizations-evolution/         ← proyecto Unreal + SimCore + Docs/ condensados
    ├── SimCore/                     ← núcleo de simulación C++ (motor-agnóstico, con tests)
    ├── Source/CivilizationSimulator/← capa UE (presentación/juego)
    └── Docs/                        ← GDD condensados que guiaron el primer código
```

## Cómo navegar la documentación (flujo)

1. Abrí **`DOCUMENTATION_INDEX.md`** para ubicar el GDD del dominio.
2. Leé el **GDD de sistema** correspondiente (autoridad de detalle).
3. Consultá **`GDD_MAESTRO.md` §5 (autoridad) y §6 (integraciones)** para las dependencias entre sistemas.
4. Usá **`DOCUMENTATION_READING_GUIDE.md`** para saber qué GDD leer juntos según la tarea.
5. Para saber qué hay hecho, mirá **`IMPLEMENTATION_ALIGNMENT.md`**.
6. Para decisiones abiertas/contradicciones, **`AUDIT_REPORT.md`**.

Leé el **original** cuando la decisión sea importante; los resúmenes no reemplazan al GDD fuente.

### Qué GDD consultar antes de tocar cada sistema (resumen; detalle en el reading guide)

- **Producción/economía:** `GDD_Produccion_y_Trabajo_v0.2` + `GDD_Economia_v0.2` + `GDD_Recursos_y_Materiales` + `GDD_Transporte_Comercio_y_Logistica` + `GDD_Poblacion_y_Sociedad`.
- **Construcción:** `GDD_Construccion_y_Arquitectura` + `GDD_Recursos_y_Materiales` + `GDD_Tecnologia_Ciencia_e_Innovacion` + `GDD_Poblacion_y_Sociedad`.
- **Mundo/terreno:** `GDD_Generacion_Procedural_del_Mundo` + `GDD_Geografia_Clima_y_Medioambiente` + `GDD_Exploracion_y_Expansion_Territorial`.
- **Población/sociedad:** `GDD_Poblacion_y_Sociedad` + `GDD_Entidades_y_Relaciones` + `GDD_Cultura...` + `GDD_Religion...`.
- **Tiempo:** `GDD_Tiempo_y_Simulacion` (reloj común a todos).

## Reglas para el desarrollo futuro

1. **No contradecir decisiones confirmadas** de los GDD. Si algo es ambiguo o choca, **documentalo como no resuelto y preguntá**; no inventes la respuesta ni la cambies en silencio (actualizá `AUDIT_REPORT.md` y el Maestro cuando se decida).
2. **No dupliques** entidades, recursos, relaciones ni lógica compartida entre sistemas.
3. **Mantené separada la simulación de la presentación** y preferí definiciones data-driven.
4. **Arquitectura modular y extensible**; nada de monolitos.
5. **Desarrollo por etapas**, no todo el juego de una. Respetá los MVP por sistema y el prototipo transversal.
6. **Preservá la flexibilidad histórica y el comportamiento emergente**; no cablees cronologías obligatorias.
7. **No sacrifiques la profundidad de simulación** solo para simplificar el prototipo.
8. **Compilá y probá** tras cada cambio significativo; mantené el proyecto buildeable. Agregá tests (SimCore ya trae una suite).
9. Cuando cambie la documentación, **actualizá el índice y el reading guide**.

## Estado de implementación (resumen; detalle en IMPLEMENTATION_ALIGNMENT.md)

- **SimCore** (C++ puro, con tests): reloj con estaciones, recursos/inventarios, recetas y producción, asignación de trabajadores, logística simplificada, consumo/hambre/salud, construcción con costo, demolición, árbol tecnológico, evolución de edificios, comercio entre asentamientos, guardar/cargar.
- **Capa UE** (`Source/CivilizationSimulator/`): GameMode, PlayerController (selección/órdenes/menú de construcción), CameraPawn (RTS), ViewManager (entorno + HUD), BuildingActor, Terrain procedural, SimSubsystem (puente a SimCore), **CivUnit** (settlers animados), **CivResourceNode** (recolección), **CivHUD**, **CivText** (i18n inglés por defecto + español). Materiales PBR en `/Game/PBR`, personaje en `/Game/Chars`. Lumen/SM6 activados.
- Este código **se está extendiendo activamente** (sistema de recursos del mapa, caza, acarreo). Antes de tocar un archivo, leé su estado actual en disco.

## Notas operativas

- **Build:** `SimCore` con CMake; el proyecto UE con `Build.bat CivilizationSimulatorEditor Win64 Development` (editor cerrado).
- **Git:** el proyecto UE (`civilizations-evolution/`) tiene su **propio** repositorio git (historia hasta el commit `HANDOFF`); la documentación/diseño se versiona en un repo **aparte** en la raíz. El trabajo **posterior al HANDOFF** (arreglos de esta sesión + gameplay nuevo) está **sin commitear**: commiteá en cada checkpoint verde. Con dos sesiones sobre el mismo árbol, cuidado con sobrescrituras; leé antes de editar.
- La carpeta `BACKUP_PRE_AUDIT_2026-11-03/` y los GDD originales **no se editan**.
