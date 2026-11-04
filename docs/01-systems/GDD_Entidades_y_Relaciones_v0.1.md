> **Control documental:** 0.1 | Estado: diseño conceptual; las decisiones marcadas como confirmadas se preservan desde la fuente. **Alcance:** este GDD es fuente de detalle de su dominio; el GDD Maestro establece la visión común. Los conflictos intersistema se registran en AUDIT_REPORT.md y no se resuelven implícitamente.

---

# GDD — Modelo de Entidades y Relaciones del Mundo
**Versión:** 0.1  
**Estado:** Diseño conceptual

## 1. Objetivo

Definir las entidades fundamentales que componen el mundo del juego, sus atributos, identidades, relaciones y evolución histórica.

Este sistema constituye una base compartida para todos los demás sistemas: población, economía, producción, construcción, tecnología, cultura, religión, política, diplomacia, exploración y guerra.

El objetivo es que el mundo no sea una colección de estadísticas aisladas, sino una estructura coherente de entidades que interactúan y evolucionan a lo largo del tiempo.

## 2. Principios fundamentales

- Todas las entidades relevantes poseen identidad propia.
- Las entidades pueden relacionarse con múltiples entidades simultáneamente.
- Las relaciones pueden cambiar, desaparecer y volver a establecerse.
- Las relaciones importantes conservan memoria histórica.
- Las entidades pueden evolucionar, transformarse, dividirse, fusionarse o desaparecer.
- Los sistemas comparten las mismas entidades y datos; no crean representaciones independientes de un mismo elemento.
- La simulación puede simplificarse según escala, importancia y rendimiento, sin eliminar necesariamente la identidad conceptual.
- La complejidad será configurable.

## 3. Habitantes e individuos

### 3.1 Modelo de población

Se utilizará un modelo híbrido:

- Individuos identificables.
- Familias y hogares.
- Grupos demográficos y sociales.
- Profesiones y sectores laborales.
- Clases sociales y comunidades.

Los individuos podrán agruparse para realizar simulaciones agregadas, especialmente en poblaciones grandes.

La agregación no implica que los habitantes pierdan necesariamente su identidad. El nivel de detalle simulado dependerá de la importancia, escala y configuración del juego.

### 3.2 Identidad individual

Cada individuo tendrá un conjunto amplio de atributos:

- Identidad y nombre.
- Edad y sexo.
- Familia y parentescos.
- Relaciones personales.
- Lugar de nacimiento y residencia.
- Ocupación y empleadores.
- Profesiones y habilidades.
- Salud y condiciones físicas.
- Educación y conocimientos.
- Cultura e idioma.
- Religión y creencias.
- Condición económica y patrimonio.
- Clase social.
- Opiniones e intereses.
- Participación en organizaciones.
- Historia personal y acontecimientos relevantes.

Los atributos y su importancia podrán variar según época y contexto histórico.

### 3.3 Personajes destacados

Cualquier individuo podrá adquirir relevancia orgánicamente.

Ejemplos:

- Líderes políticos.
- Comerciantes.
- Inventores.
- Científicos.
- Artistas.
- Religiosos.
- Militares.
- Exploradores.
- Pensadores.

El jugador podrá identificar, seguir y consultar personajes concretos, incluso cuando no sean figuras importantes para toda la civilización.

Los personajes destacados podrán disponer de biografías y relaciones más detalladas.

## 4. Familias y hogares

Se distinguirán dos conceptos relacionados:

**Familia:** unidad de parentesco y relaciones sociales. Incluye vínculos biológicos, matrimoniales, adoptivos y otros reconocidos culturalmente.

**Hogar:** unidad de convivencia y organización económica. Agrupa a quienes comparten vivienda, recursos o actividades domésticas.

Ambas estructuras podrán cambiar con el tiempo.

Un hogar puede incluir varias familias, una familia puede dividirse entre varios hogares y las estructuras familiares pueden variar según época y cultura.

Las familias y hogares podrán tener:

- Integrantes.
- Relaciones internas.
- Vivienda.
- Recursos y patrimonio.
- Actividades económicas.
- Tradiciones.
- Historia y acontecimientos relevantes.

## 5. Edificios y construcciones

Cada edificio será una entidad individual con identidad propia.

Los edificios tendrán:

- Identificador e identidad.
- Ubicación y características geográficas.
- Tipo y función.
- Propietario y responsables.
- Trabajadores y ocupantes.
- Capacidad y estado.
- Materiales y técnicas constructivas.
- Tecnología aplicada.
- Ampliaciones y componentes.
- Actividad económica o social.
- Mantenimiento y deterioro.
- Evolución arquitectónica.
- Historia y acontecimientos relevantes.

Los edificios se organizarán también en categorías y sistemas compartidos para facilitar su gestión.

