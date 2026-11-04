# DESIGN_DECISION_REGISTER — Civilization Evolutions

> **Phase 2 — Registro central de decisiones de diseño.** Tres clases de entrada: **Confirmadas por GDD** (DC-, transcritas de los GDD con fuente), **Confirmadas por delegación** (DR-, resueltas por el asistente porque el usuario delegó explícitamente la decisión — "tomá las decisiones necesarias para avanzar, confío en tu criterio", 2026-11-04) y **Sin resolver** (falta material, no una elección).
> **Trazabilidad:** las DR- NO son del GDD; son decisiones operativas tomadas por el asistente bajo delegación del usuario, **abiertas y reversibles**. Al aplicarlas a un GDD dueño, registrar responsable/fecha/versión y actualizar el resumen del Maestro. Si en el futuro contradicen una fuente, prevalece lo que el usuario decida.
> Versión 1.1 · Última actualización: 2026-11-04 · Fuentes: [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md). Hallazgos: [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md).

**Estados:** `confirmed` (por GDD) · `confirmed` ✓ (delegada) (adoptada bajo delegación, reversible) · `confirmed` ✓✓ (ratificada por el usuario, con fecha) · `unresolved` · `deprecated`.

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

## Parte 2 — Decisiones tomadas por delegación (conflictos y pendientes)

> Cada una responde a un hallazgo del audit. **Confirmadas por delegación del usuario (2026-11-04)**, con recomendación, justificación y consecuencias. **Reversibles:** si querés cambiar alguna, se ajusta y se propaga al GDD dueño. La "Propuesta" de cada una es la decisión adoptada.

### DR-00 — Trazabilidad de decisiones `confirmed` ✓ (delegada)
- **Tema/hallazgo:** F-21. Las tablas dicen "confirmada" sin responsable/fecha.
- **Propuesta:** adoptar este registro como fuente única; al confirmar algo, anotar responsable+fecha+versión y actualizar GDD dueño → Maestro.
- **Afecta:** gobernanza documental. **Consecuencia:** evita tratar supuestos como confirmados.

### DR-01 — Fecha de corte histórico `confirmed` ✓✓ (ratificada por el usuario 2026-11-04)
- **Hallazgo:** F-01 (CX). **Afecta:** Progresión, Tiempo, Tecnología, Economía, contenido.
- **Propuesta (recomendada):** definir el corte como **"inicio de la Revolución Industrial", ~1760–1780**, usado como **límite opcional** (no etapa jugable); después, sandbox sin nuevas etapas industriales. No fijar un año exacto en UI hasta tu OK.
- **Consecuencia:** acota cronología, contenidos y finalización sin cerrar la continuación sandbox.

### DR-02 — Perfil del mundo inicial (plano vs relieve) `confirmed` ✓✓ (ratificada por el usuario 2026-11-04)
- **Hallazgo:** F-02 (CX). **Afecta:** Generación, Geografía, Construcción, Logística.
- **Propuesta:** interpretar "mapas planos" como **preset opcional** (mundo sin relieve marcado / proyección simple), y usar por **defecto relieve coherente** (como ya hace el código actual, con colinas). "Plano" = una opción de generación, no el valor por defecto.
- **Consecuencia:** elimina la contradicción; ambos GDD quedan compatibles vía presets.

### DR-03 — Alcance del clima/estaciones en el MVP `confirmed` ✓ (delegada)
- **Hallazgo:** F-03 (PI). **Afecta:** Geografía, Economía, Producción, Construcción.
- **Propuesta:** **Fase 1** = estaciones con efecto **mínimo** sobre agricultura/producción (lo que ya existe) + clima **visual** en construcción; clima avanzado (meteorología, impacto ambiental, desastres) **post-MVP**. "Principalmente visual" en Construcción = sin efectos estructurales al inicio, pero las estaciones sí modulan producción.
- **Consecuencia:** reconcilia MVP; no bloquea agricultura estacional.

### DR-04 — Nombre del proyecto `confirmed` ✓ (delegada)
- **Hallazgo:** F-04 (PI). **Afecta:** docs/UI.
- **Propuesta:** usar **"Civilization Evolutions"** como nombre de trabajo oficial y unificar documentación/UI; el nombre de módulo técnico `CivilizationSimulator` puede quedar como identificador interno.
- **Consecuencia:** coherencia de marca; sin cambios de código obligatorios.

### DR-05 — Rol y poder del jugador `confirmed` ✓✓ (ratificada por el usuario 2026-11-04)
- **Hallazgo:** F-05 (CX). **Afecta:** Política, Economía, Diplomacia, Interfaz, IA.
- **Propuesta:** el jugador es una **dirección/abstracción** con intervención amplia; sus acciones se clasifican: **directas** (construcción, asignación, economía propia), **de política** (leyes/impuestos vía gobierno, con resistencia/consecuencias) y **consultivas** (donde hay autonomía social/privada/externa). No es un cargo diegético.
- **Consecuencia:** define límites para Política/Diplomacia futuras sin reducir la agencia confirmada.

### DR-06 — Límites de escala (prototipo) `confirmed` ✓✓ (ratificada por el usuario 2026-11-04)
- **Hallazgo:** F-15 (MD). **Afecta:** todos (rendimiento).
- **Propuesta (solo objetivos de prototipo, revisables):** 1 mapa; **20–100** habitantes con simulación individual; pasar a agregado por encima de ~cientos; **2–3** civilizaciones autónomas en el primer mundo vivo. Son metas de producto, no cambios de arquitectura.
- **Consecuencia:** hace verificable el rendimiento y condiciona DR-10 (LOD).

