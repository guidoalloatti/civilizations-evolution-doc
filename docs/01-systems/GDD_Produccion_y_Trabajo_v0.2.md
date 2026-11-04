> **Control documental:** 0.2 | Estado: diseño conceptual; las decisiones marcadas como confirmadas se preservan desde la fuente. **Alcance:** este GDD es fuente de detalle de su dominio; el GDD Maestro establece la visión común. Los conflictos intersistema se registran en AUDIT_REPORT.md y no se resuelven implícitamente.

---

# GDD — SISTEMA DE PRODUCCIÓN Y TRABAJO

**Versión:** 0.2  
**Estado:** Diseño conceptual  
**Período histórico:** Paleolítico – aproximadamente 1800 d.C.  
**Sistemas relacionados:** Economía, población, construcción, tecnología, logística, religión, cultura y política.

---

# 1. VISIÓN GENERAL

El sistema de producción y trabajo determinará cómo una civilización transforma los recursos disponibles en bienes, productos y servicios.

Su objetivo es representar la evolución de las actividades productivas desde pequeñas comunidades de cazadores y recolectores hasta sociedades altamente especializadas, con industrias, manufacturas, trabajadores cualificados y sistemas productivos complejos.

El sistema deberá permitir que el jugador:

- Construya libremente edificios productivos.
- Organice sus cadenas de producción.
- Aproveche los recursos disponibles en el territorio.
- Desarrolle diferentes especializaciones económicas.
- Mejore la productividad mediante tecnología.
- Gestione la disponibilidad de trabajadores.
- Influya en las condiciones laborales.
- Automatice procesos.
- Desarrolle economías diferentes según sus decisiones.
- Observe cómo sus edificios evolucionan visual y funcionalmente a través de los siglos.

La producción no deberá estar limitada exclusivamente por desbloqueos tecnológicos. La tecnología habilitará nuevas posibilidades y mejorará las existentes, pero el entorno, los recursos, la organización social y las decisiones del jugador también serán determinantes.

## 1.1. Principios de diseño

1. **Producción coherente:** los edificios producen bienes de acuerdo con sus capacidades y recursos disponibles.
2. **Libertad de construcción:** el jugador organiza sus asentamientos sin depender de una disposición predeterminada.
3. **Especialización:** los trabajadores desarrollan habilidades y profesiones.
4. **Automatización:** los procesos funcionan sin intervención constante.
5. **Interdependencia:** los edificios forman cadenas productivas conectadas.
6. **Evolución histórica:** los métodos productivos cambian con la tecnología y la sociedad.
7. **Consecuencias:** las decisiones sobre producción y trabajo tienen efectos económicos y sociales.
8. **Complejidad configurable:** el jugador elige entre una gestión sencilla o detallada.
9. **Evolución visual:** los edificios reflejan físicamente el desarrollo de la civilización.
10. **Libertad de desarrollo:** no todas las civilizaciones tienen que evolucionar de la misma manera.

---

# 2. MODELO GENERAL DE PRODUCCIÓN

La producción se organizará alrededor de cuatro componentes:

1. Edificios e instalaciones.
2. Trabajadores.
3. Recursos e insumos.
4. Procesos productivos.

Un edificio productivo necesita determinadas condiciones para funcionar.

Por ejemplo, una panadería necesita:

- Un edificio adecuado.
- Trabajadores.
- Harina.
- Agua.
- Combustible, según la tecnología utilizada.
- Acceso a los insumos.
- Capacidad de almacenamiento o distribución del producto terminado.

Cuando estas condiciones se cumplen, el edificio comienza a producir.

La producción estará limitada por el factor más restrictivo.

Una panadería puede tener capacidad para producir 100 panes por día, pero si solamente recibe harina suficiente para producir 40, su producción efectiva será de 40.

El jugador podrá intervenir para mejorar las condiciones productivas, pero no deberá administrar manualmente cada operación.

---

# 3. EDIFICIOS PRODUCTIVOS

Los edificios serán las unidades principales de producción.

Cada edificio tendrá características propias que determinarán qué puede producir, qué necesita y cómo funciona.

## 3.1. Tipos de edificios

### Extracción

Obtienen recursos directamente del entorno.

Ejemplos:
- Campamentos de recolección.
- Cotos de caza.
- Pesquerías.
- Canteras.
- Minas.
- Tala de árboles.
- Pozos.

### Producción primaria

Transforman recursos naturales mediante actividades agrícolas o ganaderas.

Ejemplos:
- Granjas.
- Huertos.
- Pasturas.
- Criaderos.
- Plantaciones.
- Piscifactorías.

### Transformación

Procesan materias primas y producen bienes intermedios o finales.

Ejemplos:
- Molinos.
- Panaderías.
- Aserraderos.
- Hornos de cerámica.
- Herrerías.
- Curtidurías.
- Tejedurías.

### Manufactura

Producen bienes más complejos y especializados.

Ejemplos:
- Talleres de herramientas.
- Carpinterías.
- Astilleros.
- Fábricas textiles.
- Talleres de muebles.
- Fabricantes de maquinaria.

### Servicios

Proporcionan actividades necesarias para el funcionamiento de la sociedad.

Ejemplos:
- Mercados.
- Escuelas.
- Centros administrativos.
- Hospitales.
- Talleres de reparación.
- Servicios de transporte.

