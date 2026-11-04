# AUDIT_REPORT — Civilization Evolutions

**Fecha de auditoría:** 3 de noviembre de 2026  
**Alcance:** 24 documentos de texto encontrados en la raíz antes de los cambios; revisión recursiva de la carpeta y subcarpetas. No había subcarpetas preexistentes, archivos de código/configuración ni repositorio Git visible.  
**Resultado:** documentación inventariada y reubicada; se creó un GDD Maestro central y se preservaron todos los originales en `BACKUP_PRE_AUDIT_2026-11-03/`.

## 1. Resumen ejecutivo

- Inventariados **24 documentos fuente**: 1 GDD general, 22 GDD de sistema y 1 plan técnico.
- **23 documentos** de diseño/técnica se reubicaron bajo `docs/`; los GDD de sistema conservaron su texto y recibieron una cabecera de alcance/autoridad. El plan técnico se reubicó y se conservó sin cambios sustantivos.
- El GDD general original fue sustituido en su ubicación documental por el [GDD Maestro](docs/00-project/GDD_MAESTRO.md), que concentra visión, alcance, decisiones marco, límites de autoridad, integraciones y vocabulario. Los detalles de sistema se consultan en sus GDD fuente.
- No se eliminó ningún documento fuente ni decisión única: el respaldo conserva copias íntegras previas al cambio.
- **No se resolvieron contradicciones por criterio editorial.** Se registran abajo con propuestas, que no son decisiones aprobadas.
- No se modificó código fuente, archivos de configuración ni la arquitectura técnica establecida.

## 2. Inventario de documentos y versiones

Las rutas de origen eran nombres sin extensión en la raíz. Las rutas destino indican la ubicación vigente. El respaldo conserva el nombre y bytes originales. Los estados se transcriben del encabezado fuente; donde el documento no tenía campo de estado, se indica «no declarado».

| # | Documento fuente / tema | Versión | Estado fuente | Ubicación vigente |
|---:|---|---|---|---|
| 1 | Plan técnico / implementación | 0.1 | Initial implementation | [Plan técnico](docs/02-technical/PLAN_TECNICO_v0.1.md) |
| 2 | GDD general / visión inicial | 0.1 | Diseño conceptual inicial | [GDD Maestro](docs/00-project/GDD_MAESTRO.md) |
| 3 | Economía | 0.2 | Diseño conceptual | [GDD Economía](docs/01-systems/GDD_Economia_v0.2.md) |
| 4 | Producción y trabajo | 0.2 | Diseño conceptual | [GDD Producción y trabajo](docs/01-systems/GDD_Produccion_y_Trabajo_v0.2.md) |
| 5 | Construcción y arquitectura | 0.1 | Documento de diseño conceptual | [GDD Construcción](docs/01-systems/GDD_Construccion_y_Arquitectura_v0.1.md) |
| 6 | Población y sociedad | 0.1 | Documento de diseño conceptual | [GDD Población](docs/01-systems/GDD_Poblacion_y_Sociedad_v0.1.md) |
| 7 | Política y gobierno | 0.1 | No declarado en campo separado | [GDD Política](docs/01-systems/GDD_Politica_y_Gobierno_v0.1.md) |
| 8 | Religión y espiritualidad | 0.1 | No declarado en campo separado | [GDD Religión](docs/01-systems/GDD_Religion_y_Espiritualidad_v0.1.md) |
| 9 | Cultura, identidad y tradiciones | 0.1 | No declarado en campo separado | [GDD Cultura](docs/01-systems/GDD_Cultura_Identidad_y_Tradiciones_v0.1.md) |
| 10 | Tecnología, ciencia e innovación | 0.1 | No declarado en campo separado | [GDD Tecnología](docs/01-systems/GDD_Tecnologia_Ciencia_e_Innovacion_v0.1.md) |
| 11 | Transporte, comercio y logística | 0.1 | Diseño conceptual | [GDD Logística](docs/01-systems/GDD_Transporte_Comercio_y_Logistica_v0.1.md) |
| 12 | Diplomacia y relaciones internacionales | 0.1 | Diseño conceptual | [GDD Diplomacia](docs/01-systems/GDD_Diplomacia_y_Relaciones_Internacionales_v0.1.md) |
| 13 | Geografía, clima y medioambiente | 0.1 | Diseño conceptual | [GDD Geografía](docs/01-systems/GDD_Geografia_Clima_y_Medioambiente_v0.1.md) |
| 14 | Generación procedural del mundo | 0.1 | Definición conceptual consolidada | [GDD Generación](docs/01-systems/GDD_Generacion_Procedural_del_Mundo_v0.1.md) |
| 15 | Exploración y expansión territorial | 0.1 | Definición conceptual consolidada | [GDD Exploración](docs/01-systems/GDD_Exploracion_y_Expansion_Territorial_v0.1.md) |
| 16 | Guerra y sistemas militares | 0.1 | Definición conceptual consolidada | [GDD Guerra](docs/01-systems/GDD_Guerra_y_Sistemas_Militares_v0.1.md) |
| 17 | Eventos y evolución histórica | 0.1 | Definición conceptual consolidada | [GDD Eventos](docs/01-systems/GDD_Eventos_y_Evolucion_Historica_v0.1.md) |
| 18 | Administración territorial y regiones | 0.1 | Definición conceptual simplificada | [GDD Administración](docs/01-systems/GDD_Administracion_Territorial_y_Regiones_v0.1.md) |
| 19 | Interfaz UI/UX | 0.1 | Definición conceptual consolidada | [GDD Interfaz](docs/01-systems/GDD_Interfaz_UI_UX_v0.1.md) |
| 20 | Progresión y finalización | 0.1 | Definición conceptual consolidada | [GDD Progresión](docs/01-systems/GDD_Progresion_y_Finalizacion_v0.1.md) |
| 21 | IA y autonomía de civilizaciones | 0.1 | Documento conceptual | [GDD IA](docs/01-systems/GDD_IA_y_Autonomia_Civilizaciones_v0.1.md) |
| 22 | Tiempo y simulación | 0.1 | Diseño conceptual | [GDD Tiempo](docs/01-systems/GDD_Tiempo_y_Simulacion_v0.1.md) |
| 23 | Recursos y materiales | 0.1 | Diseño conceptual | [GDD Recursos](docs/01-systems/GDD_Recursos_y_Materiales_v0.1.md) |
| 24 | Entidades y relaciones | 0.1 | Diseño conceptual | [GDD Entidades](docs/01-systems/GDD_Entidades_y_Relaciones_v0.1.md) |