Un edificio podrá transformarse, ampliarse, cambiar de función, quedar abandonado, convertirse en ruina o ser reemplazado, conservando cuando corresponda su identidad histórica.

La estructura de datos deberá permitir representar edificios con componentes internos independientes, aunque el nivel de detalle de esos componentes se implementará progresivamente.

## 6. Asentamientos

Aldeas, pueblos y ciudades serán entidades con identidad propia.

Su identidad surgirá de la interacción entre habitantes, edificios, economía, cultura, geografía e historia.

Cada asentamiento podrá tener:

- Población y composición demográfica.
- Familias y comunidades.
- Edificios e infraestructura.
- Economía y especialización.
- Instituciones.
- Cultura e identidad local.
- Religión y tradiciones.
- Organización administrativa.
- Relaciones con otros asentamientos.
- Territorio e influencia.
- Acontecimientos y memoria histórica.

Los asentamientos podrán crecer, cambiar de categoría, dividirse, fusionarse, ser abandonados o desaparecer.

Su identidad no dependerá exclusivamente de su población actual: podrá conservar tradiciones, edificios, nombres y legados de habitantes anteriores.

## 7. Regiones y territorios

Las regiones se representarán como entidades geográficas, económicas, culturales y administrativas.

Estas dimensiones no necesariamente coincidirán.

Por ejemplo:
- Una región cultural puede abarcar varias regiones políticas.
- Una provincia puede contener diferentes identidades culturales.
- Una región económica puede conectar asentamientos de distintos territorios.
- Una frontera administrativa puede cambiar sin alterar inmediatamente la identidad regional.

Las regiones tendrán características, recursos, población, asentamientos, instituciones, relaciones y evolución histórica propias.

Se integrarán con el sistema de Administración Territorial y Gestión de Regiones.

## 8. Organizaciones e instituciones

Las organizaciones aparecerán gradualmente cuando existan las condiciones históricas y sociales necesarias.

No estarán limitadas a una lista rígida de estructuras predeterminadas.

Podrán existir:

- Familias extensas y clanes.
- Gremios y asociaciones de artesanos.
- Empresas y organizaciones comerciales.
- Instituciones religiosas.
- Gobiernos y organismos administrativos.
- Ejércitos y organizaciones militares.
- Escuelas, universidades y academias.
- Movimientos políticos.
- Organizaciones culturales.
- Asociaciones comunitarias.
- Organizaciones diplomáticas y supranacionales.

Cada organización podrá tener:

- Identidad y propósito.
- Integrantes.
- Estructura interna.
- Liderazgo.
- Propiedades y recursos.
- Actividades.
- Influencia.
- Relaciones externas.
- Normas y organización.
- Historia y acontecimientos.

Las organizaciones podrán surgir, crecer, transformarse, dividirse, fusionarse o desaparecer.

## 9. Relaciones entre entidades

El mundo utilizará un sistema de relaciones múltiples y dinámicas.

Una misma entidad podrá mantener numerosas relaciones simultáneas con otras.

Ejemplos:

- Un individuo pertenece a una familia.
- Vive en un hogar.
- Trabaja en un edificio.
- Es miembro de un gremio.
- Practica una religión.
- Tiene relaciones personales con otros habitantes.
- Posee bienes.
- Participa en una organización política.

Las relaciones tendrán, cuando corresponda:

- Tipo.
- Entidades involucradas.
- Fecha de inicio.
- Estado actual.
- Intensidad o importancia.
- Condiciones.
- Origen.
- Acontecimientos asociados.
- Fecha de finalización.

Las relaciones podrán evolucionar y conservar memoria histórica.

Por ejemplo, una familia puede perder su propiedad, una empresa puede cambiar de dueño, un individuo puede abandonar una organización o una ciudad puede dejar de pertenecer a un determinado territorio.

El sistema deberá permitir consultar tanto las relaciones actuales como las relaciones históricas relevantes.

## 10. Identidad e historia persistente

Las entidades importantes conservarán su identidad a través del tiempo.

Se priorizará el historial de:

- Personajes relevantes.
- Edificios históricos.
- Asentamientos y ciudades.
- Organizaciones.
- Instituciones.
- Regiones.
- Civilizaciones.

El historial podrá registrar:

- Fundación y origen.
- Transformaciones.
- Propietarios y responsables.
- Cambios de función.
- Relaciones relevantes.
- Acontecimientos.
- Destrucciones y reconstrucciones.
- Evolución cultural y arquitectónica.
- Legados.

No se exige almacenar permanentemente cada acción cotidiana de cada entidad. La persistencia histórica deberá ser selectiva y configurable, evitando un crecimiento innecesario de datos.

El objetivo es poder reconstruir la historia de los elementos importantes del mundo.

## 11. Escalabilidad y simulación adaptativa

