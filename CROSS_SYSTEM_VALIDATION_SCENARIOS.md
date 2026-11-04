# CROSS_SYSTEM_VALIDATION_SCENARIOS — Civilization Evolutions

> **Phase 2 — Escenarios de validación cruzada (nivel diseño).** Comprueban conceptualmente si la arquitectura soporta la simulación buscada. **No** son tests automatizados todavía. Para cada escenario: sistemas que interactúan, datos compartidos y qué se considera comportamiento correcto. Fuentes: GDD ([DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)); entidades: [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md); flujos/eventos: [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md).
> Versión 1.0 · Última actualización 2026-11-04 · Estado: conceptual.

## Cómo usar
Cada escenario es un "camino" que la arquitectura debe poder recorrer sin duplicar datos, sin acoplar simulación a render y respetando el dueño de cada decisión. Sirven para validar diseño ahora y, más adelante, como base de tests de integración.

---

## Escenario A — Fundación de un asentamiento (30 habitantes)
- **Sistemas:** Entidades, Población, Construcción, Producción/Trabajo, Recursos, Logística, Economía, Tiempo, UI.
- **Datos compartidos:** Asentamiento, Individuo×30 (id persistente), Edificio (vivienda/productivo), Inventario, Recurso.
- **Camino:** se crea el Asentamiento (Entidades) → 30 Individuos con identidad → se construyen viviendas y edificios productivos (Construcción, con costo de Recursos + mano de obra) → se asignan trabajadores (Producción) → recolectan (madera/piedra/bayas) → almacenan (Inventario) → producen/consumen.
- **Correcto si:** cada individuo tiene id/casa/trabajo; la obra respeta costos; la recolección llena inventarios por ubicación; el consumo descuenta stock real; todo avanza con el reloj. **Valida:** vivienda, identidad poblacional, trabajo, recolección, almacenamiento, consumo, producción.
- **Riesgos que estresa:** RISK-01/02/03 (lógica en SimCore vs UE; modelo único; costo por 30 actores).

## Escenario B — Escasez de un recurso
- **Sistemas:** Recursos (naturales), Geografía, Producción, Logística, Economía, Construcción (mantenimiento), Comercio.
- **Datos compartidos:** Recurso natural (madera) con cantidad/agotamiento, Inventario, Ruta, precio.
- **Camino:** se agota la madera cercana (nodo→0, se destruye/regenera según tipo) → la recolección local falla → Logística debe traer de más lejos (mayor costo/tiempo) → Economía sube el precio → Producción/Construcción que dependen de madera se frenan o buscan **material alternativo** → posible comercio.
- **Correcto si:** el agotamiento se propaga a producción/precio/mantenimiento; nada se consume sin entrega; aparece la señal de escasez; hay alternativa (otro material/importación). **Valida:** comportamiento de recursos naturales, recolección, transporte, comercio, precios, mantenimiento, materiales alternativos.
- **Riesgos:** RISK-04 (materiales data-driven), RISK-07 (cadencia de recálculo de precios). Nota: el bug de nodo destruido ya se corrigió (capturar id antes de `Extract`).

## Escenario C — Innovación tecnológica (metalurgia)
- **Sistemas:** Tecnología, Producción, Construcción, Economía, Militar, Comercio, Cultura, Eventos. Marco: [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md).
- **Datos compartidos:** Tecnología/Descubrimiento (id, condiciones), evento `TechnologyDiscovered`, Recetas, Edificios.
- **Camino:** se cumplen condiciones (conocimiento+mineral+especialización) → surge el descubrimiento → el **marco compartido** lo publica → Producción habilita recetas/herramientas, Construcción nuevos materiales, Militar nuevas armas, Economía nuevos bienes, Cultura evolución visual; Eventos registra. La **adopción** requiere materiales+mano de obra+decisión (≠ disponibilidad).
- **Correcto si:** un único descubrimiento desencadena efectos en varios sistemas **sin** que Tecnología implemente la lógica de cada uno; lo viejo coexiste; se puede no adoptar. **Valida:** descubrimiento, adopción, recetas, herramientas, construcción, comercio, consecuencias sociales.
- **Riesgos:** RISK-05 (no scripting; reacción a hecho común).

## Escenario D — Crecimiento poblacional (varias generaciones)
- **Sistemas:** Población, Entidades, Tiempo, Construcción, Economía, Producción.
- **Datos compartidos:** Individuo, Familia (linaje) vs Hogar (residencia), Vivienda, agregado poblacional.
- **Camino:** nacimientos/muertes a lo largo de años → se forman familias y hogares → crece la demanda de vivienda → al superar un umbral, parte de la población pasa de **individual a agregada** (LOD, DR-10) conservando identidad de lo relevante → crecimiento urbano.
- **Correcto si:** la demografía es coherente; **familia ≠ hogar** (un individuo puede pertenecer a una familia sin compartir hogar); al agregar no se pierde la información histórica clave; vivienda/trabajo/comida escalan. **Valida:** nacimientos, muertes, familias, hogares, vivienda, trabajo, comida, crecimiento urbano, agregación.
- **Riesgos:** RISK-03/10 (escala; migración individuo↔agregado sin perder identidad).