## 3. Duplicaciones y consolidación

Los solapamientos encontrados eran principalmente cobertura repetida de diseño, no documentos con versiones históricas paralelas:

| Solapamiento | Tratamiento |
|---|---|
| GDD general repetía geografía, generación, economía, población, construcción, tecnología, política, religión, tiempo, eventos y guerra | La referencia central resume el propósito y dirige al dueño del dominio. El GDD Maestro reemplaza la repetición extensa como punto de entrada; el texto previo permanece íntegro en el respaldo y los detalles fuente siguen en los GDD especializados. |
| GDD de Geografía y GDD de Generación del Mundo describían ambos terreno, clima, biomas, recursos y configuración | Se delimitó autoridad: Geografía gobierna estado/procesos naturales; Generación crea el estado inicial/editor. No se eliminaron detalles compartidos que también puedan tener función de diseño. |
| Recursos aparece en Economía, Producción, Logística, Construcción y Tecnología | El Maestro establece Recursos como dueño de identidad/materiales; Economía de propiedad/mercado/consumo; Producción de transformaciones; Logística de movimiento físico. |
| Población y Entidades repetían individuo, familia, grupos, relaciones e identidad | El Maestro distingue el modelo social/demográfico de Población del modelo compartido de identidad/relaciones en Entidades. |
| Exploración, Administración y Diplomacia repetían control territorial, autonomía y dependencias | Se separó conocimiento/ocupación (Exploración), administración interna (Administración) y relación entre civilizaciones (Diplomacia). |
| Tecnología, Progresión y Eventos repetían evolución histórica y hitos | Se asignaron habilitaciones/conocimiento a Tecnología; objetivos/finalización a Progresión; causalidad/memoria a Eventos. |

La duplicación eliminada es la **repetición de autoridad y contenido marco en el GDD general**. Se mantienen explicaciones contextuales necesarias dentro de los GDD de sistema para preservar decisiones y contexto. Las tablas de «Decisiones confirmadas» siguen junto a sus fuentes para trazabilidad; el Maestro contiene solo un resumen.

## 4. Contradicciones, tensiones y propuesta de resolución