La aparición de cada categoría dependerá de las condiciones históricas, tecnológicas y sociales de cada civilización.

## 3.2. Propiedades de cada edificio

Cada edificio tendrá una ficha productiva con:

| Propiedad | Descripción |
|---|---|
| Capacidad | Producción máxima posible |
| Trabajadores | Cantidad necesaria y máxima |
| Especialización | Profesiones requeridas |
| Insumos | Recursos necesarios |
| Productos | Bienes generados |
| Tecnología | Requisitos y mejoras |
| Eficiencia | Rendimiento del proceso |
| Mantenimiento | Costos para mantenerlo operativo |
| Almacenamiento | Capacidad para guardar insumos y productos |
| Radio de operación | Área de influencia, cuando corresponda |
| Estado | Funcionamiento, deterioro o inactividad |
| Propiedad | Quién controla el edificio y sus beneficios |
| Evolución | Mejoras y transformaciones disponibles |
| Ampliaciones | Módulos y sectores que pueden incorporarse |

---

# 4. PROCESOS Y RECETAS DE PRODUCCIÓN

Cada actividad productiva estará definida mediante un proceso.

Un proceso establece qué recursos se necesitan, cuánto tiempo lleva producir y qué resultados se obtienen.

## 4.1. Estructura de una receta

Cada receta tendrá:

- Nombre.
- Edificio requerido.
- Insumos.
- Cantidad de cada insumo.
- Tiempo de producción.
- Trabajadores necesarios.
- Especialización requerida.
- Tecnología necesaria.
- Productos generados.
- Subproductos, cuando existan.
- Consumo energético.
- Desgaste de herramientas o maquinaria.

Ejemplo:

### Producción de harina

**Edificio:** Molino.

**Insumo:**
- 10 unidades de trigo.

**Proceso:**
- Molienda.
- Consumo de energía mecánica.
- Tiempo de procesamiento.

**Producto:**
- 8 unidades de harina.

**Subproducto:**
- Restos aprovechables para alimentación animal, dependiendo del proceso.

Los valores son ilustrativos y deberán balancearse durante el desarrollo.

## 4.2. Recetas alternativas

Un mismo producto podrá obtenerse mediante diferentes métodos.

Por ejemplo, una civilización podrá producir harina mediante:

- Molienda manual.
- Molinos accionados por animales.
- Molinos hidráulicos.
- Molinos de viento.

Cada método tendrá diferentes requisitos, costos y productividad.

Esto permitirá que la tecnología no se limite a desbloquear edificios nuevos, sino que también transforme los procesos existentes.

## 4.3. Flexibilidad productiva

El sistema deberá permitir incorporar nuevas recetas sin necesidad de crear un edificio completamente nuevo para cada variante.

Un mismo taller podrá tener diferentes especializaciones o procesos.

Por ejemplo, una herrería podrá producir:

- Herramientas agrícolas.
- Herramientas de construcción.
- Armas.
- Piezas metálicas.
- Elementos decorativos.

El jugador establecerá prioridades y el edificio adaptará su producción a las necesidades y a la demanda, teniendo en cuenta sus recursos y capacidades.

---

# 5. CADENAS PRODUCTIVAS

Las cadenas productivas conectarán diferentes edificios.

Su objetivo es generar dependencia económica entre actividades y hacer que la organización territorial tenga importancia.

## 5.1. Ejemplo: cadena alimentaria

1. Granja produce trigo.
2. Transporte lleva el trigo al molino.
3. Molino transforma trigo en harina.
4. Transporte lleva harina a la panadería.
5. Panadería produce pan.
6. El pan se distribuye a los mercados o puntos de consumo.
7. La población consume el producto.

Cada etapa depende de la anterior.

Una interrupción en la granja, el transporte o el molino puede afectar toda la cadena.

## 5.2. Ejemplo: cadena de construcción

1. Leñador obtiene madera.
2. Cantera obtiene piedra.
3. Aserradero procesa madera.
4. Taller produce herramientas.
5. Transporte distribuye los materiales.
6. Constructores utilizan los recursos para levantar edificios.

## 5.3. Dependencias productivas

Los edificios podrán depender de múltiples proveedores.

Una herrería, por ejemplo, puede necesitar:

- Mineral de hierro.
- Combustible.
- Herramientas auxiliares.
- Trabajadores especializados.

El sistema deberá identificar los problemas de abastecimiento y comunicar al jugador dónde se encuentra el cuello de botella.

## 5.4. Especialización regional

Las cadenas productivas no tendrán que estar concentradas en un solo asentamiento.

El jugador podrá desarrollar:

- Regiones agrícolas.
- Regiones mineras.
- Centros manufactureros.
- Ciudades comerciales.
- Puertos especializados.

Esto favorecerá el comercio y la interdependencia entre asentamientos.

---

# 6. PRODUCTIVIDAD Y EFICIENCIA

La productividad determinará cuánto puede producir un edificio en condiciones reales.

No todos los edificios tendrán el mismo rendimiento, aunque sean del mismo tipo.

## 6.1. Factores de productividad

La producción efectiva dependerá de:

- Cantidad de trabajadores.
- Experiencia y habilidades.
- Tecnología.
- Calidad de las herramientas.
- Disponibilidad de materias primas.
- Calidad de los insumos.
- Estado del edificio.
- Condiciones ambientales.
- Organización del trabajo.
- Accesibilidad y logística.
- Condiciones laborales.

## 6.2. Modelo conceptual

La productividad podrá representarse mediante una fórmula general:

**Producción efectiva = Capacidad base × Modificadores de eficiencia × Disponibilidad de insumos.**

Los modificadores podrán incluir:

- Eficiencia laboral.
- Eficiencia tecnológica.
- Calidad de materiales.
- Condiciones ambientales.
- Estado de las instalaciones.

La producción nunca deberá superar la capacidad física del edificio o del proceso, salvo que una tecnología específica permita modificarla.

## 6.3. Ejemplo

Una granja tiene una capacidad base de 100 unidades de trigo por temporada.

Sus condiciones son:

- Trabajadores: 90% de eficiencia.
- Tecnología: 110%.
- Suelo: 120%.
- Estado de herramientas: 90%.

La producción final dependerá de la combinación de estos factores.

Los valores deberán balancearse para evitar que pequeñas mejoras produzcan aumentos desproporcionados.

## 6.4. Productividad y especialización

Un trabajador experimentado podrá producir más que uno sin formación.

La especialización permitirá:

- Reducir tiempos de producción.
- Mejorar la calidad.
- Reducir desperdicios.
- Utilizar procesos más complejos.
- Desarrollar nuevas técnicas.

La experiencia se adquirirá mediante trabajo, aprendizaje y formación.

---

# 7. SISTEMA DE TRABAJO Y ASIGNACIÓN DE EMPLEADOS

**Decisión adoptada: asignación automática con intervención del jugador.**

El jugador no tendrá que asignar manualmente a cada ciudadano.

Los habitantes buscarán oportunidades laborales de acuerdo con sus capacidades, necesidades y preferencias.

## 7.1. Funcionamiento automático

Cuando se construya un edificio productivo, este generará puestos de trabajo.

El sistema buscará trabajadores disponibles teniendo en cuenta:

1. Distancia entre vivienda y trabajo.
2. Habilidades necesarias.
3. Experiencia.
4. Salario o beneficios.
5. Condiciones laborales.
6. Disponibilidad de trabajadores.
7. Prioridad económica del edificio.

La asignación será dinámica.

Un trabajador podrá cambiar de empleo si encuentra una oportunidad más conveniente, siempre que las condiciones sociales y económicas lo permitan.

## 7.2. Intervención del jugador

El jugador podrá:

- Establecer prioridades productivas.
- Limitar la cantidad de trabajadores.
- Definir salarios o beneficios en etapas avanzadas.
- Favorecer determinados sectores.
- Establecer horarios o turnos cuando corresponda.
- Aplicar restricciones laborales.
- Incentivar la formación de profesionales.

El objetivo es ofrecer control estratégico sin obligar al jugador a administrar individualmente cada empleo.

## 7.3. Prioridades de producción

Cada edificio podrá tener una prioridad:

- Crítica.
- Alta.
- Normal.
- Baja.
- Desactivada.

Si existe escasez de trabajadores, el sistema utilizará estas prioridades para distribuir la mano de obra.

Por ejemplo, durante una crisis alimentaria, el jugador podrá priorizar granjas y panaderías sobre talleres de muebles.

## 7.4. Desempleo

La población disponible no siempre encontrará trabajo.

El desempleo podrá producir:

- Menores ingresos.
- Menor consumo.
- Descontento.
- Migración.
- Problemas sociales.

Sin embargo, una cierta cantidad de población sin empleo también podrá ser necesaria para mantener flexibilidad económica y cubrir nuevas actividades.

---

# 8. PROFESIONES Y HABILIDADES

Los trabajadores podrán desarrollar diferentes profesiones y niveles de especialización.

**Decisión adoptada:** cada trabajador tendrá una profesión principal y podrá desarrollar habilidades secundarias.

## 8.1. Profesiones

Las profesiones evolucionarán con la sociedad.

Ejemplos:

**Sociedades primitivas**
- Recolector.
- Cazador.
- Pescador.
- Artesano.

**Sociedades agrícolas**
- Agricultor.
- Ganadero.
- Constructor.
- Alfarero.

**Sociedades metalúrgicas**
- Minero.
- Fundidor.
- Herrero.
- Carpintero especializado.

**Sociedades urbanas**
- Comerciante.
- Administrador.
- Escribano.
- Arquitecto.
- Médico.

**Sociedades modernas**
- Ingeniero.
- Técnico.
- Operario especializado.
- Contador.
- Investigador.

## 8.2. Niveles de habilidad

Cada profesión podrá tener diferentes niveles:

1. Sin experiencia.
2. Aprendiz.
3. Trabajador competente.
4. Especialista.
5. Maestro.

Los niveles no deberán ser necesariamente idénticos para todas las profesiones.

## 8.3. Habilidades secundarias

Un trabajador podrá tener una profesión principal y conocimientos adicionales.

Ejemplo:

**Juan**
- Profesión principal: herrero.
- Habilidad secundaria: carpintería.
- Experiencia: avanzada en metalurgia.
- Experiencia: básica en construcción.

Esto permitirá cierta flexibilidad sin convertir a todos los habitantes en trabajadores universales.

