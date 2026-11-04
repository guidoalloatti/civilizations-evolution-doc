# SYSTEM_IMPLEMENTATION_SPECIFICATIONS — Civilization Evolutions

> **Phase 2 — Especificaciones de "preparación para implementar", por sistema.** Sin código. Objetivo: que cada tarea futura sea fácil de definir y delegar. Campos por sistema: Propósito · Alcance · Responsabilidades · Entradas · Salidas · Entidades compartidas · Dependencias · Eventos · Reglas de negocio · MVP · Extensiones futuras · Aceptación.
> Fuentes: GDD de cada dominio ([DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)). Entidades: [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md). Orden/dep.: [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md). Versión 1.0 · conceptual.

---

# Parte A — Sistemas fundacionales

## 1. Tiempo y Simulación
- **Propósito:** reloj global único + ciclos internos de distinta frecuencia; coordinar la simulación.
- **Alcance:** fecha/calendario, velocidades (pausa/x1/x2/x3/x5), estaciones, cadencias; NO reglas de dominio.
- **Responsabilidades:** avanzar el tiempo independiente del FPS; emitir ticks/estaciones/año; permitir pausa/aceleración sin alterar resultados.
- **Entradas:** configuración de escala/velocidad; condiciones estacionales por región (de Geografía).
- **Salidas:** eventos de tiempo (TickDía/EstaciónCambió/AñoCambió), fecha actual.
- **Entidades compartidas:** World (fecha).
- **Dependencias:** ninguna (Nivel 0).
- **Eventos:** produce `SeasonChanged`, `YearChanged`, ticks periódicos; reacciona a pausa/velocidad.
- **Reglas:** la aceleración no cambia resultados; los sistemas no usan `Tick()` para todo; estados se conservan entre actualizaciones.
- **MVP:** pausa + velocidades + día/mes/año + estaciones (ya existe en SimCore/CivSimSubsystem).
- **Futuro:** escala configurable fina; estaciones por región; aceleración automática.
- **Aceptación:** el reloj avanza sin depender del FPS; pausar/acelerar no altera el estado resultante.

## 2. Generación del Mundo
- **Propósito:** crear el estado inicial del mundo (procedural, reproducible) y el editor.
- **Alcance:** topografía, hidrología, biomas, suelos, recursos, ubicación inicial de civilizaciones; presets; semilla.
- **Responsabilidades:** generar un mundo determinista a partir de (semilla + config); exponer el estado a Geografía.
- **Entradas:** semilla, preset, parámetros (tamaño, tierra/agua, relieve, clima, recursos).
- **Salidas:** Terreno/Regiones poblados, recursos naturales ubicados.
- **Entidades compartidas:** World, Terreno, Recurso natural.
- **Dependencias:** Geografía (modelo climático), Recursos (catálogo).
- **Eventos:** `WorldGenerated`.
- **Reglas:** mismo seed+config ⇒ mismo mundo; "plano" es preset, no default (DR-02).
- **MVP:** terreno con relieve, agua, recursos y zona plana central (parte existe en CivTerrain).
- **Futuro:** tipos de mapa, biomas variados, editor, mundos grandes por chunks.
- **Aceptación:** genera y muestra un mundo básico reproducible por semilla.

## 3. Recursos y Materiales
- **Propósito:** catálogo único de bienes con identidad, unidades y reglas compartidas.
- **Alcance:** definición de recursos (id, categoría, función económica, calidad, almacenamiento, deterioro, unidad, moneda).
- **Responsabilidades:** ser la **única** fuente de identidad de recursos; proveer inventarios.
- **Entradas:** definiciones data-driven.
- **Salidas:** catálogo + reglas consumidas por todos los económicos.
- **Entidades compartidas:** Recurso/Material, Inventario, Recurso natural (identidad).
- **Dependencias:** ninguna (Nivel 0); Geografía indica dónde hay recursos.
- **Eventos:** —.
- **Reglas:** un id por bien; sin copias por sistema (DC-07); moneda universal; unidades abstractas consistentes.
- **MVP:** subconjunto inicial (wood, stone, berries, meat, wheat, flour, bread, +water) con ids estables (existe en SimCore).
- **Futuro:** calidad/deterioro, recursos estratégicos, catálogo por período.
- **Aceptación:** todos los sistemas referencian el mismo catálogo por id; no hay recursos duplicados.