**Estado de todas las propuestas:** pendientes de aprobación. No se aplicaron como decisiones.

| ID | Diferencia detectada (fuentes) | Impacto | Propuesta para decisión del diseñador |
|---|---|---|---|
| C-01 | El GDD inicial/económico usa «hasta aproximadamente 1800 d. C.»; Progresión usa «comienzo de la Revolución Industrial» y dice que la etapa no se desarrolla; el plan técnico usa «beginning … approximately late 18th century». | El corte histórico afecta cronología, contenidos y finalización. | Confirmar una única formulación: inicio convencional de la Revolución Industrial, aproximado a 1750–1800 según el marco que se elija; confirmar si seguir después es sandbox sin nuevas etapas industriales. No fijar año hasta aprobación. |
| C-02 | Geografía especifica topografía realista y continua; Generación marca como decisión «inicialmente mapas planos con bordes naturales», además de geología y relieve coherentes. | «Plano» puede referirse a la proyección del mapa o a relieve físico; cambia generación y lectura de alcance. | Aclarar si «plano» es superficie 2D/proyección o mundo sin relieve. Si significa topografía, seleccionar explícitamente un preset de mapa plano y preservar relieve en otros presets. |
| C-03 | Construcción indica que el clima será principalmente visual al inicio; Geografía define variables climáticas y consecuencias; Agricultura/Economía/Producción dependen de estaciones y condiciones ambientales. | Alcance de MVP y dependencias de simulación climática no son consistentes. | Separar fase: efectos climáticos mínimos requeridos para ciclos estacionales/agricultura y efectos avanzados opcionales, indicando qué significa «principalmente visual» en construcción. |
| C-04 | El título del proyecto aparece como «Historical Civilization Simulator» en fuentes y «Civilization Evolutions» en la carpeta/contexto del proyecto; el original marcaba nombre definitivo pendiente. | Nombres de producto y de proyecto pueden divergir en documentación/UI. | Confirmar si Civilization Evolutions es nombre de trabajo o nombre definitivo; actualizar portada y título tras aprobación. |
| C-05 | Política describe al jugador como entidad externa que no es un gobernante; otros textos permiten establecer leyes, elegir política exterior e influir directamente en decisiones. | Falta una definición compartida de la representación diegética y límites de intervención. | Definir el rol del jugador como abstracción de dirección y precisar cuándo las órdenes son directas, políticas o consultivas, sin reducir su agencia amplia ya registrada. |
| C-06 | La visión busca simulación escalable durante milenios con individuos y civilizaciones autónomas; los límites de población/mapa/civilizaciones y cadencias permanecen pendientes. | Riesgo de objetivos de escala no verificables; afecta dependencias técnicas sin implicar cambio de arquitectura actual. | Convertir límites máximos y niveles de simulación en decisiones de producto/prototipo; documentar métricas sin modificar la arquitectura existente en esta auditoría. |
| C-07 | Economía marca como pendiente el sistema de propiedad, precios, distribución y fiscalidad en detalle; Recursos y Entidades ya describen titularidad, dinero, unidad y contabilidad en términos conceptuales. | «Conceptualizado» puede confundirse con «regla cerrada». | Mantener como confirmada solo la dirección de diseño registrada; especificar un modelo común antes de implementación y preservar la propiedad mixta como decisión económica confirmada. |
| C-08 | El GDD general mantenía Diplomacia y Sistema Militar como pendientes, pero GDDs especializados posteriores ya tienen secciones de decisiones confirmadas. | El estado del listado general quedó obsoleto al crecer el corpus. | Tratar el inventario de decisiones especializado como la revisión más reciente del diseño; confirmar procedencia/aprobación y sustituir el listado pendiente global. No se reclasificó como decisión nueva. |

### Duplicados de decisión vs. conflicto

No se consideraron contradicciones: tiempo continuo con ciclos internos de diferente frecuencia; población individual y agregada; automatización con intervención del jugador; transporte físico simplificado; evolución histórica libre con períodos de referencia; finalización opcional con continuación sandbox. Son formulaciones compatibles en las fuentes consultadas.

## 5. Ambigüedades, conceptos sin definir y decisiones incompletas

Además de C-01–C-08, queda por definir o precisar:

- Escala temporal exacta, velocidades, equivalencia entre tiempo real y tiempo histórico y efectos de pausa/aceleración.
- Umbrales, criterios de importancia y conservación de identidad al agregar/desagregar población.
- Tamaño máximo y dimensionalidad del mundo; cantidad de asentamientos y civilizaciones y presupuestos de actualización.
- Fórmulas de productividad, salarios, precios, necesidades, felicidad, salud, crecimiento demográfico y calidad de producto.
- Modelo de propiedad común para tierras, edificios, inventarios, organizaciones y hogares; herencia y unidad monetaria.
- Reglas de disponibilidad, adopción y difusión tecnológica y condiciones de transformación de edificios/métodos.
- Límites de autonomía de líderes, administradores, propietarios privados, regiones y civilizaciones no jugadoras frente a órdenes del jugador.
- Correspondencia entre acciones/eventos registrados y hechos efectivamente producidos por los sistemas dueños.
- Qué sistemas y qué profundidad integran el primer prototipo transversal frente a los MVP descritos por cada GDD.
- Fechas, responsables y referencias que sustenten el estado «confirmado» de las tablas actuales.

Las listas propias de cada GDD se mantienen en su documento fuente; el [GDD Maestro](docs/00-project/GDD_MAESTRO.md) reúne los temas transversales.

## 6. Dependencias e integraciones

Se registró un mapa de contratos en la sección 6 del Maestro. Los principales caminos críticos son:

1. Tiempo como reloj común para ciclos de población, producción, agricultura, clima, eventos y envejecimiento.
2. Geografía → recursos disponibles → producción → inventarios → logística → consumo/mercados.
3. Entidades/relaciones como base de identidad para población, propiedad, instituciones, líderes, edificios y territorios.
4. Conocimiento/tecnología → capacidades de producción, construcción, transporte, navegación y guerra.
5. Política y administración → impuestos/leyes/servicios; población aporta intereses y respuesta; economía registra efectos.
6. Diplomacia/IA → acuerdos, intercambio, expansión y conflicto; guerra debe reutilizar logística/recursos y producir hechos que registra Eventos.
7. Cultura/religión → identidad, prácticas e instituciones; población aporta afiliación; construcción y UI representan su expresión observable.

Quedan incompletos los contratos de datos, reglas de propiedad compartida, cadencias de actualización, precedencia ante eventos simultáneos y dependencias mínimas del prototipo. El mapa establece responsabilidades documentales, no una arquitectura técnica nueva.

## 7. Estructura definitiva

```text
Civilization Evolutions/
├── AUDIT_REPORT.md
├── BACKUP_PRE_AUDIT_2026-11-03/       # 24 originales íntegros
└── docs/
    ├── README.md
    ├── 00-project/
    │   └── GDD_MAESTRO.md
    ├── 01-systems/                    # 22 GDDs especializados
    └── 02-technical/
        └── PLAN_TECNICO_v0.1.md      # separado, texto preservado
```

## 8. Cambios realizados

- Creada copia previa completa de los 24 documentos antes de modificar/reubicar contenido. Los originales conservan nombres de origen dentro de la carpeta de respaldo.
- Reubicados los documentos en la estructura por autoridad y normalizados nombres/errores tipográficos de archivo (Comercio, Militar, Administración); añadido `.md` a los documentos de texto.
- Elaborado el GDD Maestro y un índice de navegación.
- Añadida a cada GDD de sistema una cabecera de control que aclara autoridad, estado y tratamiento de conflictos. No se reescribió el cuerpo de los 22 GDD de sistema.
- Reubicado el plan técnico con cabecera de procedencia; el cuerpo se conserva.
- Consolidada terminología y mapa de integraciones en el Maestro.
- Registradas discrepancias con propuestas de decisión explícitamente no aprobadas.
- No se alteraron código, configuración ni arquitectura técnica.

## 9. Estado de verificación

Se completó el inventario contra los archivos respaldados y se revisaron encabezados, versiones, secciones de decisiones confirmadas/pendientes, límites de MVP e integraciones. La copia previa se creó antes de las modificaciones. La auditoría es documental; no se realizaron pruebas de código porque no se modificó código.

### Recomendación de mantenimiento

Al aprobar una propuesta, registrar responsable, fecha, versión y justificación; actualizar primero el GDD dueño y luego el resumen del Maestro. Mantener los originales de auditoría de forma inmutable y crear respaldos nuevos antes de futuras consolidaciones.
