> **Control documental:** 0.1 | Estado: diseño conceptual; las decisiones marcadas como confirmadas se preservan desde la fuente. **Alcance:** este GDD es fuente de detalle de su dominio; el GDD Maestro establece la visión común. Los conflictos intersistema se registran en AUDIT_REPORT.md y no se resuelven implícitamente.

---

# GDD — GENERACIÓN PROCEDURAL DEL MUNDO
**Versión:** 0.1  
**Estado:** Definición conceptual consolidada  
**Proyecto:** Simulador histórico de civilizaciones  
**Período jugable:** Desde el Paleolítico hasta el comienzo de la Revolución Industrial.

---

## 1. Visión general

El sistema de Generación Procedural del Mundo será responsable de crear los escenarios geográficos donde se desarrollarán las civilizaciones.

El objetivo es generar mundos visualmente atractivos, geográficamente coherentes y suficientemente diversos para que cada partida tenga una identidad propia.

La generación debe respetar principios geológicos, climáticos, hidrológicos y ecológicos. Sin embargo, el realismo no debe limitar la libertad del jugador: los parámetros de generación serán ampliamente configurables.

El sistema debe permitir crear desde mapas relativamente pequeños con varios continentes hasta enormes mundos con archipiélagos, supercontinentes, grandes océanos y regiones extremas.

También deberá permitir utilizar geografías históricas reales y modificar el mundo antes o durante una partida.

**Principio fundamental:**

El mundo no será un escenario decorativo. Será un sistema dinámico que condicionará la historia, la economía, la cultura, la tecnología y las posibilidades de desarrollo de cada civilización.

## 2. Filosofía de generación

El sistema tendrá cuatro principios:

1. **Coherencia natural:** la geografía debe responder a relaciones lógicas entre relieve, agua, clima, suelos, biomas y recursos.
2. **Configurabilidad:** prácticamente todos los parámetros relevantes podrán modificarse.
3. **Diversidad:** no todos los mundos deben ser equilibrados ni ofrecer las mismas oportunidades.
4. **Reproducibilidad:** una misma configuración y semilla deben permitir generar el mismo mundo.

La generación no debe seguir un procedimiento puramente aleatorio. Cada etapa utilizará información de las anteriores para producir resultados coherentes.

## 3. Tipos de mundo y distribución geográfica

### 3.1. Formato del mundo

Se utilizará un sistema configurable que inicialmente priorizará mapas planos con bordes naturales.

Se contemplan diferentes posibilidades:

- Mapas con límites naturales y geografía irregular.
- Mapas con continentes y océanos.
- Mapas de islas y archipiélagos.
- Mapas fragmentados.
- Mapas con grandes masas continentales.
- Mapas con una proporción elevada de océanos.
- Otros formatos que puedan incorporarse posteriormente.

La arquitectura deberá permitir evaluar futuras alternativas, incluida la representación de mundos esféricos, sin que esto sea un requisito del MVP.

### 3.2. Distribución de masas terrestres

Todos los parámetros serán configurables:

- Cantidad de continentes.
- Tamaño y forma de cada continente.
- Cantidad de islas.
- Tamaño de los archipiélagos.
- Distribución de las masas terrestres.
- Distancia entre continentes.
- Proporción de tierra y agua.
- Existencia de supercontinentes.
- Fragmentación geográfica.
- Distribución de grandes océanos y mares interiores.

La generación debe producir configuraciones naturales y variadas, evitando patrones artificiales demasiado evidentes.

## 4. Geología y formación del relieve

### 4.1. Principios geológicos

El relieve se generará mediante procesos geológicos simulados de manera simplificada, pero coherente.

Se contemplarán:

- Formación de cordilleras.
- Elevaciones y depresiones.
- Cuencas.
- Mesetas.
- Valles.
- Llanuras.
- Colinas.
- Volcanes.
- Fallas y zonas geológicas.
- Regiones montañosas costeras.

No se requiere simular la tectónica de placas en tiempo real. El objetivo es reproducir resultados geográficos plausibles.

La intensidad y variedad de estos elementos podrán configurarse.

### 4.2. Relieve y topografía

El mundo tendrá una topografía continua, no una colección de zonas aisladas.

Cada región podrá presentar diferentes elevaciones y pendientes.

La generación deberá considerar:

- Altitud.
- Pendiente.
- Orientación del terreno.
- Valles y pasos naturales.
- Barreras montañosas.
- Cuencas hidrográficas.
- Llanuras y mesetas.
- Transiciones entre diferentes tipos de relieve.

El relieve tendrá consecuencias directas en construcción, agricultura, transporte, asentamientos, defensa y expansión territorial.

## 5. Hidrología: ríos, lagos y océanos

### 5.1. Ríos

Los ríos se generarán a partir de la topografía y las condiciones climáticas.

El sistema contemplará:

- Nacimientos en montañas, manantiales y zonas húmedas.
- Afluentes.
- Cuencas hidrográficas.
- Cursos principales y secundarios.
- Desembocaduras.
- Deltas.
- Estuarios.
- Variaciones estacionales del caudal.
- Inundaciones.
- Navegabilidad según profundidad, caudal y características del curso.

Los ríos deberán seguir trayectorias geográficamente coherentes, evitando cursos que ignoren elevaciones y pendientes.

### 5.2. Lagos y humedales

Se generarán lagos de distintos tamaños y características:

- Lagos de montaña.
- Lagos glaciares representados como accidentes geográficos.
- Lagos de cuenca.
- Lagos volcánicos.
- Lagos conectados a ríos.
- Lagos sin salida al mar.
- Pantanos y humedales.

Los lagos podrán conectarse con sistemas fluviales y tener consecuencias sobre asentamientos, pesca, agricultura y transporte.

### 5.3. Mares y costas

Se contemplarán:

- Océanos.
- Mares interiores.
- Golfos.
- Bahías.
- Penínsulas.
- Estuarios.
- Fiordos.
- Deltas.
- Arrecifes.
- Costas rocosas.
- Playas.
- Islas costeras.

El relieve submarino también tendrá representación, incluyendo profundidades variables y plataformas continentales.

Esto permitirá que las costas tengan características diferentes y afecten la navegación, la pesca, los puertos y el comercio marítimo.

## 6. Clima y biomas

### 6.1. Generación climática

El clima estará determinado principalmente por:

- Latitud.
- Altitud.
- Temperatura.
- Precipitaciones.
- Humedad.
- Cercanía al mar.
- Relieve.
- Vientos y condiciones regionales.

Se contemplarán variaciones estacionales y regionales.

No todas las regiones tendrán cuatro estaciones. Por ejemplo, las regiones tropicales podrán presentar estaciones húmedas y secas, mientras que las regiones polares tendrán dinámicas propias.

El sistema aprovechará el modelo climático ya definido en el GDD de Geografía, Clima y Medioambiente.

### 6.2. Biomas

Los biomas se distribuirán considerando las condiciones climáticas y geográficas.

Se incluirán:

- Bosques templados.
- Bosques boreales y taigas.
- Selvas tropicales.
- Sabanas.
- Praderas.
- Estepas.
- Desiertos cálidos.
- Desiertos fríos.
- Tundras.
- Humedales.
- Regiones mediterráneas.
- Regiones alpinas.

Los biomas tendrán transiciones naturales. No deben aparecer como bloques geométricos perfectamente delimitados.

La relación entre clima y biomas será realista hasta cierto punto, permitiendo configuraciones especiales y excepciones controladas.

### 6.3. Flora y fauna

Cada bioma podrá incluir especies y recursos naturales característicos:

- Árboles y vegetación.
- Plantas alimenticias.
- Plantas medicinales.
- Animales herbívoros.
- Depredadores.
- Fauna migratoria.
- Especies domesticables.
- Recursos naturales particulares.

La fauna y la flora deberán integrarse con el sistema de medioambiente, economía y domesticación.

## 7. Suelos y fertilidad

Los suelos tendrán características propias que influirán en las posibilidades productivas.

Se contemplarán:

- Suelos aluviales.
- Suelos volcánicos.
- Suelos fértiles.
- Suelos pobres.
- Suelos arenosos.
- Suelos arcillosos.
- Suelos montañosos.
- Suelos húmedos.
- Suelos áridos.

Cada región podrá tener distintos niveles de fertilidad, capacidad de retención de agua y aptitud para determinados cultivos.

La fertilidad influirá directamente en:

- Rendimiento agrícola.
- Ganadería.
- Selección de cultivos.
- Necesidad de irrigación.
- Técnicas de explotación.
- Valor económico de los territorios.

La degradación y recuperación de los suelos estarán vinculadas al sistema medioambiental.