### DR-07 — Modelo común de propiedad/titularidad `confirmed` ✓✓ (ratificada por el usuario 2026-11-04)
- **Hallazgo:** F-07/F-17 (MD). **Afecta:** Economía, Entidades, Recursos, Población, Política.
- **Propuesta (capas, conceptual; detalle en SHARED_DATA_MODEL):** separar **(a) titularidad del recurso natural** (Estado/civilización), **(b) derechos de explotación**, **(c) propiedad del edificio**, **(d) control operativo** (quién lo opera), **(e) ubicación del inventario**. Mantener "propiedad mixta" (DC-05) como confirmada.
- **Consecuencia:** evita duplicar titularidad por sistema; base para fiscalidad/herencia futuras.

### DR-08 — Autoridad de los GDD especializados `confirmed` ✓ (delegada)
- **Hallazgo:** F-06 (PI). **Afecta:** gobernanza.
- **Propuesta:** tratar los GDD especializados como la **revisión vigente**; el listado "pendiente" del GDD general queda **deprecated**.
- **Consecuencia:** quita obsolescencia; sin reclasificar nada como decisión nueva.

### DR-09 — Escala temporal `confirmed` ✓ (delegada)
- **Hallazgo:** F-14 (MD). **Afecta:** Tiempo, Población, Economía, Producción, Eventos.
- **Propuesta:** escala **configurable** (sin número fijo de segundos reales por día); fijar un valor por defecto durante las pruebas de jugabilidad; el envejecimiento/estaciones derivan del reloj, no de constantes dispersas.
- **Consecuencia:** respeta el GDD de Tiempo; habilita tuning sin recableado.

### DR-10 — Umbrales individuo↔agregado (LOD de simulación) `confirmed` ✓ (delegada)
- **Hallazgo:** F-16 (MD). **Afecta:** Población, Entidades, IA, Tiempo.
- **Propuesta:** LOD por **tamaño de asentamiento** (individual bajo un umbral; agregado por encima) y por **distancia/relevancia** para otras civilizaciones (3 niveles, como IA §8), conservando identidad conceptual de las entidades importantes.
- **Consecuencia:** condición para escalar; se vincula al riesgo RISK-03 (Actor por habitante).

### DR-11 — Disponibilidad/adopción/difusión tecnológica `confirmed` ✓ (delegada)
- **Hallazgo:** F-18 (MD). **Afecta:** Tecnología, Producción, Construcción.
- **Propuesta:** habilitación por **condiciones** (conocimiento + recursos + contexto), no por año; **adopción** separada de disponibilidad (el edificio/método se transforma si hay tecnología+materiales+decisión); difusión entre civilizaciones por los mecanismos del GDD. Implementar vía un marco de evolución **compartido** (ver HISTORICAL_EVOLUTION_FRAMEWORK).
- **Consecuencia:** evita efectos tecnológicos aislados por sistema (RISK-05).

### DR-12 — Alcance del prototipo transversal `confirmed` ✓ (delegada)
- **Hallazgo:** F-20 (MD). **Afecta:** todos.
- **Propuesta:** el primer playable integra **Tiempo, Generación/Geografía mínima, Recursos, Entidades, Construcción, Población (individual chica), Producción, Logística, Economía básica, Tecnología básica, UI mínima**; deja fuera Cultura/Religión/Política/Diplomacia/IA/Guerra/Progresión avanzada. Detalle en [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md).
- **Consecuencia:** alinea el MVP transversal con los MVP por sistema.

### DR-13 — Completar el GDD de Diplomacia `proposed` (borrador a validar, 2026-11-04)
- **Hallazgo:** F-22. El archivo fuente está **truncado** (corta en §18). **Afecta:** Diplomacia, IA, Guerra.
- **Estado:** el usuario autorizó (2026-11-04) **proponer** un borrador del faltante. Redactado en [DIPLOMACIA_PROPUESTA_COMPLETADO.md](DIPLOMACIA_PROPUESTA_COMPLETADO.md), **marcado como propuesto, a validar** — NO es canónico ni se incorporó al GDD original (que no se edita). Derivado de lo que ya fijan el propio GDD (hasta §18), IA (DC-11) y Guerra (DC-10); cada supuesto va señalado.
- **Pendiente:** tu validación para promoverlo a confirmado y recién entonces volcarlo al GDD dueño. No bloquea el prototipo (Diplomacia es etapa E5).

---

## Índice de estado

- **Confirmadas por GDD:** DC-01…DC-15.
- **Ratificadas por el usuario (2026-11-04):** DR-01, DR-02, DR-05, DR-06, DR-07 (las de visión/irreversibles que requerían confirmación humana explícita).
- **Confirmadas por delegación (2026-11-04):** DR-00, DR-03, DR-04, DR-08, DR-09, DR-10, DR-11, DR-12 (en efecto para avanzar; reversibles a tu pedido).
- **Propuesta a validar:** DR-13 → borrador en [DIPLOMACIA_PROPUESTA_COMPLETADO.md](DIPLOMACIA_PROPUESTA_COMPLETADO.md) (no canónico hasta tu OK).

> Las DR- de mayor impacto en implementación: **DR-02** (relieve por defecto), **DR-03** (estaciones afectan agricultura; clima avanzado post-MVP), **DR-06** (escala del prototipo), **DR-07** (propiedad en capas), **DR-10** (LOD individuo↔agregado), **DR-12** (alcance del primer playable).
