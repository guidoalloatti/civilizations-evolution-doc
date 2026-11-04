# Civilization Evolutions — GDD Maestro

**Versión documental:** 1.0 (consolidación editorial)  
**Estado:** Referencia central de diseño; decisiones de alcance y varias especificaciones siguen pendientes  
**Idioma canónico:** Español  
**Período de referencia:** Paleolítico hasta el comienzo de la Revolución Industrial (fecha exacta pendiente; los documentos usan aproximadamente 1800 d. C.)  
**Plataforma inicial:** PC (Windows), según el GDD original  
**Motor propuesto:** Unreal Engine 5; la arquitectura técnica detallada se conserva en [Plan técnico](../02-technical/PLAN_TECNICO_v0.1.md).

> Este documento establece la visión común, alcance, vocabulario y mapa de autoridad. Los GDD de sistema enlazados son la fuente de detalle de sus dominios. Las etiquetas «confirmada» reproducen decisiones que los documentos fuente ya marcaban así; la autoría/fecha de aprobación no está registrada en el material disponible.

## 1. Visión

Civilization Evolutions es un juego de estrategia y simulación histórica en tiempo real en el que una comunidad inicial se transforma durante siglos en una civilización con asentamientos, economía, instituciones, cultura e historia propias. La historia real sirve como referencia de plausibilidad, no como secuencia obligatoria. El jugador puede guiar una trayectoria alternativa mediante construcción libre y decisiones en varios sistemas.

La experiencia prioriza la evolución visible del mundo y la interdependencia entre población, producción, recursos, geografía, conocimiento e instituciones. La guerra es relevante, pero secundaria frente a la simulación civil y social. La población y las civilizaciones no jugadoras conservan agencia propia, en equilibrio con una intervención amplia del jugador.

Inspiraciones registradas en el GDD inicial: Anno 1800, Farthest Frontier, Age of Empires, Civilization II y Cities: Skylines. Son referencias por aspectos concretos; no establecen requisitos de copia.

## 2. Pilares de diseño

1. Evolución histórica de largo plazo con caminos no lineales.
2. Construcción libre y ciudades que conservan huellas de su historia.
3. Economía conectada: recursos, producción, consumo, trabajo y transporte.
4. Sociedad dinámica: población, cultura, religión, política e instituciones.
5. Libertad estratégica sin una única ruta óptima.
6. Geografía con consecuencias para el desarrollo.
7. Complejidad de gestión configurable.
8. Simulación escalable entre detalle individual y agregado.

## 3. Alcance y experiencia

El horizonte de diseño comienza en el Paleolítico y llega al inicio de la Revolución Industrial. Los textos fuente alternan «aproximadamente 1800 d. C.» y «comienzo de la Revolución Industrial»; su relación exacta queda pendiente (ver [auditoría](../../AUDIT_REPORT.md)). El documento de progresión contempla un límite histórico opcional y la posibilidad de continuar en modo libre. La continuidad posterior no implica que la Revolución Industrial esté diseñada como una etapa jugable.

El modo principal previsto es sandbox, con objetivos opcionales, escenarios y desafíos. La guerra no define por sí sola la victoria ni constituye el eje principal. El multijugador figura fuera del alcance inicial en el GDD original.

### Primer prototipo

El GDD inicial propone comprobar un ciclo pequeño: mundo procedural 3D con terreno, agua, bosques y recursos; construcción de viviendas, almacenes, campos y edificios productivos; población individual de 20–100 habitantes; trabajos, necesidades y movimiento básico; agricultura, recolección, almacenamiento, producción y consumo; primeros descubrimientos; ciclo día/noche, estaciones, pausa y aceleración. El criterio de éxito es que el jugador funde una aldea, asigne trabajos, produzca y distribuya alimentos, haga crecer la población, investigue y observe mejoras visibles. Cada sistema puede precisar su propio MVP; cualquier diferencia de secuencia o alcance debe registrarse como dependiente del prototipo, no como cambio de visión.

## 4. Decisiones de diseño confirmadas en documentos fuente

Se conservan como confirmadas porque aparecen bajo secciones «Decisiones confirmadas» o equivalentes en los GDD de sistema. No se encontró registro de quién las aprobó ni cuándo; conviene añadir esa trazabilidad en futuras revisiones.