## 8. Recursos naturales

### 8.1. Distribución geológica y ambiental

Los recursos se distribuirán según condiciones geológicas, climáticas y ecológicas.

Se contemplarán:

- Minerales metálicos.
- Piedra y materiales de construcción.
- Carbón y otros recursos energéticos preindustriales.
- Madera.
- Recursos forestales.
- Agua.
- Recursos pesqueros.
- Fauna.
- Plantas y productos silvestres.
- Recursos agrícolas potenciales.
- Otros recursos naturales que puedan incorporarse.

La distribución no será uniforme ni completamente aleatoria.

### 8.2. Abundancia y escasez

El jugador podrá configurar:

- Abundancia general.
- Abundancia mineral.
- Recursos forestales.
- Fertilidad de los suelos.
- Recursos pesqueros.
- Disponibilidad de agua.
- Recursos raros.

Los recursos podrán estar concentrados en determinadas regiones, generando oportunidades económicas y diferencias entre civilizaciones.

Se respetará el modelo de recursos ya establecido: los minerales son limitados y los recursos renovables también pueden agotarse mediante sobreexplotación.

## 9. Ubicación inicial de civilizaciones

### 9.1. Generación automática

Las civilizaciones iniciales aparecerán automáticamente en ubicaciones geográficas apropiadas.

El sistema evaluará:

- Disponibilidad de agua.
- Acceso a alimentos.
- Fertilidad del suelo.
- Clima.
- Recursos naturales.
- Protección geográfica.
- Posibilidades de expansión.
- Acceso a rutas terrestres y marítimas.

La ubicación inicial será una recomendación generada por el sistema, no una garantía de éxito.

### 9.2. Configuración

El jugador podrá elegir:

- Distribución aleatoria.
- Distribución en regiones habitables.
- Distribución apropiada con diversidad geográfica.
- Ubicación manual.
- Combinaciones de ubicación automática y manual.

El sistema podrá recomendar ubicaciones iniciales y explicar sus ventajas y desventajas.

### 9.3. Equilibrio geográfico

El equilibrio entre civilizaciones será opcional.

El jugador podrá crear mundos donde todas las civilizaciones tengan condiciones iniciales relativamente similares o mundos muy desiguales.

No se deberá forzar una distribución equitativa de recursos, fertilidad, clima o accesibilidad.

Las desigualdades geográficas podrán generar historias diferentes y desafíos particulares.

## 10. Exploración y conocimiento del mundo

El sistema tendrá tres estados diferenciados:

1. **Desconocido:** la civilización no tiene información suficiente sobre la región.
2. **Conocido:** existe información general sobre el territorio, obtenida por exploración, relatos, contactos o cartografía.
3. **Explorado y cartografiado:** la región fue investigada y existe información geográfica más precisa.

El nivel de conocimiento dependerá de las opciones de partida y de la tecnología disponible.

El jugador podrá configurar:

- Mundo completamente visible.
- Solo territorio inicial conocido.
- Territorio inicial y regiones cercanas.
- Exploración progresiva.

La exploración se integrará con los sistemas de población, tecnología, transporte y diplomacia.

## 11. Mapas históricos

El sistema permitirá utilizar geografías basadas en regiones reales.

Se contemplarán mapas de:

- Europa.
- Mediterráneo.
- Medio Oriente.
- Norte de África.
- Asia.
- África.
- América.
- Otras regiones históricas.

Estos mapas deberán representar geografías reconocibles y permitir diferentes configuraciones de civilizaciones iniciales.

Los mapas históricos no obligarán a reproducir acontecimientos históricos. Serán escenarios geográficos para desarrollar historias alternativas.

La precisión geográfica y el nivel de detalle podrán variar según el escenario.

## 12. Escala y dimensiones del mundo

El tamaño será configurable.

Se contemplarán:

- Superficie total.
- Resolución geográfica.
- Cantidad de continentes.
- Cantidad de islas.
- Distancia entre masas terrestres.
- Proporción de tierra y océanos.
- Cantidad de regiones.
- Cantidad de civilizaciones.
- Densidad de recursos.
- Nivel de detalle.

Se ofrecerán configuraciones preestablecidas y opciones avanzadas.

La arquitectura deberá contemplar mapas de gran escala sin exigir que toda la geografía tenga el mismo nivel de detalle o simulación.

## 13. Semillas y reproducibilidad

