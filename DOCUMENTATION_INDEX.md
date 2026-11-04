# DOCUMENTATION_INDEX — Civilization Evolutions

> Punto de entrada para encontrar la documentación. Para cada documento: ruta, versión, estado, propósito, decisiones confirmadas clave, dependencias y secciones principales (para lectura dirigida).
> Autoridad de visión: [docs/00-project/GDD_MAESTRO.md](docs/00-project/GDD_MAESTRO.md). Contradicciones y pendientes: [AUDIT_REPORT.md](AUDIT_REPORT.md). Cómo leer según la tarea: [DOCUMENTATION_READING_GUIDE.md](DOCUMENTATION_READING_GUIDE.md).

**Idioma canónico:** español. **Leyenda de estado:** `Confirmado` (tabla de decisiones consolidada) · `Parcial` (decisiones + pendientes) · `Incompleto` (sin tabla formal / base en el cuerpo) · `Truncado` (archivo cortado en la fuente).

---

## Blueprint de diseño (Phase 2 — análisis derivado de los GDD)

Documentos de análisis en la raíz del proyecto. **Derivados** de los GDD originales (no los reemplazan); distinguen decisiones confirmadas de propuestas.

| Documento | Propósito |
|---|---|
| [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) | Hallazgos clasificados (CC/PI/CX/MD/AR/FC): contradicciones, duplicaciones, faltantes, riesgos. |
| [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) | Decisiones confirmadas (DC-01..) + **propuestas** para conflictos (DR-00..DR-13, pendientes de confirmación). |
| [SHARED_DATA_MODEL.md](SHARED_DATA_MODEL.md) | Modelo conceptual de entidades compartidas, propiedad en capas y relaciones. |
| [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md) | Grafo de dependencias, flujos de información, propagación de eventos y cadencias. |
| [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) | Etapas E0–E6 con objetivos, dependencias, aceptación, riesgos y qué NO hacer aún. |
| [SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md](SYSTEM_IMPLEMENTATION_SPECIFICATIONS.md) | Spec de preparación por sistema (entradas/salidas/eventos/reglas/MVP/aceptación). |
| [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md) | Evolución histórica como marco compartido (reacción a descubrimientos/condiciones, no scripting). |
| [IMPLEMENTATION_RISK_REGISTER.md](IMPLEMENTATION_RISK_REGISTER.md) | Riesgos arquitectónicos/de desarrollo (RISK-01..12) con mitigación y cuándo abordarlos. |

Además: [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md) (código vs diseño) y [SYSTEM_ARCHITECTURE_MAP.md](SYSTEM_ARCHITECTURE_MAP.md) (mapa conceptual).

---

## 0. Proyecto y gobernanza

### GDD Maestro
- **Ruta:** `docs/00-project/GDD_MAESTRO.md` · **Versión:** 1.0 (consolidación) · **Estado:** Referencia central (decisiones de alcance pendientes).
- **Propósito:** Visión común, pilares, alcance, mapa de autoridad por dominio, integraciones entre sistemas, vocabulario canónico, decisiones transversales pendientes, dirección artística y roadmap conceptual.
- **Clave:** define quién es dueño de cada decisión (§5) y los contratos de integración (§6). Ninguna propuesta es decisión por aparecer acá.
- **Secciones:** 1 Visión · 2 Pilares · 3 Alcance y prototipo · 4 Decisiones confirmadas (resumen) · 5 Autoridad documental · 6 Integraciones · 7 Vocabulario · 8 Pendientes transversales · 9 Dirección artística y roadmap · 10 Estructura documental.

### Plan técnico
- **Ruta:** `docs/02-technical/PLAN_TECNICO_v0.1.md` (copia idéntica en `civilizations-evolution/Docs/00_TECHNICAL.md`) · **Versión:** 0.1 · **Estado:** Initial implementation.
- **Propósito:** Fundación técnica (UE5, C++). Principios de arquitectura, estructura de proyecto, núcleo de simulación, mundo, construcción, recursos, producción, población, logística, UI, testing, Git, milestones 1–6, reglas de desarrollo.
- **Clave:** separación simulación/presentación; data-driven; modular; reloj independiente del FPS; escalabilidad individual→agregado; flexibilidad histórica. El mundo se simula como sistema coherente, no features sueltas.
- **Secciones:** 1 Overview · 2 Objetivos · 3 Stack · 4 Principios · 5 Estructura · 6 Núcleo de simulación · 7 Mundo · 8 Construcción · 9 Recursos · 10 Producción · 11 Población · 12 Logística · 13 UI · 14 Config · 15 Testing · 16 Git · 17 Milestones · 18 Reglas · 19 Primera tarea · 20 Visión técnica final.