## 8.4. Aprendizaje

Los habitantes podrán adquirir habilidades mediante:

- Experiencia laboral.
- Aprendizaje familiar.
- Formación con otros trabajadores.
- Instituciones educativas.
- Gremios.
- Capacitación especializada.

La transmisión de conocimientos será importante para la evolución tecnológica.

## 8.5. Movilidad laboral

Los trabajadores podrán cambiar de profesión.

Sin embargo, algunas actividades requerirán formación y tiempo de aprendizaje.

Esto evitará que una civilización pueda transformar instantáneamente a toda su población en especialistas de cualquier sector.

---

# 9. SALARIOS, INCENTIVOS Y CONDICIONES LABORALES

El trabajo tendrá una dimensión económica y social.

No todas las sociedades funcionarán con el mismo sistema laboral.

## 9.1. Evolución de las relaciones laborales

Podrán existir diferentes modelos:

- Trabajo comunitario.
- Trabajo familiar.
- Trabajo obligatorio.
- Servidumbre.
- Trabajo esclavo, cuando corresponda históricamente.
- Trabajo asalariado.
- Trabajo independiente.
- Trabajo empresarial.

No todas las civilizaciones deberán atravesar las mismas etapas ni adoptar los mismos modelos.

## 9.2. Salarios

En sociedades con economías monetarias, los trabajadores podrán recibir salarios.

Estos dependerán de:

- Profesión.
- Especialización.
- Oferta y demanda laboral.
- Condiciones de trabajo.
- Rentabilidad de la actividad.
- Legislación.
- Poder de negociación.

## 9.3. Condiciones laborales

El jugador podrá influir en:

- Jornadas.
- Descansos.
- Seguridad.
- Alimentación de los trabajadores.
- Vivienda.
- Remuneración.
- Beneficios.

Las mejores condiciones podrán aumentar la satisfacción y productividad, pero también elevar los costos.

Las malas condiciones podrán provocar accidentes, enfermedades, conflictos y pérdida de trabajadores.

## 9.4. Conflictos laborales

En sociedades más complejas podrán aparecer:

- Protestas.
- Reclamos salariales.
- Huelgas.
- Organizaciones de trabajadores.
- Gremios.
- Conflictos entre propietarios y empleados.

Estos acontecimientos estarán conectados con los sistemas político y social.

---

# 10. EVOLUCIÓN TECNOLÓGICA DE LA PRODUCCIÓN

La tecnología transformará profundamente la forma de producir.

No se limitará a aumentar números: permitirá nuevos métodos, edificios, herramientas y organizaciones.

## 10.1. Evolución de los métodos productivos

### Paleolítico

- Producción manual.
- Herramientas rudimentarias.
- Caza y recolección.
- Producción comunitaria.
- Baja especialización.

### Neolítico

- Agricultura.
- Ganadería.
- Cerámica.
- Herramientas agrícolas.
- Almacenamiento de excedentes.
- Especialización inicial.

### Edad del Bronce y del Hierro

- Metalurgia.
- Herramientas más eficientes.
- Talleres especializados.
- Producción artesanal.
- Mayor división del trabajo.

### Antigüedad

- Molinos.
- Canales de riego.
- Hornos especializados.
- Grandes explotaciones agrícolas.
- Talleres urbanos.
- Infraestructura productiva.

### Edad Media

- Molinos hidráulicos.
- Molinos de viento.
- Gremios.
- Talleres especializados.
- Técnicas de construcción avanzadas.
- Mayor organización productiva.

### Edad Moderna

- Manufacturas.
- Especialización de tareas.
- Mejores herramientas.
- Nuevas técnicas agrícolas.
- Mayor escala productiva.
- Primeras formas de mecanización.

### Período preindustrial

- Máquinas.
- Sistemas productivos más complejos.
- División avanzada del trabajo.
- Mecanización inicial.
- Mayor concentración industrial.

Estas etapas son referencias, no un árbol tecnológico obligatorio.

## 10.2. Nuevos métodos de producción

Una tecnología podrá habilitar diferentes formas de realizar una actividad.

Por ejemplo:

Un molino podrá funcionar inicialmente con fuerza humana o animal y posteriormente utilizar energía hidráulica o eólica.

Esto permitirá mantener la continuidad histórica de los edificios.

## 10.3. Mejoras tecnológicas

Las tecnologías podrán modificar:

- Capacidad productiva.
- Velocidad de producción.
- Eficiencia.
- Calidad.
- Consumo de recursos.
- Necesidades de trabajadores.
- Métodos de producción.
- Apariencia de las instalaciones.

---

# 11. EVOLUCIÓN VISUAL Y FUNCIONAL DE LOS EDIFICIOS

**Esta es una de las mecánicas centrales de identidad del juego.**

Los edificios no serán estructuras estáticas que simplemente se desbloquean y reemplazan.

Cada construcción podrá evolucionar mediante mejoras, ampliaciones y transformaciones habilitadas por diferentes acontecimientos.

El objetivo es que el jugador pueda observar cómo una pequeña aldea primitiva se transforma gradualmente en una civilización compleja.

## 11.1. Desencadenantes de evolución

Una nueva evolución podrá habilitarse por diferentes motivos:

### Tecnológicos
- Descubrimiento de nuevas técnicas.
- Nuevas herramientas.
- Nuevos materiales.
- Nuevos métodos constructivos.
- Nuevas fuentes de energía.

### Históricos
- Transición a una nueva edad.
- Cambios en las condiciones históricas.
- Desarrollo de nuevas instituciones.

### Religiosos
- Aparición de una nueva religión.
- Reformas religiosas.
- Cambios en las prácticas religiosas.
- Nuevas necesidades ceremoniales o institucionales.

### Culturales
- Nuevas costumbres.
- Nuevos conocimientos.
- Nuevas formas de organización.
- Cambios estéticos y arquitectónicos.

### Sociales
- Aparición de nuevas clases sociales.
- Nuevas necesidades de la población.
- Cambios en las relaciones laborales.
- Desarrollo de instituciones.

### Políticos
- Cambios de gobierno.
- Nuevas leyes.
- Reformas administrativas.
- Cambios en la organización territorial.

### Económicos
- Nuevas formas de propiedad.
- Desarrollo del comercio.
- Especialización productiva.
- Nuevas necesidades del mercado.

Los desencadenantes no serán exclusivamente tecnológicos.

Un cambio religioso o cultural también podrá modificar la arquitectura, las funciones y las necesidades de determinados edificios.

## 11.2. Sistema de notificaciones de mejoras

Cuando un edificio tenga disponible una nueva evolución, el jugador recibirá una indicación visible.

La opción aparecerá:

- En el propio edificio.
- En su representación dentro del mapa.
- En el panel de información del edificio.

La interfaz podrá mostrar:

**Nueva evolución disponible: Herrería medieval.**

Requisitos cumplidos:
- Tecnología metalúrgica avanzada.
- Disponibilidad de hierro.
- Conocimientos especializados.

Beneficios:
- Mayor capacidad productiva.
- Nuevas recetas.
- Mejor calidad de herramientas.
- Nueva apariencia arquitectónica.

El jugador podrá consultar los cambios y decidir cuándo aplicar la mejora.

## 11.3. Transformaciones de edificios

Las evoluciones podrán modificar:

- Arquitectura exterior.
- Materiales de construcción.
- Equipamiento.
- Herramientas.
- Maquinaria.
- Capacidad productiva.
- Profesiones necesarias.
- Productos disponibles.
- Necesidades de infraestructura.
- Funciones del edificio.

Una mejora no será necesariamente un simple aumento de productividad.

Podrá representar un cambio profundo en la forma en que funciona una actividad.

## 11.4. Evolución visual histórica

Los edificios deberán reflejar el contexto histórico y cultural de la civilización.

Por ejemplo:

Una herrería podrá evolucionar desde un pequeño taller artesanal hasta un complejo productivo con diferentes sectores, mejores hornos, depósitos y trabajadores especializados.

Una granja podrá evolucionar desde una explotación familiar sencilla hasta una instalación agrícola más organizada, con mejores herramientas, sistemas de riego y almacenamiento.

La evolución no será idéntica en todas las civilizaciones.

La geografía, los materiales disponibles, la cultura, la religión y las tecnologías influirán en su apariencia.

## 11.5. Evolución no obligatoria

El jugador no estará obligado a aplicar todas las mejoras.

Podrá mantener edificios antiguos cuando tenga sentido económico, cultural o estratégico.

Una construcción histórica podrá seguir funcionando si dispone de los recursos y condiciones necesarios.

Esto permitirá crear ciudades con diferentes capas históricas y arquitectónicas.

---

# 12. AMPLIACIÓN FÍSICA Y MODULAR DE EDIFICIOS

**Decisión adoptada: ampliación mediante módulos y sectores.**

Los edificios podrán crecer físicamente a medida que la civilización se desarrolle.

No todos tendrán que ser reemplazados por versiones completamente nuevas.

## 12.1. Sistema modular

Un edificio podrá incorporar diferentes módulos.

Ejemplos:

**Herrería**
- Taller principal.
- Depósito de minerales.
- Depósito de combustible.
- Área de fundición.
- Sector de fabricación.
- Área de almacenamiento.

**Granja**
- Vivienda familiar.
- Granero.
- Establo.
- Depósito de herramientas.
- Área de procesamiento.
- Instalaciones de riego.

**Aserradero**
- Área de recepción de madera.
- Sector de procesamiento.
- Depósito.
- Área de secado.
- Taller complementario.

## 12.2. Beneficios de las ampliaciones

Los módulos podrán:

- Aumentar capacidad.
- Mejorar eficiencia.
- Incorporar nuevas recetas.
- Reducir costos logísticos.
- Permitir más trabajadores.
- Incorporar nuevas tecnologías.
- Diversificar la producción.

## 12.3. Restricciones

Las ampliaciones dependerán de:

- Espacio disponible.
- Recursos.
- Tecnología.
- Costos.
- Mano de obra.
- Condiciones geográficas.

La ampliación física deberá respetar la libertad de construcción del jugador.

---

# 13. CALIDAD Y VARIEDAD DE PRODUCTOS

**Decisión adoptada: incorporación gradual de la calidad.**

No todos los productos deberán ser idénticos.

En etapas avanzadas, la calidad podrá convertirse en un factor económico importante.

## 13.1. Factores de calidad