## Escenario E — Especialización regional
- **Sistemas:** Administración Territorial, Economía, Producción, Logística/Comercio, Exploración, Geografía.
- **Datos compartidos:** Región, Asentamiento×2, Ruta comercial, Recurso, precios regionales.
- **Camino:** dos asentamientos con recursos distintos (uno agrícola, otro minero) → cada uno se especializa (Producción) → se conectan por Ruta (Logística) → comercian excedentes (Economía) → la Administración los agrupa en región y fija políticas/impuestos.
- **Correcto si:** la distribución geográfica determina la especialización; el comercio equilibra oferta/demanda entre asentamientos; la región administra sin microgestión. **Valida:** distribución de recursos, producción, transporte, comercio, administración regional, relaciones económicas.
- **Riesgos:** RISK-07/09 (cadencia; escala de mundo/mercados).

## Escenario F — Cambio cultural y religioso
- **Sistemas:** Religión, Cultura, Población, Construcción, Política, Diplomacia, Eventos.
- **Datos compartidos:** Religión/Cultura (afiliación), Individuo (creencia), Organización religiosa, Edificio (templo), evento.
- **Camino:** surge un movimiento religioso → se difunde entre individuos/familias (afiliación) → cambia identidad cultural → se construyen templos → influye en política (legitimidad) y diplomacia (relaciones religiosas) → Eventos registra reformas/divisiones.
- **Correcto si:** la afiliación vive en los individuos (no duplicada), la religión deja huella física (templos) y social (política/diplomacia); la convivencia puede ser pacífica o conflictiva según contexto. **Valida:** individuos, familias, identidad cultural, instituciones religiosas, arquitectura, política, diplomacia.
- **Riesgos:** RISK-06 (acoplamiento Cultura/Religión/Población/Política vía eventos, no referencias directas).

## Escenario G — Evolución histórica de largo plazo (siglos)
- **Sistemas:** Tiempo, Tecnología, Construcción, Población, Cultura, Eventos, Progresión, Economía.
- **Datos compartidos:** Edificio (versión histórica + identidad persistente), Civilización, Registro histórico, Tecnología.
- **Camino:** un asentamiento sobrevive siglos → los edificios **evolucionan** (choza→casa de piedra→…) conservando su identidad/historia → la infraestructura y la tecnología cambian → la identidad cultural se transforma manteniendo continuidad → el Registro histórico acumula hechos y biografías → persisten consecuencias.
- **Correcto si:** la misma entidad (edificio/asentamiento/civilización) conserva id e historia a través de evoluciones y del cambio individual↔agregado; la crónica es coherente; se ve la evolución visual del mundo. **Valida:** evolución de edificios, infraestructura, tecnología, población, identidad cultural, registro histórico, consecuencias persistentes.
- **Riesgos:** RISK-08/10 (persistencia/guardado a largo plazo; migración de entidades).

---

## Cobertura por sistema (qué escenario lo ejercita)

| Sistema | A | B | C | D | E | F | G |
|---|---|---|---|---|---|---|---|
| Tiempo | ✓ | | | ✓ | | | ✓ |
| Entidades | ✓ | | | ✓ | | ✓ | ✓ |
| Recursos | ✓ | ✓ | ✓ | | ✓ | | |
| Geografía | | ✓ | | | ✓ | | |
| Construcción | ✓ | ✓ | ✓ | ✓ | | ✓ | ✓ |
| Población | ✓ | | ✓ | ✓ | | ✓ | ✓ |
| Producción | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ |
| Logística/Comercio | | ✓ | ✓ | | ✓ | | |
| Economía | ✓ | ✓ | ✓ | ✓ | ✓ | | ✓ |
| Tecnología | | | ✓ | | | | ✓ |
| Cultura/Religión | | | ✓ | | | ✓ | ✓ |
| Política/Admin | | | | | ✓ | ✓ | |
| Diplomacia | | | | | | ✓ | |
| Eventos | | | ✓ | | | ✓ | ✓ |
| Progresión | | | | | | | ✓ |

**Observación:** los escenarios A, B, C y D cubren el corazón del primer playable (E1–E2 del [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md)) y deberían poder "recorrerse" en diseño antes de implementar; E, F y G validan etapas posteriores.
