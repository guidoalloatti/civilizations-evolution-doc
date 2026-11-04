# DESIGN_DECISION_REGISTER — Civilization Evolutions

> **Phase 2 — Registro central de decisiones de diseño.** Dos clases de entrada: **Confirmadas** (transcritas de los GDD, con fuente) y **Propuestas** (resolución recomendada para un conflicto/pendiente, **NO aplicada**, a la espera de tu confirmación). Las propuestas **no** son decisiones: no inventan diseño, lo recomiendan.
> Versión 1.0 · Estado: vivo. Al confirmar una propuesta: cambiá su estado a `confirmed`, registrá responsable/fecha, actualizá el **GDD dueño** y luego el resumen del Maestro. Fuentes: [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md). Hallazgos: [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md).

**Estados:** `confirmed` · `unresolved` · `proposed` (recomendación pendiente) · `deprecated`.

## Parte 1 — Decisiones confirmadas (transcritas de los GDD)

| ID | Tema | Decisión | Fuente | Sistemas afectados |
|---|---|---|---|---|
| DC-01 | Evolución histórica | Libre y no lineal; los períodos son referencia, no desbloqueos obligatorios. | Maestro §4; GDD general §19; Tecnología | Todos |
| DC-02 | Rol del jugador | Entidad que guía (no gobernante) con influencia amplia; sociedad y otras civilizaciones conservan autonomía. | Maestro §4; Política §14 | Política, IA, Interfaz |
| DC-03 | Construcción | Colocación libre con cuadrícula opcional; obra con trabajadores y materiales; evolución arquitectónica elegida por el jugador; conservación de edificios antiguos. | Construcción §22 | Construcción, Producción, Población |
| DC-04 | Población | Modelo híbrido individuos/familias/grupos; detalle según escala y configuración. | Maestro §4; Población §20 | Población, Entidades, Economía |
| DC-05 | Economía/producción | Cadenas interdependientes; asignación de trabajo automática con intervención; propiedad **mixta**; complejidad configurable. | Economía §19; Producción §23 | Economía, Producción, Logística |
| DC-06 | Transporte/comercio | Movimiento físico de recursos desde el inicio (simplificado); comercio en 3 niveles (básico/intermedio/avanzado). | Maestro §4; Logística §22 | Logística, Economía |
| DC-07 | Recurso único | Un recurso = un identificador e inventario común; sin copias por sistema. | Recursos §17; Maestro §6 | Todos los económicos |
| DC-08 | Reloj común | Tiempo continuo con reloj global único y ciclos internos de distinta frecuencia; velocidades no alteran resultados. | Tiempo §2/§15 | Todos |
| DC-09 | Tecnología | Habilita métodos/capacidades por condiciones y conocimiento, **no por el año**; disponibilidad ≠ adopción automática. | Tecnología §27; Maestro §6 | Tecnología, Producción, Construcción, Militar |
| DC-10 | Guerra | Consecuencias amplias; importante pero **secundaria**; reutiliza logística/recursos. | Guerra §20; Maestro §4 | Guerra, Logística, Población |
| DC-11 | IA de civilizaciones | Autónomas, con las **mismas reglas** que el jugador; sin omnisciencia ni conocimiento del futuro. | IA §4/§11 | IA, Diplomacia, Economía |
| DC-12 | Interfaz | El mundo 3D es la interfaz principal; la UI **lee** el estado, no lo posee; accesibilidad prioritaria. | Interfaz §14; Maestro §6 | Interfaz, todos |
| DC-13 | Finalización | Límite histórico **opcional**; continuación posible; decisión final del jugador; sandbox auténtico. | Progresión §13 | Progresión, Tiempo |
| DC-14 | Eventos | Registran hechos de los sistemas dueños; no crean efectos sin regla propietaria. | Eventos §17; Maestro §6 | Eventos, todos |
| DC-15 | Entidades | Identidad persistente y relaciones compartidas; sistemas comparten entidades sin duplicar. | Entidades §2 | Todos |

## Parte 2 — Propuestas para conflictos y pendientes (NO aplicadas)