La calidad dependerá de:

- Materias primas.
- Experiencia del trabajador.
- Tecnología.
- Herramientas.
- Tiempo de producción.
- Condiciones del proceso.

## 13.2. Evolución histórica

En las primeras épocas, la calidad tendrá una influencia limitada y estará simplificada.

A medida que aparezcan profesiones especializadas, manufacturas y mercados más complejos, la calidad adquirirá mayor importancia.

## 13.3. Consecuencias

Los productos de mayor calidad podrán:

- Tener mayor valor comercial.
- Durar más.
- Ser más eficientes.
- Satisfacer necesidades superiores.
- Ser utilizados en construcciones o productos especiales.

Por ejemplo, una herramienta de hierro de buena calidad podrá durar más y mejorar la productividad agrícola.

## 13.4. Especialización productiva

Los asentamientos podrán desarrollar reputación por determinados productos.

Una ciudad podrá especializarse en cerámica, textiles, herramientas o productos agrícolas.

Esto podrá generar ventajas comerciales y culturales.

---

# 14. MANTENIMIENTO Y DETERIORO

Los edificios, herramientas y máquinas necesitarán mantenimiento.

## 14.1. Edificios

Los edificios podrán deteriorarse debido a:

- Tiempo.
- Clima.
- Uso.
- Falta de mantenimiento.
- Materiales de construcción.

El deterioro podrá reducir su eficiencia y eventualmente impedir su funcionamiento.

## 14.2. Herramientas y maquinaria

Las herramientas tendrán desgaste.

Su reemplazo o reparación requerirá recursos y trabajo.

Las tecnologías avanzadas podrán mejorar su durabilidad.

## 14.3. Mantenimiento automático

El jugador podrá establecer políticas de mantenimiento:

- Mínimo.
- Normal.
- Preventivo.
- Intensivo.

Cada opción tendrá costos y consecuencias.

---

# 15. ESTACIONALIDAD Y CONDICIONES AMBIENTALES

Los procesos productivos dependerán del entorno.

## 15.1. Agricultura

La producción agrícola dependerá de:

- Estaciones.
- Clima.
- Suelo.
- Agua.
- Cultivos.
- Tecnología.

## 15.2. Actividades extractivas

Algunas actividades podrán verse afectadas por:

- Clima extremo.
- Accesibilidad.
- Disponibilidad de recursos.
- Condiciones geográficas.
- Distancia.

## 15.3. Actividades artesanales e industriales

Las actividades de transformación podrán funcionar durante todo el año, aunque dependerán de los insumos, la energía y las condiciones de los edificios.

Esto permitirá que las economías tengan ritmos productivos diferentes según la estación.

---

# 16. ORGANIZACIÓN DEL TRABAJO Y ESCALA PRODUCTIVA

La forma de organizar la producción cambiará históricamente.

## 16.1. Producción familiar

En sociedades pequeñas, una familia podrá producir diferentes bienes y satisfacer gran parte de sus necesidades.

## 16.2. Producción artesanal

Con la especialización aparecerán trabajadores dedicados a una actividad específica.

Los talleres podrán concentrar conocimientos y herramientas.

## 16.3. Gremios y organizaciones productivas

En determinadas sociedades podrán aparecer organizaciones que regulen profesiones, calidad, aprendizaje y acceso a determinadas actividades.

## 16.4. Manufactura

La producción podrá concentrarse en establecimientos donde diferentes trabajadores realicen tareas especializadas.

## 16.5. Industria

En el período final del juego podrán aparecer instalaciones de mayor escala, con maquinaria, energía y una división del trabajo más avanzada.

La evolución no será necesariamente igual en todas las civilizaciones.

---

# 17. PROPIEDAD Y CONTROL DE LA PRODUCCIÓN

**Decisión adoptada: modelo mixto de autonomía privada e intervención del jugador.**

Los edificios podrán pertenecer a:

- La comunidad.
- Una familia.
- Un propietario privado.
- El Estado.
- Instituciones religiosas.
- Organizaciones comerciales.
- Empresas.

La propiedad determinará quién controla el edificio y quién recibe sus beneficios.

## 17.1. Producción comunitaria

La producción y los recursos podrán administrarse colectivamente.

## 17.2. Producción familiar

Las familias podrán gestionar sus propios establecimientos y conservar parte de sus excedentes.

## 17.3. Producción privada

Los propietarios podrán contratar trabajadores, comprar insumos y vender productos.

Tendrán autonomía para tomar decisiones económicas dentro de las condiciones establecidas por la sociedad.

## 17.4. Producción estatal

El Estado podrá controlar instalaciones estratégicas, establecer objetivos productivos y administrar sus beneficios.

## 17.5. Producción religiosa

Las instituciones religiosas podrán poseer tierras, talleres y establecimientos productivos.

## 17.6. Intervención del jugador

El jugador podrá influir sobre los productores privados mediante:

- Impuestos.
- Incentivos.
- Subsidios.
- Regulaciones.
- Restricciones.
- Políticas comerciales.
- Legislación laboral.
- Políticas de propiedad.

El objetivo es que la economía tenga autonomía y no sea simplemente un conjunto de edificios que ejecutan órdenes directas del jugador.

---

# 18. INTERACCIÓN CON OTROS SISTEMAS

