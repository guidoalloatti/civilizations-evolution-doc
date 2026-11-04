# DESIGN_CONSISTENCY_AUDIT — Civilization Evolutions

> **Phase 2 — Auditoría de consistencia de diseño.** Revisión conjunta de los 22 GDD de sistema + Maestro + plan técnico. Análisis, no implementación. **No se resuelven conflictos de diseño por cuenta propia**: se clasifican y, donde corresponde, se remite a una resolución en [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md). Desde el 2026-11-04, DR-00…DR-12 están **confirmadas por delegación** del diseñador (reversibles y auditables); DR-13 sigue **sin resolver** (fuente truncada). La columna **Severidad** y **¿Confirmación del usuario?** (§ Severidad abajo) indica cuáles requieren todavía una decisión humana explícita antes de implementar.
> Versión: 1.1 · Última actualización: 2026-11-04 · Estado: análisis · Fuentes: ver [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md). Extiende (no reemplaza) [AUDIT_REPORT.md](AUDIT_REPORT.md).

## Clasificación usada

| Clase | Significado |
|---|---|
| **CC** Confirmado y consistente | Decisión marcada confirmada, coherente entre documentos. |
| **PI** Inconsistencia potencial | Tensión redaccional o de alcance; puede no ser real. |
| **CX** Contradicción confirmada | Dos fuentes afirman cosas incompatibles. |
| **MD** Decisión de diseño faltante | Hace falta una definición que hoy no existe. |
| **AR** Riesgo arquitectónico | Puede causar refactors caros o acoplamiento. |
| **FC** Consideración futura | Válido, pero fuera del alcance inicial. |

---

## A. Contradicciones y tensiones entre documentos

| ID | Clase | Hallazgo | Fuentes | Remisión |
|---|---|---|---|---|
| F-01 | **CX** | Fecha de corte histórico: "≈1800 d.C." vs "comienzo de la Revolución Industrial" vs "late 18th century". | GDD general §1/§3; Economía; Progresión §8–9; Plan técnico §1 | DR-01 |
| F-02 | **CX** | Perfil del mundo inicial: "mapas planos con bordes naturales" (Generación, decisión confirmada) vs "topografía realista y continua" (Geografía, decisión confirmada). | Generación §21; Geografía §3/§23 | DR-02 |
| F-03 | **PI** | Clima "principalmente visual al inicio" (Construcción) vs variables climáticas con consecuencias y dependencia estacional de agricultura/producción. | Construcción §12; Geografía §7–8; Economía §7; Producción | DR-03 |
| F-04 | **PI** | Nombre del producto: "Historical Civilization Simulator" (plan técnico, GDD general) vs "Civilization Evolutions" (carpeta/proyecto UE). | Plan técnico; 01_GDD_GENERAL; `.uproject` | DR-04 |
| F-05 | **CX** | Rol del jugador: "entidad externa que NO es gobernante" (Política) vs capacidad de dictar leyes, política exterior y decisiones directas (Política, Economía, Diplomacia). | Política §2/§14; Economía; Diplomacia §1 | DR-05 |
| F-06 | **PI** | Estado "pendiente" de Diplomacia/Militar en el GDD general vs GDD especializados ya con decisiones confirmadas. | 01_GDD_GENERAL §20; Guerra §20; Diplomacia | DR-08 (resuelto como "el especializado es la revisión vigente") |
| F-07 | **MD** | Propiedad/titularidad: Recursos y Entidades fijan "titularidad del Estado/civilización + privados operan"; Economía la marca pendiente en detalle (precios, fiscalidad, herencia). | Recursos §11/§13; Entidades §13; Economía §12/§20 | DR-07 |

## B. Mecánicas duplicadas / responsabilidades solapadas

| ID | Clase | Hallazgo | Tratamiento documental vigente |
|---|---|---|---|
| F-08 | **PI** | Geografía y Generación del Mundo describen ambos terreno, clima, biomas, recursos. | Maestro §5: Geografía = estado/procesos naturales; Generación = estado inicial/editor. Mantener ese límite. |
| F-09 | **AR** | "Recursos" aparece en Economía, Producción, Logística, Construcción, Tecnología. | Dueño único = Recursos y Materiales (catálogo/inventario). Los demás referencian, no copian. |
| F-10 | **PI** | Población y Entidades repiten individuo/familia/relaciones. | Población = modelo social/demográfico; Entidades = identidad/relaciones compartidas. |
| F-11 | **PI** | Exploración, Administración y Diplomacia repiten control territorial/autonomía. | Exploración = conocimiento/ocupación; Administración = gestión interna; Diplomacia = relación entre civilizaciones. |
| F-12 | **PI** | Tecnología, Progresión y Eventos repiten "evolución histórica". | Tecnología = habilitaciones/conocimiento; Progresión = objetivos/fin; Eventos = causalidad/memoria. |
| F-13 | **AR** | Logística: Guerra, Exploración y Comercio podrían crear transporte propio. | Guerra/Exploración **reutilizan** el sistema de Transporte/Logística (así lo declaran). No duplicar. |