> Cada una responde a un hallazgo del audit. **Marcadas `proposed`: necesitan tu confirmación.** Incluyen recomendación, justificación y consecuencias.

### DR-00 — Trazabilidad de decisiones `proposed`
- **Tema/hallazgo:** F-21. Las tablas dicen "confirmada" sin responsable/fecha.
- **Propuesta:** adoptar este registro como fuente única; al confirmar algo, anotar responsable+fecha+versión y actualizar GDD dueño → Maestro.
- **Afecta:** gobernanza documental. **Consecuencia:** evita tratar supuestos como confirmados.

### DR-01 — Fecha de corte histórico `proposed`
- **Hallazgo:** F-01 (CX). **Afecta:** Progresión, Tiempo, Tecnología, Economía, contenido.
- **Propuesta (recomendada):** definir el corte como **"inicio de la Revolución Industrial", ~1760–1780**, usado como **límite opcional** (no etapa jugable); después, sandbox sin nuevas etapas industriales. No fijar un año exacto en UI hasta tu OK.
- **Consecuencia:** acota cronología, contenidos y finalización sin cerrar la continuación sandbox.

### DR-02 — Perfil del mundo inicial (plano vs relieve) `proposed`
- **Hallazgo:** F-02 (CX). **Afecta:** Generación, Geografía, Construcción, Logística.
- **Propuesta:** interpretar "mapas planos" como **preset opcional** (mundo sin relieve marcado / proyección simple), y usar por **defecto relieve coherente** (como ya hace el código actual, con colinas). "Plano" = una opción de generación, no el valor por defecto.
- **Consecuencia:** elimina la contradicción; ambos GDD quedan compatibles vía presets.

### DR-03 — Alcance del clima/estaciones en el MVP `proposed`
- **Hallazgo:** F-03 (PI). **Afecta:** Geografía, Economía, Producción, Construcción.
- **Propuesta:** **Fase 1** = estaciones con efecto **mínimo** sobre agricultura/producción (lo que ya existe) + clima **visual** en construcción; clima avanzado (meteorología, impacto ambiental, desastres) **post-MVP**. "Principalmente visual" en Construcción = sin efectos estructurales al inicio, pero las estaciones sí modulan producción.
- **Consecuencia:** reconcilia MVP; no bloquea agricultura estacional.

### DR-04 — Nombre del proyecto `proposed`
- **Hallazgo:** F-04 (PI). **Afecta:** docs/UI.
- **Propuesta:** usar **"Civilization Evolutions"** como nombre de trabajo oficial y unificar documentación/UI; el nombre de módulo técnico `CivilizationSimulator` puede quedar como identificador interno.
- **Consecuencia:** coherencia de marca; sin cambios de código obligatorios.

### DR-05 — Rol y poder del jugador `proposed`
- **Hallazgo:** F-05 (CX). **Afecta:** Política, Economía, Diplomacia, Interfaz, IA.
- **Propuesta:** el jugador es una **dirección/abstracción** con intervención amplia; sus acciones se clasifican: **directas** (construcción, asignación, economía propia), **de política** (leyes/impuestos vía gobierno, con resistencia/consecuencias) y **consultivas** (donde hay autonomía social/privada/externa). No es un cargo diegético.
- **Consecuencia:** define límites para Política/Diplomacia futuras sin reducir la agencia confirmada.

### DR-06 — Límites de escala (prototipo) `proposed`
- **Hallazgo:** F-15 (MD). **Afecta:** todos (rendimiento).
- **Propuesta (solo objetivos de prototipo, revisables):** 1 mapa; **20–100** habitantes con simulación individual; pasar a agregado por encima de ~cientos; **2–3** civilizaciones autónomas en el primer mundo vivo. Son metas de producto, no cambios de arquitectura.
- **Consecuencia:** hace verificable el rendimiento y condiciona DR-10 (LOD).