El sistema de producción no funcionará de manera aislada.

## Economía

Determina costos, ingresos, precios, excedentes y rentabilidad.

## Población

Proporciona trabajadores y consumidores.

## Tecnología

Habilita nuevos métodos y aumenta la eficiencia.

## Construcción

Determina dónde se ubican los edificios, cómo crecen y qué infraestructura necesitan.

## Logística

Permite transportar materias primas, herramientas y productos.

## Política

Regula impuestos, salarios, propiedad y condiciones laborales.

## Religión

Puede modificar las necesidades productivas, las construcciones, los materiales y las actividades de determinadas instituciones.

## Cultura y sociedad

Influyen en las profesiones, la organización del trabajo y las aspiraciones de la población.

## Medioambiente

Determina la disponibilidad de recursos y las condiciones de producción.

---

# 19. INTERFAZ DE PRODUCCIÓN

El jugador deberá poder comprender el funcionamiento de cada edificio sin tener que consultar información externa constantemente.

## 19.1. Panel del edificio

Cada edificio mostrará:

- Estado de funcionamiento.
- Producción actual.
- Capacidad máxima.
- Trabajadores asignados.
- Puestos disponibles.
- Insumos disponibles.
- Productos almacenados.
- Eficiencia.
- Problemas detectados.
- Propietario.
- Mejoras disponibles.
- Ampliaciones disponibles.
- Evolución histórica disponible.

## 19.2. Panel de producción

Permitirá observar todas las actividades productivas de un asentamiento.

Ejemplos:

- Producción total de alimentos.
- Producción de materiales.
- Consumo de materias primas.
- Capacidad productiva.
- Trabajadores ocupados.
- Edificios inactivos.
- Productividad por sector.

## 19.3. Alertas

El sistema notificará situaciones importantes:

- Falta de trabajadores.
- Falta de materias primas.
- Falta de herramientas.
- Problemas de transporte.
- Producción interrumpida.
- Baja productividad.
- Exceso de inventario.
- Edificios deteriorados.
- Mejoras disponibles.
- Nuevas evoluciones habilitadas.

## 19.4. Información contextual

El jugador podrá seleccionar cualquier edificio y consultar por qué está produciendo menos de lo esperado.

Ejemplo:

**Panadería: producción reducida.**

- Capacidad: 100 panes.
- Producción actual: 35 panes.
- Trabajadores: suficientes.
- Harina: insuficiente.
- Problema detectado: el molino no está recibiendo suficiente trigo.

La información deberá permitir identificar rápidamente el origen de los problemas.

---

# 20. COMPLEJIDAD CONFIGURABLE

El sistema tendrá diferentes niveles de profundidad.

## Nivel básico

- Los edificios buscan trabajadores automáticamente.
- Los procesos productivos son automáticos.
- Los recursos se distribuyen mediante logística simplificada.
- El jugador establece prioridades generales.
- La calidad y las condiciones laborales tienen poca intervención directa.

## Nivel intermedio

- El jugador controla prioridades por edificio.
- Puede configurar cantidades de producción.
- Puede establecer reservas.
- Puede administrar salarios y algunas condiciones laborales.
- Puede optimizar cadenas productivas.
- Puede controlar las ampliaciones.

## Nivel avanzado

- Gestión detallada de trabajadores.
- Especialización y capacitación.
- Control de salarios y contratos.
- Planificación de producción.
- Gestión de inventarios.
- Control detallado de logística.
- Optimización de costos y productividad.
- Administración de organizaciones productivas.

La complejidad no deberá alterar las reglas fundamentales de la simulación. Solamente cambiará cuánto control tiene el jugador.

---

# 21. SIMULACIÓN Y RENDIMIENTO

El sistema deberá funcionar tanto en pequeñas comunidades como en grandes ciudades.

## 21.1. Edificios

Cada edificio tendrá una simulación de producción independiente, pero optimizada.

No será necesario procesar continuamente todos sus cálculos si no hay cambios relevantes.

## 21.2. Trabajadores

En asentamientos pequeños se podrán simular trabajadores individuales.

En asentamientos grandes, los trabajadores podrán agruparse estadísticamente.

## 21.3. Procesos productivos

Las actividades podrán calcularse por intervalos de tiempo, en lugar de actualizar cada operación constantemente.

Por ejemplo, una granja podrá actualizar su producción diaria o estacional.

## 21.4. Escalabilidad

La arquitectura deberá permitir incorporar nuevas profesiones, edificios, recetas y métodos productivos sin modificar los sistemas centrales.

---

# 22. MVP — PRIMER PROTOTIPO DE PRODUCCIÓN Y TRABAJO

El primer prototipo deberá demostrar que los edificios pueden producir de manera autónoma y que los trabajadores se asignan correctamente.

No intentaremos implementar todas las profesiones ni tecnologías desde el comienzo.

## 22.1. Edificios

- Viviendas.
- Recolector.
- Leñador.
- Cantera.
- Granja.
- Molino.
- Panadería.
- Almacén.

## 22.2. Profesiones

- Recolector.
- Leñador.
- Minero o cantero.
- Agricultor.
- Molinero.
- Panadero.
- Constructor.

## 22.3. Recursos

- Madera.
- Piedra.
- Bayas.
- Carne.
- Trigo.
- Harina.
- Pan.