### GDD general original (consolidado por el Maestro)
- **Ruta:** `civilizations-evolution/Docs/01_GDD_GENERAL.md` · **Versión:** 0.1 · **Estado:** Diseño conceptual (histórico; su autoridad pasó al Maestro).
- **Propósito:** Visión original completa: períodos históricos, evolución de edificios por época, población híbrida, economía, tecnología, política, religión, tiempo, dirección artística, roadmap de 8 fases, **MVP** y 12 principios no negociables, 16 decisiones pendientes.

---

## 1. Mundo, geografía y tiempo

### Generación procedural del mundo
- **Ruta:** `docs/01-systems/GDD_Generacion_Procedural_del_Mundo_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Sistema que crea escenarios geográficos coherentes, diversos y configurables (semillas reproducibles, editor de mundos).
- **Confirmadas:** tipo de mundo configurable (inicialmente mapas planos con bordes naturales — ver conflicto **C-02**); geología/relieve coherentes; ríos/lagos por topografía; biomas por condiciones geográficas; ubicación de civilizaciones automática con recomendaciones; escala con presets + semillas; editor como módulo independiente.
- **Dependencias:** consume el modelo climático de Geografía y el de Recursos; provee el estado inicial del mundo a casi todos los sistemas.
- **Secciones:** 1 Visión · 2 Filosofía · 3 Tipos de mundo · 4 Geología/relieve · 5 Hidrología · 6 Clima/biomas · 7 Suelos · 8 Recursos · 9 Ubicación de civilizaciones · 10 Exploración/conocimiento · 11 Mapas históricos · 12 Escala · 13 Semillas · 14 Calidad/rendimiento · 15 Editor · 16 Config · 17 Integración · 18 Arquitectura de generación · 19 MVP · 20 Visión · 21 Decisiones.

### Geografía, clima y medioambiente
- **Ruta:** `docs/01-systems/GDD_Geografia_Clima_y_Medioambiente_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Base natural (geografía, clima, ecosistemas, recursos) que existe antes de las civilizaciones y condiciona su desarrollo.
- **Confirmadas:** todas las formaciones principales; topografía realista y continua (vs "mapas planos" de Generación — **C-02**); estaciones adaptadas por región (no impone 4); clima = variables meteorológicas relevantes; desastres incluidos pero **deshabilitados por defecto**; regeneración diferenciada renovables/no renovables; domesticación muy importante en etapas tempranas. Clima "principalmente visual al inicio" vs dependencias de estaciones (**C-03**).
- **Dependencias:** fuente del modelo climático (reusado por Generación); provee a Economía/Producción/Agricultura/Construcción/Población/Transporte/Tecnología/Cultura/Religión/Guerra.
- **Secciones (H1):** 1 Visión · 2 Geografía · 3 Topografía · 4 Ríos · 5 Océanos/costas · 6 Biomas · 7 Clima · 8 Estaciones · 9 Variabilidad climática · 10 Impacto humano · 11 Recursos/geología · 12 Regeneración/agotamiento · 13 Flora/fauna · 14 Domesticación · 15 Desastres · 16 Frecuencia desastres · 17 Conservación · 18 Geografía y asentamientos · 19 Generación/config · 20 Complejidad · 21 Integración · 22 MVP · 23 Decisiones · 24 Visión.