La simulación será adaptativa según:

- Importancia de la entidad.
- Cantidad de habitantes.
- Actividad económica.
- Complejidad de las relaciones.
- Acontecimientos recientes.
- Relevancia para el jugador.
- Configuración de simulación.

No se utilizará únicamente la distancia al jugador como criterio.

Una ciudad lejana que atraviesa una guerra importante puede necesitar más detalle que una región cercana sin acontecimientos relevantes.

Las entidades podrán pasar entre distintos niveles de simulación, conservando los datos necesarios para mantener continuidad y coherencia.

La transición entre simulación detallada y agregada no deberá producir saltos demográficos, económicos o históricos artificiales.

## 12. Integración con otros sistemas

El modelo será compartido por todos los sistemas.

Ejemplos:

- **Población:** individuos, familias, hogares, grupos y clases sociales.
- **Economía:** propietarios, productores, trabajadores, empresas y bienes.
- **Producción:** edificios, trabajadores, recetas y organizaciones productivas.
- **Construcción:** edificios, materiales, propietarios, arquitectos y obras.
- **Tecnología:** especialistas, instituciones, conocimientos y descubrimientos.
- **Religión:** individuos, comunidades, instituciones y organizaciones religiosas.
- **Cultura:** individuos, grupos, asentamientos y tradiciones.
- **Política:** gobiernos, líderes, instituciones y facciones.
- **Diplomacia:** civilizaciones, líderes, diplomáticos y tratados.
- **Guerra:** ejércitos, comandantes, asentamientos y territorios.
- **Exploración:** expediciones, individuos, organizaciones y regiones.

Los sistemas deberán consultar y modificar las mismas entidades, respetando las reglas de cada dominio.

## 13. Propiedad y titularidad

Se distinguirán conceptos que no deben confundirse:

- Titularidad de recursos naturales.
- Derechos de explotación.
- Propiedad de edificios e instalaciones.
- Propiedad de bienes y mercancías.
- Control operativo de actividades.
- Administración y regulación estatal.

Esto permite mantener la decisión de que los recursos naturales pertenecen al Estado o civilización, mientras productores privados, familias y organizaciones pueden administrar actividades, operar instalaciones y gestionar bienes conforme a los derechos establecidos.

La titularidad y el control podrán evolucionar históricamente.

## 14. Interfaz y consulta

El jugador podrá consultar entidades y sus relaciones desde el mundo 3D y desde herramientas específicas.

Se contemplan:

- Fichas individuales.
- Árboles familiares.
- Organigramas.
- Relaciones entre organizaciones.
- Historiales de edificios y ciudades.
- Cronologías.
- Mapas territoriales.
- Redes económicas y sociales.
- Biografías de personajes.

La profundidad de información dependerá de la complejidad elegida.

## 15. Complejidad configurable

El sistema deberá permitir ajustar:

- Nivel de detalle individual.
- Agregación demográfica.
- Profundidad de relaciones.
- Persistencia histórica.
- Simulación de organizaciones.
- Seguimiento de personajes.
- Frecuencia de actualización.
- Detalle de entidades lejanas.

La configuración de detalle no deberá modificar arbitrariamente las reglas fundamentales del mundo.

## 16. MVP

El primer prototipo deberá contemplar:

- Individuos identificables.
- Familias y hogares básicos.
- Edificios individuales.
- Asentamientos.
- Relaciones entre habitantes, hogares y edificios.
- Organizaciones simples.
- Propiedad básica.
- Historial elemental de entidades.
- Capacidad de agrupar población.
- Identificadores persistentes.

El MVP no necesita implementar todas las relaciones sociales, organizaciones históricas ni biografías completas. Debe establecer una estructura extensible que permita incorporarlas sin reconstruir el modelo central.

## 17. Criterios de aceptación

El sistema será conceptualmente satisfactorio cuando:

1. Un habitante pueda tener identidad, familia, trabajo, vivienda y relaciones propias.
2. Las familias y hogares puedan cambiar con el tiempo.
3. Los edificios mantengan identidad e historial.
4. Las ciudades desarrollen identidad propia.
5. Las regiones puedan tener dimensiones culturales, económicas, geográficas y políticas diferentes.
6. Las organizaciones puedan surgir y evolucionar orgánicamente.
7. Las relaciones puedan cambiar sin perder su historia relevante.
8. Las entidades importantes puedan consultarse y seguirse.
9. La simulación pueda escalar sin romper la continuidad del mundo.
10. Todos los sistemas compartan entidades y datos coherentes.

## 18. Principio rector

**El mundo no estará formado por objetos aislados, sino por entidades con identidad, relaciones y memoria. Cada habitante, edificio, ciudad y organización será parte de una historia común que evoluciona a lo largo de los siglos.**