### DR-07 — Modelo común de propiedad/titularidad `proposed`
- **Hallazgo:** F-07/F-17 (MD). **Afecta:** Economía, Entidades, Recursos, Población, Política.
- **Propuesta (capas, conceptual; detalle en SHARED_DATA_MODEL):** separar **(a) titularidad del recurso natural** (Estado/civilización), **(b) derechos de explotación**, **(c) propiedad del edificio**, **(d) control operativo** (quién lo opera), **(e) ubicación del inventario**. Mantener "propiedad mixta" (DC-05) como confirmada.
- **Consecuencia:** evita duplicar titularidad por sistema; base para fiscalidad/herencia futuras.

### DR-08 — Autoridad de los GDD especializados `proposed`
- **Hallazgo:** F-06 (PI). **Afecta:** gobernanza.
- **Propuesta:** tratar los GDD especializados como la **revisión vigente**; el listado "pendiente" del GDD general queda **deprecated**.
- **Consecuencia:** quita obsolescencia; sin reclasificar nada como decisión nueva.

### DR-09 — Escala temporal `proposed`
- **Hallazgo:** F-14 (MD). **Afecta:** Tiempo, Población, Economía, Producción, Eventos.
- **Propuesta:** escala **configurable** (sin número fijo de segundos reales por día); fijar un valor por defecto durante las pruebas de jugabilidad; el envejecimiento/estaciones derivan del reloj, no de constantes dispersas.
- **Consecuencia:** respeta el GDD de Tiempo; habilita tuning sin recableado.

### DR-10 — Umbrales individuo↔agregado (LOD de simulación) `proposed`
- **Hallazgo:** F-16 (MD). **Afecta:** Población, Entidades, IA, Tiempo.
- **Propuesta:** LOD por **tamaño de asentamiento** (individual bajo un umbral; agregado por encima) y por **distancia/relevancia** para otras civilizaciones (3 niveles, como IA §8), conservando identidad conceptual de las entidades importantes.
- **Consecuencia:** condición para escalar; se vincula al riesgo RISK-03 (Actor por habitante).

### DR-11 — Disponibilidad/adopción/difusión tecnológica `proposed`
- **Hallazgo:** F-18 (MD). **Afecta:** Tecnología, Producción, Construcción.
- **Propuesta:** habilitación por **condiciones** (conocimiento + recursos + contexto), no por año; **adopción** separada de disponibilidad (el edificio/método se transforma si hay tecnología+materiales+decisión); difusión entre civilizaciones por los mecanismos del GDD. Implementar vía un marco de evolución **compartido** (ver HISTORICAL_EVOLUTION_FRAMEWORK).
- **Consecuencia:** evita efectos tecnológicos aislados por sistema (RISK-05).

### DR-12 — Alcance del prototipo transversal `proposed`
- **Hallazgo:** F-20 (MD). **Afecta:** todos.
- **Propuesta:** el primer playable integra **Tiempo, Generación/Geografía mínima, Recursos, Entidades, Construcción, Población (individual chica), Producción, Logística, Economía básica, Tecnología básica, UI mínima**; deja fuera Cultura/Religión/Política/Diplomacia/IA/Guerra/Progresión avanzada. Detalle en [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md).
- **Consecuencia:** alinea el MVP transversal con los MVP por sistema.

### DR-13 — Completar el GDD de Diplomacia `unresolved`
- **Hallazgo:** F-22. El archivo fuente está **truncado** (corta en §18). **Afecta:** Diplomacia, IA, Guerra.
- **Acción:** recuperar/rehacer la parte faltante (desde §18: personalidad de líderes, MVP, decisiones confirmadas). **No** la invento. Requiere tu material o tu OK para proponerla.

---

## Índice de estado

- **Confirmadas:** DC-01…DC-15.
- **Propuestas (pendientes de tu confirmación):** DR-00…DR-12.
- **Sin resolver (necesitan material/decisión):** DR-13 (Diplomacia truncada).

> Para avanzar a implementación conviene confirmar al menos **DR-01, DR-02, DR-03, DR-05, DR-06, DR-07, DR-10, DR-12** (condicionan terreno, clima, escala, propiedad, LOD y alcance del prototipo).