### Exploración y expansión territorial
- **Ruta:** `docs/01-systems/GDD_Exploracion_y_Expansion_Territorial_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Cómo las civilizaciones conocen, exploran, ocupan y controlan el mundo (expansión dirigida + crecimiento autónomo).
- **Confirmadas:** exploración configurable con estados de conocimiento; cartografía con niveles de precisión; control territorial mixto y simplificado; colonias con autonomía/independencia posibles; migraciones dirigidas y autónomas; asentamientos espontáneos con intervención del jugador.
- **Dependencias:** Geografía, Población, Economía, Transporte (reutiliza su logística), Tecnología, Diplomacia, Política, Cultura, Religión, Militar, Construcción.
- **Secciones:** 1 Visión · 2 Exploración · 3 Descubrimiento de regiones · 4 Cartografía · 5 Expansión · 6 Fundación de asentamientos · 7 Migración/colonización · 8 Administración/autonomía · 9 Autonomía de otras civilizaciones · 10 Integración · 11 Complejidad · 12 MVP · 13 Visión · 14 Decisiones.

### Tiempo y simulación
- **Ruta:** `docs/01-systems/GDD_Tiempo_y_Simulacion_v0.1.md` · **v0.1** · **Incompleto** (sin tabla de decisiones; el menos desarrollado).
- **Propósito:** Modelo de **tiempo continuo con reloj global único** y ciclos internos de frecuencia independiente (simular siglos sin procesar todo a la misma frecuencia).
- **Definiciones firmes:** reloj global único (los sistemas no tienen relojes propios); calendario configurable con consecuencias; velocidades pausa/normal/aceleradas sin alterar resultados; población híbrida; producción/edificios conservan estado sin actualización constante; pausa sin avances ocultos.
- **Dependencias:** transversal — reloj de referencia para TODOS los sistemas; consume estaciones/condiciones ambientales.
- **Secciones:** 1 Visión · 2 Reloj global · 3 Calendario · 4 Velocidad · 5 Ciclos internos · 6 Población/envejecimiento · 7 Producción/edificios · 8 Estaciones · 9 Acontecimientos · 10 Pausa · 11 Aceleración/rendimiento · 12 Config · 13 Integración · 14 MVP · 15 Principio rector.

---

## 2. Economía, recursos, producción, logística, construcción

### Recursos y materiales
- **Ruta:** `docs/01-systems/GDD_Recursos_y_Materiales_v0.1.md` · **v0.1** · **Incompleto** (sin tabla formal; reglas base en el cuerpo).
- **Propósito:** Catálogo **único y unificado** de recursos con identidad propia, compartido por todos los sistemas (sin duplicar materiales).
- **Reglas base:** sistema global sin duplicados; doble clasificación (naturaleza/categoría + función económica: materia prima/intermedio/final); unidad monetaria universal desde el inicio; titularidad general del Estado/civilización compatible con propiedad privada de edificios; unidades abstractas con escala consistente.
- **Dependencias:** transversal — lo consumen Economía, Producción, Construcción, Población, Transporte, Comercio, Tecnología, Agricultura, Geografía, Administración, Diplomacia, Guerra.
- **Secciones:** 1 Visión · 2 Clasificación · 3 Nivel de detalle · 4 Calidad/propiedades · 5 Recursos naturales · 6 Almacenamiento · 7 Deterioro · 8 Consumo · 9 Distribución geográfica · 10 Sustitución · 11 Propiedad · 12 Recursos estratégicos · 13 Dinero/moneda · 14 Unidades/contabilización · 15 Integración · 16 MVP · 17 Principio rector.

### Economía
- **Ruta:** `docs/01-systems/GDD_Economia_v0.2.md` · **v0.2** · **Parcial**.
- **Propósito:** Evolución económica de subsistencia a economías complejas: red de recursos, producción, consumo, logística, comercio, propiedad y finanzas.
- **Confirmadas:** propiedad mixta y evolutiva; asignación laboral automática con intervención; transporte físico desde el inicio (simplificado); población híbrida; complejidad configurable; comercio en tres niveles; evolución histórica flexible; simulación detallada (chico) / agregada (grande).
- **Pendientes:** fórmulas de productividad/salarios/precios; distribución automática; propiedad de edificios/tierras; integración económica entre asentamientos; instituciones financieras; balance (**C-07**).
- **Dependencias:** consume Recursos, Producción, Población, Tecnología, Medioambiente; provee precios/mercados/tesorería/impuestos; impacta social/político/cultural.
- **Secciones (top):** 1 Visión · 3 Recursos · 4 Producción · 5 Población/trabajo · 6 Consumo · 7 Agricultura/estaciones · 8 Almacenamiento/logística · 9 Mercados/precios · 10 Comercio · 11 Finanzas/moneda · 12 Propiedad · 13 Impuestos/políticas · 14 Etapas históricas · 16 Interfaz · 17 Simulación detallada/agregada · 18 MVP · 19 Decisiones · 20 Pendientes.

### Producción y trabajo
- **Ruta:** `docs/01-systems/GDD_Produccion_y_Trabajo_v0.2.md` · **v0.2** · **Parcial**.
- **Propósito:** Cómo se transforman recursos en bienes/servicios y cómo se organiza el trabajo; énfasis en **evolución visual y funcional de edificios**.
- **Confirmadas:** producción basada en edificios/procesos; cadenas interdependientes; asignación laboral automática con intervención; especialización (profesión + habilidades secundarias); recetas alternativas; el jugador fija prioridades y el edificio adapta; calidad gradual; mantenimiento evolutivo; propiedad mixta; ampliación modular; evolución de edificios por desencadenantes (tecnológicos/históricos/culturales…), con notificación visible; evolución **no obligatoria**.
- **Pendientes:** fórmulas de productividad; aprendizaje; salarios; calidad; autonomía de propietarios; condiciones de mejora; compatibilidad edificios antiguos↔nuevas tecnologías.
- **Dependencias:** consume Recursos, Población (trabajadores), Tecnología, Construcción, Logística; regulada por Política/Religión/Cultura/Medioambiente; provee bienes y especialización regional a Economía.
- **Secciones (top):** 1 Visión · 3 Edificios · 4 Recetas · 5 Cadenas · 6 Productividad · 7 Asignación laboral · 8 Profesiones/habilidades · 9 Relaciones laborales · 10 Métodos/tecnología · 11 Evolución de edificios · 12 Modularidad · 13 Calidad · 14 Herramientas/mantenimiento · 15 Sectores · 16 Escalas de producción · 17 Propiedad/control · 18 Integración · 19 Interfaz · 20 Complejidad · 21 Simulación · 22 MVP · 23 Decisiones · 24 Pendientes.

### Transporte, comercio y logística
- **Ruta:** `docs/01-systems/GDD_Transporte_Comercio_y_Logistica_v0.1.md` · **v0.1** · **Parcial**.
- **Propósito:** Conectar físicamente productores, consumidores, almacenes, mercados, ciudades, regiones y civilizaciones; del transporte manual a redes marítimas internacionales.
- **Confirmadas:** todo recurso se transporta físicamente; transporte propio + transportistas; medios/infraestructura históricos; caminos con consecuencias (distancia/terreno/clima/capacidad/mantenimiento); escalas local/regional/internacional; comercio en 3 niveles; precios dinámicos; rutas adaptables; comercio marítimo (flotas/puertos); comerciantes autónomos + intervención; logística militar integrada (sin sistema paralelo).
- **Pendientes:** costos/tiempos; algoritmos de rutas; congestión; mercados; flotas; rendimiento en mapas extensos.
- **Dependencias:** Economía, Producción, Construcción, Población, Tecnología, Política, Cultura, Religión, Diplomacia, Guerra, Medioambiente; provee la red logística y precios regionales.
- **Secciones (top):** 1 Visión · 2 Modalidades/asignación · 3 Medios · 4 Infraestructura · 5 Factores de transporte · 6 Logística urbana · 7 Escalas de comercio · 8 Precios · 9 Rutas · 10 Comercio marítimo · 11 Comerciantes · 12 Almacenamiento · 13 Problemas logísticos · (niveles de control) · 18 Interfaz · 19 Integración · 21 MVP · 22 Decisiones · 23 Futuras iteraciones.

### Construcción y arquitectura
- **Ruta:** `docs/01-systems/GDD_Construccion_y_Arquitectura_v0.1.md` · **v0.1** · **Parcial**.
- **Propósito:** Cómo el jugador construye, organiza, transforma y desarrolla físicamente los asentamientos a lo largo de la historia; la ciudad como testimonio del progreso.
- **Confirmadas:** construcción libre con cuadrícula opcional; rotación libre/por intervalos; obra con trabajadores y materiales; construcción por etapas; si faltan materiales la obra se detiene y busca alternativas; **evolución arquitectónica elegida por el jugador** (no automática); conservación de edificios históricos; abandono→deterioro→ruinas; estilos por cultura/geografía/época; clima principalmente visual al inicio (**C-03**); demolición/traslado con costos; crecimiento urbano mixto con barrios orgánicos.
- **Pendientes:** cálculo de belleza; valoración patrimonial; crecimiento orgánico; evolución arquitectónica por civilización; rendimiento de grandes ciudades; generación de ruinas.
- **Dependencias:** consume Recursos/materiales y Población (mano de obra); Tecnología habilita; Cultura/Religión dan estilo; Política regula; Medioambiente condiciona; provee el soporte físico a los demás sistemas.
- **Secciones (top):** 1 Visión · 2 Construcción libre · 3 Proceso de obra · 4 Evolución de edificios · 5 Módulos · 6 Materiales · 7 Arquitectura vertical · 8 Crecimiento urbano · 9 Belleza · 10 Infraestructura · 11 Estilos · 12 Terreno/clima · 13 Deterioro/ruinas · 14 Demolición/patrimonio · 15 Herramientas · 16 Cámara · 17 Escala · 18 Complejidad · 19 Integración · 20 MVP · 22 Decisiones · 23 Pendientes.

---

## 3. Población, sociedad, cultura, religión, política, administración

### Población y sociedad
- **Ruta:** `docs/01-systems/GDD_Poblacion_y_Sociedad_v0.1.md` · **v0.1** · **Parcial** (único con sección explícita de pendientes).
- **Propósito:** Cómo viven, se reproducen, trabajan, aprenden, se relacionan y evolucionan los habitantes; de comunidad a sociedad compleja.
- **Confirmadas:** información individual completa; protagonismo individual alto al inicio y decreciente; familias como entidades, hogares según cultura/época; natalidad/mortalidad multifactoriales; necesidades (básicas→sociales→culturales) con evolución histórica; felicidad individual/agregada; clases sociales con movilidad condicionada; salud poblacional con epidemias configurables; educación/alfabetización; migración interna y externa; representantes sociales; criminalidad agregada.
- **Pendientes (§21):** modelos matemáticos de necesidades/felicidad/demografía; linajes; movilidad social; instituciones; escalabilidad poblacional.
- **Dependencias:** Economía, Producción/trabajo, Construcción (vivienda), Tecnología, Política, Religión, Cultura, Medioambiente; provee el modelo poblacional/social.
- **Secciones:** 1 Principios · 2 Individuos · 3 Familias/demografía · 4 Necesidades · 5 Felicidad/calidad de vida · 6 Clases sociales · 7 Salud · 8 Educación · 9 Cultura · 10 Religión · 11 Migración · 12 Representantes/conflictos · 13 Criminalidad/seguridad · 14 Evolución histórica · 15 Interfaz · 16 Complejidad · 17 Jugador vs sociedad · 18 Integración · 20 Decisiones · 21 Pendientes.

### Cultura, identidad y tradiciones
- **Ruta:** `docs/01-systems/GDD_Cultura_Identidad_y_Tradiciones_v0.1.md` · **v0.1** · **Confirmado** (sin sección de pendientes).
- **Propósito:** Personalidad de la civilización (idiomas, gastronomía, vestimenta, arte, arquitectura, costumbres, festividades, tradiciones) que evoluciona con la civilización.
- **Confirmadas:** evolución cultural orgánica e influenciable; identidad general/regional/local; intercambio por comercio/migraciones/guerras/diplomacia; fusión progresiva no destructiva; idiomas evolucionan/se dividen/mezclan/desaparecen; modas; **evolución visual fundamental y visible** en el mundo.
- **Dependencias:** Población, Religión, Política, Construcción (expresión física), Economía, Educación, Tecnología, Diplomacia, Migración.
- **Secciones:** 1 Visión · 2 Componentes · 3 Identidad · 4 Geografía · 5 Intercambio · 6 Fusión · 7 Idioma · 8 Modas · 9 Jugador · 10 Evolución visual · 11 Tradiciones/patrimonio · 12 Clases sociales · 13 Religión · 14 Config · 15 Integración · 16 MVP · 17 Decisiones · 18 Visión.

### Religión y espiritualidad
- **Ruta:** `docs/01-systems/GDD_Religion_y_Espiritualidad_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Creencias, prácticas, instituciones y tradiciones espirituales como sistemas vivos que aparecen, evolucionan, se expanden, se dividen y desaparecen.
- **Confirmadas:** origen orgánico + creación por el jugador; todas las creencias históricas; influencia social muy importante en épocas tempranas; convivencia pacífica o conflictiva según contexto; instituciones con propiedades; arquitectura religiosa monumental; figuras religiosas identificables; reformas/divisiones; complejidad configurable.
- **Dependencias:** Población, Política, Cultura, Construcción, Economía, Educación, Tecnología, Diplomacia.
- **Secciones:** 1 Visión · 2 Origen · 3 Tipos de creencias · 4 Evolución · 5 Influencia social · 6 Convivencia · 7 Instituciones · 8 Arquitectura · 9 Figuras · 10 Vida cotidiana · 11 Reformas/divisiones · 12 Secularización · 13 Jugador · 14 Config · 15 Integración · 16 MVP · 17 Decisiones · 18 Visión.