Cada generación podrá utilizar una semilla (*seed*).

Esto permitirá:

- Reproducir exactamente un mundo.
- Compartir mundos entre jugadores.
- Comparar diferentes configuraciones.
- Volver a generar escenarios.
- Realizar pruebas de balance.
- Facilitar pruebas técnicas.

La misma semilla y configuración deberán producir resultados reproducibles.

## 14. Calidad visual y rendimiento

La calidad visual tendrá prioridad alta, sin perder de vista la posibilidad de generar mapas grandes.

Se buscará representar:

- Relieves naturales.
- Montañas y valles reconocibles.
- Costas variadas.
- Ríos y lagos integrados en el terreno.
- Transiciones entre biomas.
- Vegetación característica.
- Diferencias visuales entre regiones.

La geografía deberá ser visualmente convincente desde diferentes alturas y escalas.

El sistema utilizará niveles de detalle y estrategias de optimización para evitar que el tamaño del mundo comprometa el rendimiento.

La representación visual y la simulación geográfica deberán estar desacopladas: una región podrá conservar una simulación simplificada cuando no se encuentre dentro del área visual del jugador.

## 15. Editor de mundos

El editor de mundos será un módulo independiente.

Permitirá modificar el mundo antes de iniciar una partida.

Entre las herramientas previstas:

- Crear o modificar montañas.
- Elevar o reducir terrenos.
- Modificar valles y mesetas.
- Crear y eliminar lagos.
- Modificar ríos.
- Crear islas y archipiélagos.
- Modificar costas.
- Cambiar biomas.
- Modificar recursos.
- Cambiar la ubicación inicial de civilizaciones.

El editor deberá permitir regenerar determinadas capas cuando sea posible, sin tener que reconstruir todo el mundo.

Por ejemplo, modificar un río podría requerir actualizar la hidrología y ciertos elementos relacionados, pero no necesariamente regenerar la distribución de continentes.

### 15.1. Modificación durante la partida

El jugador también tendrá algunas herramientas para modificar el terreno durante una partida.

Estas opciones no reemplazarán al editor completo.

Deberán integrarse con los sistemas de construcción, economía y medioambiente, contemplando las consecuencias de cada intervención.

Algunos ejemplos:

- Modificar terrenos para construir.
- Crear canales.
- Drenar determinados humedales.
- Modificar cursos de agua mediante obras.
- Construir infraestructura hidráulica.
- Realizar intervenciones de ingeniería sobre el relieve.

Las modificaciones tendrán requisitos tecnológicos, materiales, costos y consecuencias ambientales cuando corresponda.

La disponibilidad dependerá de la época y de los conocimientos de la civilización.

## 16. Configuración y complejidad

Se ofrecerán tres niveles principales:

**Sencillo**
- Generación automática.
- Configuraciones preestablecidas.
- Parámetros básicos.
- Exploración simplificada.
- Pocas opciones de intervención.

**Intermedio**
- Configuración de continentes, recursos, biomas y civilizaciones.
- Control de abundancia y fertilidad.
- Configuración de exploración.
- Opciones adicionales de generación.

**Avanzado**
- Control detallado de los parámetros geográficos.
- Configuración de recursos y distribución.
- Ubicación manual de civilizaciones.
- Herramientas avanzadas de edición.
- Mayor control sobre los procesos de generación.

Los módulos también podrán habilitarse o deshabilitarse individualmente cuando sea posible.

## 17. Integración con otros sistemas

La generación procedural no será un sistema aislado.

La información geográfica será utilizada por los demás sistemas del juego.

| Sistema | Información que recibe |
|---|---|
| Construcción y arquitectura | Relieve, pendientes, suelos, agua y materiales |
| Economía | Recursos, fertilidad, accesibilidad y distancias |
| Producción y trabajo | Recursos disponibles y condiciones regionales |
| Transporte y logística | Relieve, ríos, costas, caminos y obstáculos |
| Población y sociedad | Clima, alimentos, agua y condiciones de habitabilidad |
| Tecnología | Recursos, necesidades y oportunidades de desarrollo |
| Cultura e identidad | Biomas, entorno, recursos y aislamiento geográfico |
| Religión | Paisajes, lugares naturales y características regionales |
| Política y gobierno | Territorios, fronteras naturales y recursos |
| Diplomacia | Accesibilidad, proximidad y conexiones entre civilizaciones |
| Medioambiente | Ecosistemas, especies, recursos y condiciones climáticas |
| Guerra | Terreno, pasos, ríos, costas y posiciones estratégicas |