## 4. Entidades y Relaciones
- **Propósito:** base compartida de identidad y relaciones persistentes.
- **Alcance:** habitantes, familias, edificios, asentamientos, regiones, organizaciones; relaciones; historia persistente.
- **Responsabilidades:** identificadores estables; relaciones múltiples con memoria; evolución/fusión/división sin perder identidad.
- **Entradas:** altas/bajas/cambios de los sistemas de dominio.
- **Salidas:** consultas de identidad/relaciones para todos.
- **Entidades compartidas:** todas.
- **Dependencias:** ninguna (Nivel 0).
- **Eventos:** `EntityCreated/Changed/Removed/Merged`.
- **Reglas:** no duplicar entidades; identidad independiente del nivel de simulación (DR-10); propiedad en capas (DR-07).
- **MVP:** identidad básica para edificios/asentamientos/individuos (hoy implícita en SimCore/UE — **unificar**, RISK-02).
- **Futuro:** relaciones sociales ricas, organizaciones, biografías.
- **Aceptación:** una entidad conserva id e historia al evolucionar/agregarse.

## 5. Construcción y Arquitectura
- **Propósito:** construir y transformar físicamente los asentamientos.
- **Alcance:** colocación (libre + cuadrícula opcional), obra con materiales/trabajadores, evolución, patrimonio, infraestructura.
- **Responsabilidades:** validar y ejecutar obra; gestionar estado/condición; soportar evolución elegida por el jugador.
- **Entradas:** materiales (Recursos), mano de obra (Población), tecnología (habilitación).
- **Salidas:** Edificios/infraestructura; demanda de materiales/trabajo.
- **Entidades compartidas:** Edificio, Asentamiento, Inventario.
- **Dependencias:** Entidades, Recursos, Geografía; Producción, Tecnología.
- **Eventos:** `BuildingConstructed/Upgraded/Abandoned`.
- **Reglas:** si faltan materiales, la obra se detiene; evolución **no** automática; lo viejo coexiste.
- **MVP:** colocación con costo + validación + selección + evolución simple (existe parcial en SimCore + CivBuildingActor/menú UE).
- **Futuro:** construcción libre real, materiales por época, ruinas, patrimonio, urbanismo orgánico.
- **Aceptación:** el jugador coloca/inspecciona edificios y la obra respeta costos.

## 6. Población y Sociedad
- **Propósito:** dinámica demográfica y social de los habitantes.
- **Alcance:** individuos/familias/clases, necesidades, salud, educación, migración, felicidad (según escala).
- **Responsabilidades:** nacimiento/envejecimiento/muerte; necesidades y su satisfacción; clases/representantes; agregación.
- **Entradas:** vivienda (Construcción), alimentos/consumo (Economía), empleo (Producción), condiciones (Geografía).
- **Salidas:** fuerza de trabajo, consumo, estado social, felicidad.
- **Entidades compartidas:** Individuo, Familia/Hogar, Organización.
- **Dependencias:** Tiempo, Entidades, Construcción.
- **Eventos:** `PopulationChanged`, `Migration`, `Epidemic`.
- **Reglas:** modelo híbrido; el agregado conserva coherencia con el total; protagonismo individual decreciente con la escala.
- **MVP:** población (individual chica); necesidades básicas; crecimiento simple. **Divergencia a resolver:** unificar el agregado de SimCore con los settlers UE (RISK-02, DR-10).
- **Futuro:** familias/linajes, clases, salud/educación, migración, modelos matemáticos.
- **Aceptación:** una colonia crece/consume de forma coherente a lo largo de años.

## 7. Producción y Trabajo
- **Propósito:** transformar recursos en bienes/servicios y organizar el trabajo.
- **Alcance:** edificios productivos, recetas, productividad, empleo, evolución de edificios.
- **Responsabilidades:** ejecutar recetas (consumir insumos/producir salidas); asignar trabajadores; adaptar producción a prioridades.
- **Entradas:** insumos (Recursos/Inventario), trabajadores (Población), recetas, tecnología.
- **Salidas:** bienes producidos; demanda de insumos/trabajo.
- **Entidades compartidas:** Edificio, Receta, Profesión, Inventario.
- **Dependencias:** Recursos, Población, Construcción, Tecnología; Logística.
- **Eventos:** `ResourceProduced/Consumed`, `ProductionBlocked`.
- **Reglas:** no produce sin insumos; asignación automática con intervención; evolución visible.
- **MVP:** cadena trigo→molino→harina→panadería→pan + asignación automática (existe en SimCore). (Producción de edificios UE a unificar, RISK-01.)
- **Futuro:** habilidades/calidad/salarios, módulos, mantenimiento.
- **Aceptación:** una cadena productiva funciona consumiendo y generando recursos.

