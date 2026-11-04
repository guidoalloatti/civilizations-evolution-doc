# DOCUMENTATION_READING_GUIDE — Civilization Evolutions

> Guía práctica para consultar la documentación **sin releer todo en cada tarea**. Empezá siempre por [CLAUDE.md](CLAUDE.md) → [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md) → el GDD del dominio → [GDD_MAESTRO §6](docs/00-project/GDD_MAESTRO.md) para dependencias. Leé el **original** cuando la decisión sea importante.

## Estructura de carpetas

```
docs/
├── 00-project/GDD_MAESTRO.md          Visión, autoridad, integraciones, vocabulario, pendientes
├── 01-systems/                        22 GDD de sistema (fuente de detalle por dominio)
└── 02-technical/PLAN_TECNICO_v0.1.md  Arquitectura técnica (separada del diseño)
AUDIT_REPORT.md                        Contradicciones (C-01..C-08) y pendientes transversales
CLAUDE.md / DOCUMENTATION_INDEX.md / SYSTEM_ARCHITECTURE_MAP.md / IMPLEMENTATION_ALIGNMENT.md / este archivo
civilizations-evolution/               Proyecto UE + SimCore + Docs/ condensados + código
BACKUP_PRE_AUDIT_2026-11-03/            Respaldo inmutable (no editar)
```

## Categorías de documentación

- **Visión/gobernanza:** `GDD_MAESTRO.md` (qué es el juego, quién decide qué, cómo se integran los sistemas).
- **GDD de sistema (`docs/01-systems/`):** autoridad de detalle de cada dominio. Cada uno trae (según el doc) tabla de **Decisiones confirmadas**, sección de **MVP** y niveles de **complejidad** (básico/intermedio/avanzado).
- **Técnica (`docs/02-technical/`):** cómo construirlo en UE5/C++ (arquitectura, milestones, reglas). Separada del diseño.
- **Operativa (`civilizations-evolution/`):** condensados que guiaron el primer código, HANDOFF, READMEs y ASSETS.
- **Meta de navegación (raíz):** este guide, el índice, el mapa de arquitectura y el alignment.

## Qué leer primero según la tarea

| Si vas a trabajar en… | Leé (en este orden) |
|---|---|
| **Producción / edificios productivos** | `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Economia_v0.2` → `GDD_Recursos_y_Materiales` → `GDD_Transporte_Comercio_y_Logistica` → `GDD_Poblacion_y_Sociedad` → Maestro §6 |
| **Economía / mercados / impuestos** | `GDD_Economia_v0.2` → `GDD_Recursos_y_Materiales` → `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Poblacion_y_Sociedad` → `GDD_Politica_y_Gobierno` |
| **Recursos / inventarios / materiales** | `GDD_Recursos_y_Materiales` → `GDD_Economia_v0.2` → `GDD_Entidades_y_Relaciones` |
| **Transporte / comercio / logística** | `GDD_Transporte_Comercio_y_Logistica` → `GDD_Economia_v0.2` → `GDD_Geografia_Clima_y_Medioambiente` |
| **Construcción / urbanismo / arquitectura** | `GDD_Construccion_y_Arquitectura` → `GDD_Recursos_y_Materiales` → `GDD_Tecnologia_Ciencia_e_Innovacion` → `GDD_Poblacion_y_Sociedad` → `GDD_Cultura_Identidad_y_Tradiciones` |
| **Mundo / terreno / biomas** | `GDD_Generacion_Procedural_del_Mundo` → `GDD_Geografia_Clima_y_Medioambiente` → `GDD_Exploracion_y_Expansion_Territorial` |
| **Población / familias / clases** | `GDD_Poblacion_y_Sociedad` → `GDD_Entidades_y_Relaciones` → `GDD_Economia_v0.2` → `GDD_Cultura...` → `GDD_Religion...` |
| **Tiempo / estaciones / velocidades** | `GDD_Tiempo_y_Simulacion` → Maestro §6 (reloj común) |
| **Tecnología / investigación** | `GDD_Tecnologia_Ciencia_e_Innovacion` → `GDD_Produccion_y_Trabajo_v0.2` → `GDD_Construccion_y_Arquitectura` |
| **Cultura / religión** | `GDD_Cultura_Identidad_y_Tradiciones` + `GDD_Religion_y_Espiritualidad` → `GDD_Poblacion_y_Sociedad` |
| **Política / administración territorial** | `GDD_Politica_y_Gobierno` → `GDD_Administracion_Territorial_y_Regiones` → `GDD_Poblacion_y_Sociedad` |
| **Diplomacia / IA / otras civilizaciones** | `GDD_Diplomacia...` (⚠️ truncado) + `GDD_IA_y_Autonomia_Civilizaciones` → `GDD_Exploracion...` + `GDD_Guerra...` |
| **Guerra / militar** | `GDD_Guerra_y_Sistemas_Militares` → `GDD_Transporte_Comercio_y_Logistica` → `GDD_Diplomacia...` → `GDD_Construccion...` (fortificaciones) |
| **Eventos / historia emergente** | `GDD_Eventos_y_Evolucion_Historica` → el GDD del dominio que origina el hecho |
| **Interfaz / HUD / mapas** | `GDD_Interfaz_UI_UX` → el GDD del sistema cuyos datos se muestran |
| **Objetivos / finalización / dificultad** | `GDD_Progresion_y_Finalizacion` → `GDD_Tiempo_y_Simulacion` |
| **Identidad/relaciones de entidades** | `GDD_Entidades_y_Relaciones` → `GDD_Poblacion_y_Sociedad` → `GDD_Administracion_Territorial_y_Regiones` |
| **Cualquier cosa técnica (arquitectura/UE/C++)** | `docs/02-technical/PLAN_TECNICO_v0.1.md` + `IMPLEMENTATION_ALIGNMENT.md` |