La geografía también condicionará el desarrollo histórico de cada civilización.

Por ejemplo, una civilización ubicada en una región montañosa tendrá oportunidades y limitaciones diferentes a otra asentada en una gran llanura fértil o en un archipiélago.

## 18. Arquitectura conceptual de generación

La generación estará organizada en capas dependientes.

**Orden conceptual:**

1. Distribución de masas terrestres y océanos.
2. Geología y relieve.
3. Hidrología.
4. Clima.
5. Biomas.
6. Suelos y fertilidad.
7. Recursos naturales.
8. Ubicaciones iniciales de civilizaciones.
9. Exploración y conocimiento inicial.

Cada capa utilizará información de las anteriores.

Esto permitirá mantener coherencia y facilitar la modificación parcial del mundo.

Por ejemplo, cambiar la distribución de continentes requerirá regenerar buena parte de las capas posteriores, mientras que modificar la abundancia de minerales no debería obligar a reconstruir la topografía.

La implementación técnica concreta queda pendiente de las decisiones de arquitectura y rendimiento.

## 19. MVP — Primera versión jugable

El MVP no necesita implementar toda la complejidad geográfica.

Debe permitir generar un mundo funcional sobre el que puedan desarrollarse los primeros sistemas de civilización.

### Funcionalidades iniciales

- Generación procedural de mapas planos con bordes naturales.
- Distribución de masas terrestres y océanos.
- Elevación y relieve básico.
- Llanuras, colinas y montañas.
- Lagos y ríos básicos.
- Biomas iniciales.
- Temperatura y estaciones.
- Distribución de recursos.
- Fertilidad básica del terreno.
- Ubicaciones iniciales de civilizaciones.
- Configuración de semilla.
- Parámetros básicos de tamaño y distribución.

### Fuera del MVP

- Mapas históricos detallados.
- Geología avanzada.
- Relieve submarino complejo.
- Simulación hidrológica avanzada.
- Gran variedad de especies.
- Editor completo de mundos.
- Herramientas avanzadas de modificación durante la partida.
- Configuración geográfica extremadamente detallada.

Estos elementos deberán contemplarse desde el diseño para evitar tener que reconstruir el sistema posteriormente.

## 20. Visión final

El objetivo final es que cada mundo tenga una geografía propia, con oportunidades, limitaciones y características que influyan de manera profunda en el desarrollo de las civilizaciones.

Un mundo con grandes ríos, tierras fértiles y mares interiores debería producir oportunidades diferentes a uno formado por montañas, desiertos y archipiélagos.

No se busca que el jugador reciba simplemente un mapa aleatorio. Se busca generar un entorno geográfico coherente, capaz de sostener miles de años de historia alternativa.

**Principio de diseño:**

*La geografía no determinará inevitablemente el destino de una civilización, pero condicionará profundamente las oportunidades que tendrá para construirlo.*

---

## 21. Decisiones confirmadas

| Área | Decisión |
|---|---|
| Tipo de mundo | Configurable; inicialmente mapas planos con bordes naturales |
| Continentes e islas | Distribución completamente configurable |
| Geología | Realista y configurable |
| Relieve | Geográficamente coherente |
| Ríos y lagos | Generación basada en topografía e hidrología |
| Costas y océanos | Gran diversidad de formaciones |
| Biomas | Determinados por condiciones geográficas, con cierta flexibilidad |
| Recursos | Distribución realista |
| Suelos | Diferenciados según origen y características |
| Abundancia | Completamente configurable |
| Civilizaciones | Ubicación automática con recomendaciones |
| Distribución inicial | Configurable, incluida ubicación manual |
| Equilibrio | Opcional |
| Exploración | Configurable, con estados de conocimiento |
| Mapas históricos | Sí |
| Escala | Configurable mediante presets y opciones avanzadas |
| Semillas | Sí |
| Calidad visual | Prioridad alta con optimización |
| Editor de mundos | Módulo independiente |
| Modificación durante la partida | Herramientas limitadas integradas |
| Complejidad | Sencilla, intermedia y avanzada |

**Estado:** GDD conceptual v0.1 consolidado. Preparado para integrarse con el diseño general del juego y evolucionar hacia una especificación técnica cuando corresponda.