- **Construcción:** colocación libre con cuadrícula opcional; obra con trabajadores y materiales; evolución arquitectónica elegida por el jugador y posibilidad de conservar edificios antiguos.
- **Población:** modelo híbrido de individuos, familias y grupos; el nivel de detalle cambia con escala y configuración.
- **Economía y producción:** cadenas interdependientes; asignación de trabajo automática con intervención; propiedad mixta y complejidad configurable.
- **Transporte y comercio:** movimiento físico de recursos desde el inicio, con simulación simplificada; comercio con niveles de gestión básico, intermedio y avanzado.
- **Historia y progreso:** evolución flexible y no lineal; períodos como referencias, no como desbloqueos obligatorios.
- **Geografía:** mundo y complejidad configurables; geografía condiciona asentamientos y oportunidades.
- **Otras civilizaciones:** autonomía configurable, con relaciones y trayectorias propias.
- **Guerra:** consecuencias amplias; importancia significativa pero secundaria.
- **Interfaz:** el mundo 3D es la interfaz principal, complementada por paneles y mapas; accesibilidad prioritaria.
- **Final de partida:** límite histórico opcional, continuación posible y decisión final a elección del jugador.

La tabla completa por sistema permanece en cada GDD especializado. Si una ficha de confirmación parece contradecir otra, el conflicto se registra en la auditoría hasta recibir una decisión del diseñador.

## 5. Autoridad documental y límites de cada GDD

| Dominio | Fuente de detalle |
|---|---|
| Visión, pilares, alcance global y prototipo transversal | Este GDD Maestro |
| Economía, mercados, consumo, propiedad económica e impuestos | [Economía](../01-systems/GDD_Economia_v0.2.md) |
| Edificios productivos, recetas, productividad, empleo y calidad | [Producción y trabajo](../01-systems/GDD_Produccion_y_Trabajo_v0.2.md) |
| Obras, materiales constructivos, urbanismo, arquitectura y patrimonio | [Construcción y arquitectura](../01-systems/GDD_Construccion_y_Arquitectura_v0.1.md) |
| Individuos, familias, salud, educación y estructura social | [Población y sociedad](../01-systems/GDD_Poblacion_y_Sociedad_v0.1.md) |
| Gobierno, leyes, instituciones y política interna | [Política y gobierno](../01-systems/GDD_Politica_y_Gobierno_v0.1.md) |
| Creencias, prácticas e instituciones religiosas | [Religión y espiritualidad](../01-systems/GDD_Religion_y_Espiritualidad_v0.1.md) |
| Identidad, idioma, tradiciones y difusión cultural | [Cultura](../01-systems/GDD_Cultura_Identidad_y_Tradiciones_v0.1.md) |
| Ciencia, conocimiento, innovación y tecnología | [Tecnología](../01-systems/GDD_Tecnologia_Ciencia_e_Innovacion_v0.1.md) |
| Transporte, comercio, redes logísticas y mercados físicos | [Transporte, comercio y logística](../01-systems/GDD_Transporte_Comercio_y_Logistica_v0.1.md) |
| Relaciones internacionales, tratados y diplomacia | [Diplomacia](../01-systems/GDD_Diplomacia_y_Relaciones_Internacionales_v0.1.md) |
| Geografía, clima, ecosistemas y procesos ambientales | [Geografía y medioambiente](../01-systems/GDD_Geografia_Clima_y_Medioambiente_v0.1.md) |
| Generación inicial del mapa y editor de mundos | [Generación del mundo](../01-systems/GDD_Generacion_Procedural_del_Mundo_v0.1.md) |
| Exploración, cartografía, colonización y expansión | [Exploración](../01-systems/GDD_Exploracion_y_Expansion_Territorial_v0.1.md) |
| Fuerzas armadas, conflictos y guerra | [Sistemas militares](../01-systems/GDD_Guerra_y_Sistemas_Militares_v0.1.md) |
| Acontecimientos, memoria y cronología | [Eventos e historia](../01-systems/GDD_Eventos_y_Evolucion_Historica_v0.1.md) |
| Regiones, administración y autonomía territorial | [Administración territorial](../01-systems/GDD_Administracion_Territorial_y_Regiones_v0.1.md) |
| Controles, paneles, visualización y accesibilidad | [Interfaz UI/UX](../01-systems/GDD_Interfaz_UI_UX_v0.1.md) |
| Objetivos, dificultad y finalización | [Progresión](../01-systems/GDD_Progresion_y_Finalizacion_v0.1.md) |
| Decisiones autónomas de civilizaciones no jugadoras | [IA y autonomía](../01-systems/GDD_IA_y_Autonomia_Civilizaciones_v0.1.md) |
| Reloj, calendario, escalas y ciclos | [Tiempo y simulación](../01-systems/GDD_Tiempo_y_Simulacion_v0.1.md) |
| Catálogo, identidad y contabilidad de materiales | [Recursos](../01-systems/GDD_Recursos_y_Materiales_v0.1.md) |
| Identidad, relaciones y memoria compartida de objetos del mundo | [Entidades y relaciones](../01-systems/GDD_Entidades_y_Relaciones_v0.1.md) |
| Arquitectura y secuencia técnica existente | [Plan técnico v0.1](../02-technical/PLAN_TECNICO_v0.1.md), separado del diseño de juego |