**Regla general:** casi todo toca **Recursos**, **Entidades**, **Tiempo** y **Población**. Ante la duda, revisá esos cuatro + el Maestro §6.

### Análisis derivado (blueprint Phase 2, raíz del proyecto)

Para **planificar implementación** (no reemplaza a los GDD; es análisis derivado):

| Si necesitás… | Leé |
|---|---|
| Saber qué conflictos/decisiones hay abiertos y su severidad | [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) + [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) |
| Modelar entidades sin duplicar (familia≠hogar, propiedad, agregación) | [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) |
| Orden de implementación, qué depende de qué, dueño del dato | [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md) |
| Validar que un flujo multi-sistema "cierra" antes de codear | [CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md) (escenarios A–G) |
| Qué construir en cada etapa y qué NO aún | [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) + [SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md](SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md) |
| Implementar evolución histórica/tecnología sin scripting | [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md) |
| Anticipar riesgos y si bloquean el MVP | [IMPLEMENTATION_RISK_REGISTER.md](IMPLEMENTATION_RISK_REGISTER.md) |

> **Antes de implementar una feature multi-sistema**, recorré su escenario en CROSS_SYSTEM_VALIDATION_SCENARIOS para confirmar que los datos y eventos ya existen en el modelo.

## Cómo encontrar…

- **Decisiones confirmadas:** sección "Decisiones confirmadas" (tabla) al final de cada GDD. Nota: `Recursos`, `Tiempo`, `IA`, `Entidades` y `Diplomacia` **no** tienen esa tabla — sus reglas están afirmadas en el cuerpo / en "principios" o "criterios de aceptación". Resumen transversal en Maestro §4.
- **Decisiones no resueltas / ambigüedades:** `AUDIT_REPORT.md` (C-01..C-08 + §5) y Maestro §8. Varios GDD traen su propia lista ("Temas pendientes/para profundizar") — con sección explícita solo en `Poblacion_y_Sociedad`; los demás difieren lo avanzado a sus niveles/MVP.
- **Requisitos del MVP:** sección "MVP" de cada GDD + el **MVP transversal** en `01_GDD_GENERAL` §18 y Maestro §3 ("Primer prototipo"). El plan técnico define Milestones 1–6.
- **Guía técnica de implementación:** `PLAN_TECNICO_v0.1.md` (§4 principios, §5 estructura, §6 núcleo, §17 milestones, §18 reglas) + `IMPLEMENTATION_ALIGNMENT.md` (qué hay hecho).
- **Dependencias entre sistemas:** Maestro §6 (contratos) + sección "Integración con otros sistemas" de cada GDD + `SYSTEM_ARCHITECTURE_MAP.md`.
- **Vocabulario canónico:** Maestro §7 (asentamiento, ciudad, región, civilización, recurso/material, producción/trabajo, transporte/logística/comercio, confirmada/propuesta/pendiente, MVP/visión final).

## Reglas al consultar/editar documentación

1. No resolver contradicciones por edición silenciosa: registrarlas en `AUDIT_REPORT.md` y, al decidirse, actualizar primero el GDD dueño y luego el resumen del Maestro.
2. Etiquetar contenido nuevo como **Confirmado / Propuesto / Pendiente** con versión, responsable, fecha y justificación.
3. No editar los originales del respaldo (`BACKUP_PRE_AUDIT_2026-11-03/`).
4. Cuando cambie la documentación, actualizar **este guide** y el **índice**.
5. Los resúmenes (índice, mapa, alignment) **no reemplazan** al GDD fuente para decisiones importantes.