## C. Definiciones faltantes / decisiones de diseño pendientes (MD)

| ID | Clase | Falta definir | Sistemas afectados | Remisión |
|---|---|---|---|---|
| F-14 | **MD** | Escala temporal exacta (seg reales ↔ día/mes/año) y equivalencia con envejecimiento/estaciones. | Tiempo, Población, Economía, Producción, Eventos | DR-09 |
| F-15 | **MD** | Límites de escala: tamaño de mapa, población máx, nº de civilizaciones, presupuestos de actualización. | Todos (rendimiento) | DR-06 |
| F-16 | **MD** | Umbrales de transición simulación individual ↔ agregada (LOD) y conservación de identidad. | Población, Entidades, IA, Tiempo | DR-10 |
| F-17 | **MD** | Modelo común de propiedad (tierras, edificios, inventarios, organizaciones), herencia y unidad monetaria operativa. | Economía, Entidades, Recursos, Población, Política | DR-07 |
| F-18 | **MD** | Reglas de disponibilidad/adopción/difusión tecnológica y condiciones de transformación de edificios. | Tecnología, Producción, Construcción | DR-11 |
| F-19 | **MD** | Modelos matemáticos: productividad, salarios, precios, necesidades, felicidad, salud, demografía, belleza. | Economía, Producción, Población, Construcción | (pendiente de etapa) |
| F-20 | **MD** | Alcance del prototipo transversal vs los MVP por sistema (qué sistemas y a qué profundidad entran al primer playable). | Todos | DR-12 |
| F-21 | **MD** | Trazabilidad de aprobación (responsable/fecha/versión) de las decisiones "confirmadas". | Gobernanza documental | DR-00 |

## D. Documentos incompletos / de fuente

| ID | Clase | Hallazgo |
|---|---|---|
| F-22 | **CX/MD** | **`GDD_Diplomacia...` TRUNCADO** en la fuente: corta a mitad de §18 ("Personalidad de los líderes"); sin MVP ni tabla de decisiones. Hay que completarlo o recuperar la fuente. |
| F-23 | **PI** | `GDD_Recursos_y_Materiales`, `GDD_Tiempo_y_Simulacion`, `GDD_IA...` y `GDD_Entidades...` **no** traen tabla formal "Decisiones confirmadas" (usan cuerpo/principios/criterios). Riesgo: confundir "afirmado en el cuerpo" con "confirmado formal". |
| F-24 | **PI** | `00_TECHNICAL.md` (proyecto UE) es **idéntico** al plan técnico; `02/03/04` son condensados de GDDs. Riesgo de divergencia futura entre copia y original. |

## E. Riesgos arquitectónicos (resumen; detalle en IMPLEMENTATION_RISK_REGISTER.md)

| ID | Clase | Hallazgo |
|---|---|---|
| F-25 | **AR** | **Doble modelo de población/economía** en el código: agregado en SimCore ↔ individual/colonia en la capa UE. Riesgo de duplicación y de lógica de simulación fuera de SimCore. → RISK-01/02. |
| F-26 | **AR** | **Un Actor por habitante** (`CivUnit`) contradice "no crear un Actor por ciudadano" y la meta de escala. → RISK-03. |
| F-27 | **AR** | Evolución histórica implementada como efectos aislados por sistema en vez de reaccionar a descubrimientos/condiciones comunes. → RISK-05, ver [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md). |
| F-28 | **AR** | Contenido (recursos/edificios/recetas/tecnologías) hardcodeado en C++ (`DefaultContent`) en vez de datos (Data Tables/Assets). → RISK-04. |

## F. Consideraciones futuras (FC)

- Mundos esféricos (Generación): evaluable a futuro, no MVP.
- Clima avanzado, hidrología, ecosistemas, desastres (Geografía): post-MVP.
- Multijugador: fuera del alcance inicial.
- Organizaciones internacionales, estados dependientes, espionaje avanzado (Diplomacia): niveles avanzados.