## 8. Almacenamiento y Logística
- **Propósito:** mover y almacenar recursos físicamente.
- **Alcance:** inventarios por ubicación, transporte simplificado, rutas, infraestructura, comercio (niveles).
- **Responsabilidades:** trasladar recursos entre inventarios; impedir consumo sin entrega.
- **Entradas:** inventarios de origen/destino, geografía/distancia, infraestructura.
- **Salidas:** entregas, costos/tiempos (futuro), precios regionales.
- **Entidades compartidas:** Inventario, Ruta, Recurso.
- **Dependencias:** Recursos, Geografía; Producción, Economía.
- **Eventos:** `TransportRequested/Delivered`.
- **Reglas:** nada se consume en destino sin entrega real (DC-06); transporte físico desde el inicio (simplificado).
- **MVP:** transferencia física simplificada (existe en SimCore). Acarreo de settlers al town hall (UE, a unificar).
- **Futuro:** rutas/vehículos/capacidad/costos, comercio marítimo, congestión.
- **Aceptación:** un recurso producido solo se consume en destino tras ser entregado.

## 9. Economía
- **Propósito:** propiedad, consumo, mercados, moneda, impuestos.
- **Alcance:** stocks económicos, precios/oferta/demanda, propiedad mixta, tesorería, impuestos, comercio.
- **Responsabilidades:** marco económico común; precios; consumo; fiscalidad (futuro).
- **Entradas:** producción, trabajo, población (demanda), logística (costos).
- **Salidas:** precios, tesorería, señales de prioridad.
- **Entidades compartidas:** Inventario, Recurso, Civilización (tesorería), Organización.
- **Dependencias:** Recursos, Población, Producción, Logística.
- **Eventos:** `PriceChanged`, `TradeCompleted`.
- **Reglas:** propiedad mixta (DC-05); complejidad configurable; mismo sistema económico en los 3 niveles de comercio.
- **MVP:** stocks + consumo + comercio entre asentamientos simple (existe parcial en SimCore). Sin mercados/precios aún.
- **Futuro:** mercados con precios dinámicos, moneda operativa, impuestos, finanzas.
- **Aceptación:** consumo y comercio básicos coherentes; sin recursos "mágicos".

## 10. Tecnología, Ciencia e Innovación
- **Propósito:** conocimiento que habilita métodos/capacidades por condiciones.
- **Alcance:** disciplinas, descubrimientos, transmisión, difusión, obsolescencia; marco de evolución compartido.
- **Responsabilidades:** habilitar (no adoptar); publicar descubrimientos al marco común.
- **Entradas:** condiciones (conocimiento+recursos+contexto), financiación (Economía), intercambio (Diplomacia).
- **Salidas:** habilitaciones para Producción/Construcción/Logística/Militar; eventos de descubrimiento.
- **Entidades compartidas:** Tecnología/Conocimiento (estado por civilización).
- **Dependencias:** Tiempo, Economía.
- **Eventos:** `TechnologyDiscovered`, `TechnologyDiffused`.
- **Reglas:** por condiciones, no por año (DC-09); disponibilidad ≠ adopción (DR-11); pérdida excepcional.
- **MVP:** árbol/condiciones simples + evolución de edificios (existe en SimCore). Migrar a marco compartido (RISK-05).
- **Futuro:** disciplinas, personajes, difusión, obsolescencia con coexistencia.
- **Aceptación:** un descubrimiento habilita efectos en varios sistemas sin que estos conozcan su implementación.

---

# Parte B — Sistemas restantes (nivel apropiado)

## 11. Cultura, Identidad y Tradiciones
- **Propósito/alcance:** identidad cultural (idioma, arte, tradiciones, festividades) evolutiva y visible. **Dep.:** Población, Religión, Construcción. **Entidades:** Cultura. **Reglas:** evolución orgánica; fusión no destructiva; evolución visual. **MVP:** identidad básica + diferencias regionales simples. **Aceptación:** la cultura se ve en el mundo y cambia con el tiempo.

## 12. Religión y Espiritualidad
- **Propósito/alcance:** creencias, instituciones, arquitectura y figuras religiosas. **Dep.:** Población, Política, Construcción, Cultura. **Entidades:** Religión, Organización, Edificio. **Reglas:** origen orgánico + creación por el jugador; convivencia según contexto. **MVP:** una religión básica + seguidores + un edificio + festividades. **Aceptación:** una religión influye en la sociedad y deja huella física.