**Regla de edición:** este Maestro resume las decisiones marco y remite al sistema dueño. Los detalles y tablas completas viven en el GDD de dominio. Los otros sistemas describen sus contratos de entrada/salida, pero no redefinen conceptos cuya autoridad pertenece a otro GDD. Ninguna propuesta se convierte en decisión confirmada por aparecer en este resumen.

## 6. Integraciones entre sistemas

| Interacción | Contrato documental que debe mantenerse |
|---|---|
| Geografía ↔ generación del mundo ↔ exploración | Geografía describe el mundo y sus procesos; generación instancia su estado inicial; exploración controla conocimiento y cartografía. Distinguir terreno real de información conocida por cada sociedad. |
| Recursos ↔ economía ↔ producción ↔ logística | Recursos define identidades y unidades materiales; economía define propiedad, consumo y mercados; producción transforma entradas en salidas; logística mueve inventarios físicamente. Reutilizar un recurso común, sin copias por sistema. |
| Población ↔ entidades ↔ trabajo ↔ economía | Entidades mantiene identidad y relaciones; población define dinámica social y demográfica; producción asigna personas a empleo; economía define compensación y consumo. El modelo agregado debe conservar coherencia con la población total. |
| Tiempo ↔ todos los sistemas | El reloj común coordina ciclos con frecuencias distintas. El calendario y la escala temporal exacta aún no están cerrados. |
| Tecnología ↔ producción, construcción, guerra, transporte y sociedad | Conocimiento/tecnología habilita métodos y cambia capacidades; la disponibilidad no debe confundirse con adopción automática. Las reglas de transición siguen perteneciendo a cada dominio. |
| Política ↔ administración ↔ población ↔ economía | Gobierno define instituciones y políticas; administración las aplica en territorios; población aporta intereses y respuesta social; economía refleja impuestos y asignación. Delimitar quién establece cada política. |
| Cultura ↔ religión ↔ población ↔ arquitectura | Sistemas relacionados, con conceptos propios: cultura cubre identidad y prácticas; religión, creencias e instituciones espirituales; población, afiliación y diversidad; construcción, manifestación física. |
| Diplomacia ↔ IA ↔ guerra ↔ comercio ↔ exploración | Diplomacia es dueña de relaciones y tratados; IA selecciona acciones autónomas; guerra resuelve conflicto armado; economía/logística ejecuta intercambio; exploración registra ocupación y conocimiento territorial. |
| Eventos ↔ todos los sistemas | Los acontecimientos registran cambios derivados de causas de dominio; el sistema de eventos no debe crear efectos de juego sin regla propietaria. Memoria y cronología consumen hechos verificables. |
| Interfaz ↔ todos los sistemas | La interfaz presenta información/órdenes disponibles; no es autoridad de reglas. Notificaciones deben proceder de cambios simulados. |
| Progresión ↔ tiempo ↔ todos los sistemas | Progresión ofrece objetivos/límite opcionales; tiempo lleva la fecha; ningún sistema debe imponer una cronología histórica obligatoria. |

## 7. Vocabulario canónico