---

## G. Severidad y confirmación requerida (por hallazgo)

> **Severidad:** *Critical* = bloquea arquitectura/MVP o fuerza refactor caro si se ignora; *High* = afecta varios sistemas, decidir antes de E1–E2; *Medium* = acotable, decidir antes del sistema afectado; *Low* = cosmético/futuro.
> **¿Confirmación del usuario?** *Sí* = decisión humana irreversible o de visión que **no** debo tomar solo (aunque haya DR propuesto); *Delegada* = ya cubierta por la delegación 2026-11-04 (reversible); *No* = regla documental/arquitectónica sin decisión de visión pendiente.

| ID | Clase | Severidad | ¿Confirmación del usuario? | Estado |
|---|---|---|---|---|
| F-01 | CX | **Critical** | **Sí** (corte histórico = alcance del juego) | DR-01 delegada, marcar como visión a ratificar |
| F-02 | CX | **High** | **Sí** (perfil del mundo afecta generación y geografía) | DR-02 delegada |
| F-03 | PI | Medium | Delegada | DR-03 |
| F-04 | PI | Low | Delegada (nombre de producto) | DR-04 |
| F-05 | CX | **Critical** | **Sí** (define el rol del jugador = pilar de diseño) | DR-05 delegada, ratificar |
| F-06 | PI | Medium | No | DR-08 resuelto (el especializado es vigente) |
| F-07 | MD | **High** | **Sí** (modelo de propiedad/herencia/fiscalidad) | DR-07 delegada |
| F-08 | PI | Medium | No | regla de dominio (Maestro §5) |
| F-09 | AR | **High** | No | dueño único = Recursos |
| F-10 | PI | Medium | No | regla de dominio |
| F-11 | PI | Medium | No | regla de dominio |
| F-12 | PI | Medium | No | regla de dominio |
| F-13 | AR | Medium | No | reutilizar Logística |
| F-14 | MD | **High** | Delegada | DR-09 (escala temporal) |
| F-15 | MD | **Critical** | **Sí** (límites de escala = presupuesto de rendimiento) | DR-06 delegada, ratificar |
| F-16 | MD | **High** | Delegada | DR-10 (LOD individual↔agregado) |
| F-17 | MD | **High** | **Sí** (propiedad/moneda operativa) | DR-07 delegada |
| F-18 | MD | Medium | Delegada | DR-11 (adopción tecnológica) |
| F-19 | MD | Medium | No (modelos matemáticos, por etapa) | pendiente de etapa |
| F-20 | MD | **High** | Delegada | DR-12 (alcance del prototipo) |
| F-21 | MD | Medium | No | DR-00 (gobernanza) |
| F-22 | CX/MD | **Critical** | **Sí** (fuente truncada; no inventar diplomacia) | DR-13 **sin resolver** |
| F-23 | PI | Low | No | nota documental |
| F-24 | PI | Low | No | nota documental |
| F-25 | AR | **Critical** | No (regla arquitectónica) | RISK-01/02 |
| F-26 | AR | **High** | No | RISK-03 |
| F-27 | AR | **High** | No | RISK-05 |
| F-28 | AR | Medium | No | RISK-04 |

**Estado de la confirmación humana (actualizado 2026-11-04):** F-01, F-02, F-05, F-07/F-17 y F-15 fueron **ratificados por el usuario** (ver DR-01/02/05/07/06). F-22 (Diplomacia truncada) tiene un **borrador propuesto a validar** ([DIPLOMACIA_PROPUESTA_COMPLETADO.md](DIPLOMACIA_PROPUESTA_COMPLETADO.md)); sigue sin ser canónico. Las demás quedan cubiertas por la delegación (reversibles) o son reglas documentales/arquitectónicas.

---

## Resumen por clase

- **CC** (confirmado y consistente): la mayoría de los pilares (ver DECISION_REGISTER estado `confirmed`).
- **CX** (contradicciones): F-01, F-02, F-05, F-22. → requieren decisión del diseñador.
- **PI** (potenciales): F-03, F-04, F-06, F-08, F-10, F-11, F-12, F-23, F-24.
- **MD** (faltantes): F-07, F-14..F-21.
- **AR** (riesgos): F-09, F-13, F-25..F-28.
- **FC** (futuro): sección F.

**Regla aplicada:** ninguna de estas se resolvió por criterio propio. Las que admiten propuesta técnica tienen una resolución **propuesta** (no confirmada) en el registro de decisiones.