## 13. Política y Gobierno
- **Propósito/alcance:** poder, leyes, instituciones, estabilidad. **Dep.:** Población, Economía. **Entidades:** Institución política, Civilización. **Reglas:** el jugador guía (DC-02/DR-05); cambios con consecuencias. **MVP:** estructura simple + estabilidad + pocas políticas/impuestos. **Aceptación:** cambiar una política produce consecuencias coherentes.

## 14. Administración Territorial y Regiones
- **Propósito/alcance:** agrupar/administrar asentamientos en regiones con autonomía configurable. **Dep.:** Política, Entidades; usa Economía/Población/Construcción existentes. **Entidades:** Región/Territorio. **Reglas:** modelo mixto; no duplica funcionalidades. **MVP:** regiones + presupuestos/impuestos simples + administradores automáticos. **Aceptación:** gestionar varias ciudades sin microgestión individual.

## 15. Transporte, Comercio y Logística (comercio)
- Ver sistema 8 (almacenamiento/logística). **Comercio:** niveles básico/intermedio/avanzado sobre el mismo sistema económico; rutas dinámicas; comercio marítimo (futuro). **Aceptación:** comercio local/regional con precios por condiciones.

## 16. Exploración y Expansión Territorial
- **Propósito/alcance:** conocer/ocupar/colonizar el mundo (dirigido + autónomo). **Dep.:** Geografía, Logística. **Entidades:** Región, Asentamiento, Civilización. **Reglas:** conocimiento ≠ ocupación; reutiliza logística. **MVP:** exploración con estados de conocimiento + fundación de asentamientos. **Aceptación:** descubrir territorio y fundar un asentamiento nuevo.

## 17. Diplomacia y Relaciones Internacionales ⚠️
- **GDD truncado (DR-13)** — spec limitada a lo disponible. **Propósito/alcance:** relaciones multidimensionales con memoria histórica entre civilizaciones; tratados; negociación en 3 niveles. **Dep.:** Entidades, Exploración, IA; integra Religión. **Entidades:** Civilización, Registro histórico. **Reglas:** autonomía + intervención amplia; cooperación preferente. **MVP:** por definir (falta en la fuente). **Aceptación:** por definir tras completar el GDD.

## 18. IA y Autonomía de Civilizaciones
- **Propósito/alcance:** civilizaciones autónomas con objetivos/personalidad/historia; mundo que evoluciona sin el jugador. **Dep.:** usa las mismas reglas que el jugador (todos los sistemas). **Entidades:** Civilización. **Reglas:** sin omnisciencia ni conocimiento del futuro; 3 niveles de simulación por distancia/relevancia. **MVP:** 2–3 civilizaciones con economía/tecnología/diplomacia básicas. **Aceptación:** una IA evoluciona de forma plausible sin intervención.

## 19. Guerra y Sistemas Militares
- **Propósito/alcance:** organización militar, combate configurable, fortificaciones, conflictos; secundario. **Dep.:** Logística, Población, Construcción, Diplomacia. **Entidades:** Organización (fuerzas), Edificio (fortificación). **Reglas:** reutiliza logística/recursos; diplomacia para prevenir; consecuencias amplias. **MVP:** conflicto simple + fortificación básica. **Aceptación:** un conflicto coherente con el período, sin volverse el eje.

## 20. Eventos y Evolución Histórica
- **Propósito/alcance:** generar/registrar/representar acontecimientos; historia emergente. **Dep.:** Tiempo; recibe de todos. **Entidades:** Evento, Registro histórico. **Reglas:** el hecho lo origina el sistema dueño; Eventos registra (DC-14). **MVP:** generación mixta simple + registro + notificaciones. **Aceptación:** una partida produce una crónica propia y coherente.

## 21. Interfaz UI/UX
- **Propósito/alcance:** mundo 3D como interfaz principal + paneles/mapas; lee el estado. **Dep.:** lee todos. **Reglas:** no posee reglas; notificaciones desde cambios simulados; accesibilidad prioritaria. **MVP:** HUD de depuración (fecha, recursos, población, construcción, selección, tooltips) — existe. **Aceptación:** valida que la simulación funciona; no invierte en arte final aún.

## 22. Progresión y Finalización
- **Propósito/alcance:** objetivos, puntuación, dificultad, finalización opcional (sandbox). **Dep.:** Tiempo, Eventos; indicadores de todos. **Entidades:** — (lee indicadores). **Reglas:** sandbox auténtico (objetivos desactivables); límite histórico opcional; decisión final del jugador. **MVP:** progresión por períodos + objetivos simples opcionales. **Aceptación:** se puede jugar en sandbox sin objetivos y, opcionalmente, con metas.