## 22.4. Funcionalidades

El prototipo deberá permitir:

1. Construir edificios productivos.
2. Generar puestos de trabajo.
3. Asignar trabajadores automáticamente.
4. Producir recursos.
5. Transformar materias primas.
6. Transportar productos entre edificios.
7. Almacenar insumos y productos.
8. Consumir recursos según las necesidades.
9. Establecer prioridades de producción.
10. Identificar problemas productivos.
11. Observar el impacto de la falta de trabajadores.
12. Simular variaciones de productividad.

## 22.5. Primer escenario de prueba

Crear una pequeña aldea de 30 habitantes.

El jugador deberá construir:

- Una zona de recolección.
- Una granja.
- Un molino.
- Una panadería.
- Un almacén.
- Viviendas.

El sistema deberá organizar automáticamente a los trabajadores.

El objetivo será comprobar que:

- Los habitantes encuentran trabajo.
- Los edificios reciben insumos.
- Las cadenas productivas funcionan.
- Los productos llegan a los consumidores.
- La población puede alimentarse.
- La falta de recursos genera problemas.
- El jugador puede intervenir para solucionarlos.

**Criterio de éxito:** la aldea debe poder funcionar durante un período prolongado sin que el jugador tenga que asignar manualmente cada trabajador o transportar personalmente cada recurso.

## 22.6. Evolución visual del prototipo

Aunque el MVP comience con pocas tecnologías, deberá contemplar desde su arquitectura la posibilidad de evolucionar edificios.

No es necesario implementar decenas de variantes inicialmente, pero sí diseñar el sistema para que una construcción pueda tener:

- Una versión inicial.
- Mejoras tecnológicas.
- Ampliaciones modulares.
- Transformaciones arquitectónicas.
- Nuevas funcionalidades.

La evolución visual no debe agregarse como un elemento decorativo al final del desarrollo. Debe formar parte del diseño desde el principio.

---

# 23. DECISIONES DE DISEÑO CONFIRMADAS

| Sistema | Decisión |
|---|---|
| Organización productiva | Basada en edificios y procesos |
| Cadenas productivas | Interdependientes |
| Asignación laboral | Automática, con intervención del jugador |
| Especialización | Profesión principal y habilidades secundarias |
| Productividad | Determinada por múltiples factores |
| Tecnología | Mejora procesos existentes y habilita nuevos |
| Recetas | Variables y potencialmente alternativas |
| Control productivo | El jugador establece prioridades y el edificio adapta su producción |
| Calidad | Diferenciada y de incorporación gradual |
| Mantenimiento | Necesario y evolutivo |
| Propiedad | Mixta, con autonomía privada e intervención del jugador |
| Escala productiva | Desde producción familiar hasta industria |
| Ampliación | Modular, mediante módulos y sectores |
| Evolución de edificios | Tecnológica, histórica, religiosa, cultural, social, política y económica |
| Mejoras | Notificaciones visibles en el edificio y en el mapa |
| Evolución arquitectónica | Cambios visuales y funcionales |
| Complejidad | Configurable |
| Simulación | Individual en asentamientos pequeños y agregada en grandes ciudades |

---

# 24. CUESTIONES PENDIENTES DE DISEÑO

Los siguientes temas necesitarán mayor profundidad antes de su implementación:

1. Fórmulas definitivas de productividad.
2. Modelo de experiencia y aprendizaje de trabajadores.
3. Duración y condiciones de los cambios de profesión.
4. Modelo detallado de salarios e incentivos.
5. Sistema de calidad de productos.
6. Reglas de autonomía de propietarios privados.
7. Diseño de módulos y ampliaciones.
8. Condiciones exactas para habilitar mejoras.
9. Compatibilidad entre edificios antiguos y nuevas tecnologías.
10. Costos y consecuencias de las transformaciones.
11. Evolución arquitectónica por civilización y región.
12. Balance entre producción automática y control del jugador.

Estas cuestiones no impiden avanzar con el diseño general. Se podrán resolver progresivamente.

---

# 25. PRINCIPIO FUNDAMENTAL DEL SISTEMA

La producción debe representar la capacidad de una sociedad para transformar los recursos disponibles en bienes y servicios.

El trabajo debe representar la organización de las personas, sus conocimientos y sus capacidades para conseguirlo.

Una civilización no debería crecer simplemente construyendo más edificios. Deberá desarrollar mejores métodos productivos, formar trabajadores, organizar sus recursos, especializar sus asentamientos y resolver las limitaciones de su economía.

Pero hay algo todavía más importante:

La evolución de una civilización deberá ser visible.

Los edificios que el jugador construyó al principio no serán simples elementos descartables. Podrán crecer, transformarse, modernizarse y adaptarse a las nuevas condiciones históricas.

Una pequeña herrería podrá convertirse en un complejo productivo. Una granja familiar podrá evolucionar hacia una explotación agrícola avanzada. Un pequeño asentamiento podrá transformarse en una ciudad donde cada barrio refleje una etapa diferente de su historia.

El mundo deberá conservar las huellas de las decisiones del jugador.

**El progreso de una civilización será también el progreso de su capacidad para producir, organizar y transformar el mundo que la rodea.**

**Y ese progreso deberá poder verse, recorrerse y sentirse.**