- **Asentamiento:** núcleo habitado local; usar para poblado/ciudad cuando no se requiera una escala urbana específica.
- **Ciudad:** asentamiento urbano. **Región:** unidad territorial de administración o análisis que puede agrupar asentamientos.
- **Civilización:** sociedad política e histórica de mayor alcance; no usar como sinónimo de ciudad o asentamiento.
- **Recurso/material:** recurso es la categoría económica compartida; material se reserva para la materia física, especialmente en construcción. Un mismo bien conserva un identificador e inventario común.
- **Producción:** transformación o extracción ejecutada por procesos/edificios. **Trabajo:** asignación, ocupación y habilidades de personas.
- **Transporte:** movimiento físico. **Logística:** planificación, almacenamiento y suministro. **Comercio:** intercambio económico; no implica por sí mismo transporte instantáneo.
- **Generación del mundo:** creación inicial procedural. **Geografía:** estado y procesos naturales del mundo. **Exploración:** adquisición y circulación del conocimiento espacial.
- **Confirmada:** aparece marcada explícitamente como decisión confirmada en un documento fuente. **Propuesta:** resolución sugerida, sin aprobación. **Pendiente:** requiere decisión o especificación. **Fuera de alcance inicial:** excluido del alcance inicial, no necesariamente descartado.
- **MVP/prototipo:** conjunto mínimo de prueba. **Visión final:** capacidad deseada a largo plazo; no implica inclusión en el MVP.

## 8. Decisiones transversales pendientes

1. Confirmar fecha de corte y definir la relación entre «aprox. 1800» y «comienzo de la Revolución Industrial».
2. Especificar el papel formal del jugador en el mundo (entidad externa, representación diegética, autoridad) y sus límites comunes entre política, diplomacia e interfaz.
3. Definir escalas temporales jugables y la correspondencia entre tiempo de simulación, calendario, estaciones y envejecimiento.
4. Determinar límites de escala, mapas, población, civilizaciones simultáneas y presupuestos de simulación.
5. Precisar el alcance inicial de clima y medioambiente: efectos simulados frente a representación principalmente visual en construcción.
6. Acordar el perfil predeterminado del mundo: topografía coherente frente a la mención de mapas iniciales planos en generación del mundo.
7. Definir modelo común de propiedad/titularidad de entidades, tierras, edificios, inventarios y organizaciones.
8. Precisar interoperabilidad de monedas, precios, mercados, impuestos y transferencias entre asentamientos.
9. Definir umbrales y reglas de transición entre simulación individual y agregada.
10. Resolver trazabilidad de aprobación: responsable, fecha, versión y justificación para cada decisión confirmada.

Ver el registro completo de conflictos y propuestas en [AUDIT_REPORT.md](../../AUDIT_REPORT.md). No se resolvieron estos puntos durante esta consolidación.

## 9. Dirección artística y secuencia conceptual

### Dirección artística

El GDD general propone un estilo 3D realista e históricamente reconocible, con detalle alto, sin perseguir fotorrealismo extremo ni una estética caricaturesca. La cámara contempla perspectiva isométrica, rotación, zoom, vista panorámica y acercamiento a edificios, habitantes y unidades. El mundo deberá expresar visualmente el paso del tiempo mediante cambios en arquitectura, materiales, vestimenta, vehículos, infraestructura, vegetación cultivada y paisaje urbano. Los GDD de Construcción, Cultura e Interfaz detallan sus respectivas expresiones. Estas indicaciones son dirección de diseño heredada; las decisiones de producción visual siguen sujetas a iteración.

### Roadmap conceptual heredado

La secuencia del GDD inicial se conserva como propuesta de orden, no como cronograma aprobado:

1. **Fundación técnica:** entorno UE, generación de mapas/terreno, recursos, cámara y construcción básica.
2. **Primer asentamiento:** viviendas, ciudadanos, trabajos, necesidades, agricultura, recolección y almacenamiento.
3. **Economía:** producción, consumo, cadenas, mercados, oferta/demanda y distribución.
4. **Evolución histórica:** tecnologías, desbloqueos, edificios evolutivos, mejoras productivas y cambios arquitectónicos.
5. **Expansión:** asentamientos, infraestructura, comercio y especialización regional.
6. **Sociedad:** grupos, representantes, cultura, religión, instituciones y política.
7. **Mundo vivo:** civilizaciones autónomas, economía independiente, diplomacia, comercio internacional y conflictos.
8. **Preparación del producto:** balance, optimización, animación, sonido, interfaz, tutoriales, pruebas y contenido histórico.

La secuencia no sustituye los MVP por sistema ni el plan técnico. Cambios en prioridad o alcance deben registrarse como decisión de producto.

## 10. Estructura documental

- `docs/00-project/`: visión central y gobernanza de documentación.
- `docs/01-systems/`: GDDs especializados, una fuente de detalle por dominio.
- `docs/02-technical/`: documentación técnica existente, separada del diseño.
- Raíz: informe de auditoría y respaldo íntegro previo a la reorganización.

Para el inventario por archivo y la historia de esta consolidación, consultar [AUDIT_REPORT.md](../../AUDIT_REPORT.md).