### Política y gobierno
- **Ruta:** `docs/01-systems/GDD_Politica_y_Gobierno_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Cómo las sociedades organizan el poder, deciden, legislan y administran. El jugador es **entidad externa que guía** (no gobernante) con poder siempre amplio.
- **Confirmadas:** rol del jugador = entidad superior que guía; poder siempre amplio (ver conflicto **C-05**); todas las formas de gobierno históricas; cambios políticos con consecuencias; instituciones existen pero el jugador conserva control; leyes modificables; líderes identificables; conflictos configurables; evolución progresiva y no lineal.
- **Dependencias:** transversal — Población, Economía, Religión, Cultura, Producción, Construcción, Tecnología, Militar.
- **Secciones:** 1 Visión · 2 Papel del jugador · 3 Formas de gobierno · 4 Evolución política · 5 Instituciones · 6 Leyes/políticas · 7 Consejeros/grupos de poder · 8 Oposición · 9 Líderes · 10 Conflictos · 11 Config · 12 Integración · 13 MVP · 14 Decisiones · 15 Visión.

### Administración territorial y regiones
- **Ruta:** `docs/01-systems/GDD_Administracion_Territorial_y_Regiones_v0.1.md` · **v0.1** · **Confirmado** (simplificado; el más breve).
- **Propósito:** Organizar y administrar territorios a medida que la civilización crece, sin controlar cada asentamiento individualmente (decisiones centrales + gestión autónoma).
- **Confirmadas:** ciudades agrupadas en regiones; administración mixta; identidad regional orgánica; autonomía configurable; impuestos generales y diferenciados; relaciones de cooperación/competencia; evolución automática e influenciable; complejidad configurable con automatización.
- **Dependencias:** usa Economía/Producción/Social/Construcción existentes sin duplicar; fuertemente ligado a Política.
- **Secciones:** 1 Visión · 2 Organización · 3 Administración regional · 4 Gestión · 5 Identidad/especialización · 6 Autonomía · 7 Impuestos/economía regional · 8 Relaciones entre regiones · 9 Evolución histórica · 10 Distancia · 11 Complejidad · 12 MVP · 13 Decisiones.

---

## 4. Tecnología, militar, diplomacia, IA, eventos, progresión, interfaz, entidades

### Tecnología, ciencia e innovación
- **Ruta:** `docs/01-systems/GDD_Tecnologia_Ciencia_e_Innovacion_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Desarrollo de conocimientos e invenciones como consecuencia orgánica de la historia (no árbol tecnológico tradicional).
- **Confirmadas:** modelo híbrido (orgánico + dirigido); el jugador fija prioridades y financia, especialistas investigan; todas las disciplinas; transmisión familiar/artesanal/educativa/institucional/internacional; pérdida de conocimiento excepcional; evolución simultánea en paralelo; innovadores históricos; obsolescencia con coexistencia; caminos diversos; estancamiento posible; **evolución visual fundamental** conectada a Producción y Construcción.
- **Dependencias:** Economía, Producción, Construcción, Población, Cultura, Religión, Política, Medioambiente, Transporte, Militar, Diplomacia.
- **Secciones:** 1 Visión · 2 Modelo de progreso · 3 Niveles de conocimiento · 4 Disciplinas · 5 Origen de descubrimientos · 6 Personajes · 7 Instituciones · 8 Transmisión/conservación · 9 Intercambio · 10 Evolución de disciplinas · 11 Evolución simultánea · 12 Control del jugador · 13 Ciencia vs tecnología · 14 Educación · 15 Economía e innovación · 16 Medioambiente · 17 Obsolescencia · 18 Diversidad de caminos · 19 Estancamiento · 20 Tecnología militar · 21 Evolución visual · 22 Interfaz · 23 Config · 24 Alcance histórico · 25 Integración · 26 MVP · 27 Decisiones · 28 Visión.

### Guerra y sistemas militares
- **Ruta:** `docs/01-systems/GDD_Guerra_y_Sistemas_Militares_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Evolución histórica de organización militar, tecnologías bélicas, tácticas, fortificaciones y conflictos; fuerza transformadora pero **secundaria**.
- **Confirmadas:** organización configurable; categorías históricas de fuerzas; reclutamiento civil/profesional por época; combate configurable (automático/táctico/detallado); moral importante no determinante; logística y economía militar simplificadas; causas/inicio/fin configurables; fortificaciones con evolución visual; diplomacia muy importante para prevenir; consecuencias amplias; complejidad por módulos.
- **Dependencias:** usa Transporte/Logística generales; Población (reclutamiento); Construcción (fortificaciones); integra Diplomacia; provee consecuencias a economía/población/política/cultura/territorio.
- **Secciones:** 1 Visión · 2 Organización · 3 Reclutamiento · 4 Evolución militar · 5 Comandantes · 6 Combate · 7 Logística · 8 Tipos de conflicto · 9 Causas/inicio · 10 Diplomacia/prevención · 11 Finalización · 12 Fortificaciones · 13 Guerra naval · 14 Consecuencias · 15 Autonomía militar · 16 Inteligencia · 17 Config · 18 MVP · 19 Visión · 20 Decisiones.

### Diplomacia y relaciones internacionales ⚠️
- **Ruta:** `docs/01-systems/GDD_Diplomacia_y_Relaciones_Internacionales_v0.1.md` · **v0.1** · **TRUNCADO** (el archivo fuente se corta a mitad de la sección 18 "Personalidad de los líderes"; sin MVP ni tabla de decisiones). **Registrar para completar.**
- **Propósito:** Civilizaciones que interactúan, cooperan, compiten, negocian y entran en conflicto mediante relaciones multidimensionales con memoria histórica y evolución autónoma.
- **Definiciones firmes (§1, principios):** relaciones multidimensionales (no una sola puntuación); memoria histórica; autonomía de civilizaciones; intervención amplia del jugador; cooperación/resolución pacífica; integración con economía/política/cultura/religión/tecnología; negociaciones en 3 niveles; espionaje y organizaciones internacionales configurables.
- **Dependencias:** integra Religión (§13); vínculos con economía/política/cultura/tecnología; relación con Militar; provee tratados, memoria histórica, prestigio, estados dependientes.
- **Secciones (H1, hasta el corte):** 1 Visión · 2 Modelo de relaciones · 3 Memoria histórica · 4 Evolución histórica · 5 Embajadores · 6 Tratados · 7 Duración/cumplimiento · 8 Negociaciones · 9 Objetivos/intereses · 10 Fronteras/disputas · 11 Influencia/prestigio · 12 Espionaje · 13 Religión · 14 Cultura · 15 Organizaciones internacionales · 16 Estados dependientes · 17 Diplomacia/guerra/paz · 18 Personalidad de líderes (cortada).

### IA y autonomía de civilizaciones
- **Ruta:** `docs/01-systems/GDD_IA_y_Autonomia_Civilizaciones_v0.1.md` · **v0.1** · **Incompleto** (sin tabla; principios + criterios de aceptación).
- **Propósito:** Que las civilizaciones funcionen como sociedades autónomas con objetivos, personalidades, líderes e historia propia; el mundo evoluciona sin el jugador.
- **Definiciones firmes:** autonomía configurable global y por civilización; identidad estratégica con tendencias no rígidas; IA **sin** información no descubierta ni conocimiento del futuro; tres niveles de simulación por distancia/importancia; nacimiento/desaparición posibles pero no frecuentes; dificultad = calidad de decisiones + ventajas opcionales, **sin IA omnisciente**; usa las **mismas reglas** que el jugador.
- **Dependencias:** Economía, Población, Construcción, Tecnología, Cultura/Religión, Diplomacia, Exploración, Administración, Guerra, Eventos.
- **Secciones:** 1 Visión · 2 Autonomía · 3 Personalidad/objetivos · 4 Decisiones · 5 Líderes · 6 Competencia/cooperación · 7 Evolución diferenciada · 8 Simulación por distancia · 9 Nacimiento/desaparición · 10 Influencia del jugador · 11 Dificultad · 12 Integración · 13 Memoria/conocimiento · 14 Config · 15 Interfaz · 16 MVP · 17 Principio rector.

### Eventos y evolución histórica
- **Ruta:** `docs/01-systems/GDD_Eventos_y_Evolucion_Historica_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Generar, conectar, registrar y representar los acontecimientos que construyen una historia emergente y única por partida.
- **Confirmadas:** generación mixta (histórico/dinámico/aleatorio) con aleatoriedad moderada; cadenas de consecuencias simplificadas; balance con más positivos relativos; personajes históricos con biografías (importantes); registro completo + enciclopedia resumida; huellas históricas en el mundo; intervención directa del jugador; evolución histórica libre; otras civilizaciones con historia autónoma; complejidad por módulos.
- **Dependencias:** transversal — recibe hechos de todos los sistemas y los registra; reutiliza arquitectura compartida del GDD de Tecnología. **No debe crear efectos de juego sin regla propietaria** (Maestro §6).
- **Secciones:** 1 Visión · 2 Generación · 3 Categorías · 4 Balance · 5 Cadenas · 6 Personajes · 7 Memoria histórica · 8 Huellas · 9 Jugador · 10 Ritmo · 11 Presentación · 12 Cronología · 13 Config · 14 MVP · 15 Integración · 16 Visión · 17 Decisiones.

### Progresión y finalización
- **Ruta:** `docs/01-systems/GDD_Progresion_y_Finalizacion_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Cómo progresa una partida, qué objetivos persigue el jugador y cómo finaliza; prioriza experiencia **sandbox** sin secuencia obligatoria.
- **Confirmadas:** progresión por períodos orientativos y evolución libre; objetivos configurables y desactivables (sandbox auténtico); puntuación general opcional + indicadores; dificultad adaptativa transparente (no tramposa); **límite histórico opcional** (inicio Revolución Industrial, no desarrollada — **C-01**); continuación posible; decisión final del jugador; legado histórico opcional.
- **Dependencias:** consume indicadores de Economía/Población/Tecnología/Cultura/Religión/Diplomacia/Territorio/Construcción; legado se nutre de Eventos.
- **Secciones:** 1 Visión · 2 Progresión histórica · 3 Objetivos · 4 Puntuación/prestigio · 5 Objetivos personalizados · 6 Dificultad · 7 Desafíos/escenarios · 8 Duración/finalización · 9 Fin del período histórico · 10 Legado · 11 Complejidad · 12 MVP · 13 Decisiones.

### Interfaz UI/UX
- **Ruta:** `docs/01-systems/GDD_Interfaz_UI_UX_v0.1.md` · **v0.1** · **Confirmado**.
- **Propósito:** Interfaz moderna, contextual y adaptable; el **mundo 3D es la interfaz principal**, menús/paneles al servicio de la experiencia.
- **Confirmadas:** estilo moderno/contextual; interacción con mundo 3D primero; info contextual + global; mapas temáticos muy importantes; notificaciones configurables; tutoriales inicial y contextual; personalización completa; **accesibilidad prioridad alta**; complejidad adaptable (simple/intermedio/avanzado). La UI **lee** el estado de simulación, no lo posee.
- **Dependencias:** presenta datos de casi todos los sistemas; no define simulación propia.
- **Secciones:** 1 Visión · 2 Estilo · 3 Interacción con el mundo · 4 Información/estadísticas · 5 Navegación · 6 Mapas/indicadores · 7 Notificaciones · 8 Tutoriales · 9 Personalización · 10 Info detallada · 11 Accesibilidad · 12 Adaptación a complejidad · 13 MVP · 14 Decisiones.

### Entidades y relaciones
- **Ruta:** `docs/01-systems/GDD_Entidades_y_Relaciones_v0.1.md` · **v0.1** · **Incompleto** (sin tabla; 8 principios + criterios de aceptación).
- **Propósito:** Entidades fundamentales (habitantes, familias, edificios, asentamientos, regiones, organizaciones), atributos, relaciones y evolución — **base de datos compartida** por todos los sistemas.
- **Definiciones firmes (§2):** identidad propia; relaciones múltiples simultáneas que cambian/desaparecen/reaparecen; memoria histórica en relaciones importantes; entidades que evolucionan/dividen/fusionan/desaparecen; sistemas comparten entidades sin duplicar; identificadores persistentes; complejidad configurable. Titularidad (§13): recursos naturales del Estado/civilización mientras privados operan.
- **Dependencias:** base consumida por todos los sistemas; integra Administración Territorial.
- **Secciones:** 1 Objetivo · 2 Principios · 3 Habitantes · 4 Familias/hogares · 5 Edificios · 6 Asentamientos · 7 Regiones · 8 Organizaciones · 9 Relaciones · 10 Identidad/historia persistente · 11 Escalabilidad · 12 Integración · 13 Propiedad/titularidad · 14 Interfaz/consulta · 15 Complejidad · 16 MVP · 17 Criterios de aceptación · 18 Principio rector.

---

## 5. Documentos del proyecto UE (condensados / operativos)

- `civilizations-evolution/Docs/00_TECHNICAL.md` — **idéntico** al plan técnico (ver arriba).
- `civilizations-evolution/Docs/01_GDD_GENERAL.md` — GDD general original (ver sección 0).
- `civilizations-evolution/Docs/02_GDD_ECONOMIA.md` — versión condensada del GDD de Economía usada para guiar el primer código.
- `civilizations-evolution/Docs/03_GDD_PRODUCCION_TRABAJO.md` — condensado de Producción y Trabajo.
- `civilizations-evolution/Docs/04_GDD_CONSTRUCCION.md` — condensado de Construcción.
- `civilizations-evolution/HANDOFF.md` — traspaso para compilar en UE5 (contexto de implementación).
- `civilizations-evolution/README.md` y `Source/README_UE5.md` — puesta en marcha del proyecto UE.
- `civilizations-evolution/Docs/ASSETS.md` — investigación de assets CC0/Fab/Mixamo (mantenida por la sesión paralela de assets).

## 6. Contradicciones y pendientes (resumen; detalle en AUDIT_REPORT.md)

- **C-01** fecha de corte (≈1800 vs "inicio Revolución Industrial").
- **C-02** "mapas planos" (Generación) vs "topografía realista y continua" (Geografía).
- **C-03** clima "principalmente visual" (Construcción) vs dependencias estacionales (Geografía/Agricultura).
- **C-04** nombre del proyecto ("Historical Civilization Simulator" vs "Civilization Evolutions").
- **C-05** rol del jugador (entidad externa no gobernante vs capacidad de legislar/dirigir).
- **C-06** límites de escala (población/mapa/civilizaciones) sin definir.
- **C-07** propiedad/precios/fiscalidad: dirección confirmada pero reglas abiertas.
- **C-08** listado "pendiente" del GDD general quedó obsoleto frente a los GDD especializados.
- **Diplomacia truncada** en la fuente (ver arriba).
