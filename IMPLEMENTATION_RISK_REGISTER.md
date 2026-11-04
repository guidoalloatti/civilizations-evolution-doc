# IMPLEMENTATION_RISK_REGISTER — Civilization Evolutions

> **Phase 2 — Registro de riesgos de arquitectura y desarrollo.** Para cada riesgo: por qué importa, sistemas afectados, consecuencias, mitigación y cuándo abordarlo. No es implementación. Relacionado con [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) (sección E) e [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md).
> Versión 1.1 · Última actualización 2026-11-04 · Severidad: 🔴 alta · 🟠 media · 🟡 baja.
> **¿Bloquea MVP?** = si no mitigarlo **impide** alcanzar el primer playable (Escenarios A–D / roadmap E1–E2). *Sí* = hay que atacarlo antes o durante el MVP; *No* = puede diferirse sin bloquear el MVP (pero sigue siendo deuda). Esto es ortogonal a la severidad: un riesgo puede ser 🔴 y **no** bloquear el MVP si solo aparece a gran escala.

| ID | Riesgo | Sev | ¿Bloquea MVP? | Cuándo abordar |
|---|---|---|---|---|
| RISK-01 | Lógica de simulación en la capa de presentación (UE) | 🔴 | **Sí** | Ya (antes de ampliar gameplay) |
| RISK-02 | Duplicación de datos (doble modelo de población/economía) | 🔴 | **Sí** | Ya |
| RISK-03 | Rendimiento con miles de entidades (un Actor por habitante) | 🔴 | No (MVP ~20–30) | Antes de escalar población |
| RISK-04 | Contenido hardcodeado en vez de data-driven | 🟠 | No | Al crecer el catálogo |
| RISK-05 | Evolución histórica "scripteada" / efectos tecnológicos aislados | 🟠 | Parcial | Antes de implementar Tecnología a fondo |
| RISK-06 | Acoplamiento fuerte entre sistemas | 🟠 | **Sí** (marca el patrón desde el inicio) | Continuo |
| RISK-07 | Frecuencia de actualización excesiva (todo por Tick) | 🟠 | No | Continuo |
| RISK-08 | Complejidad de guardar/cargar al crecer el estado | 🟠 | No | Antes de persistir sistemas nuevos |
| RISK-09 | Escalabilidad de la generación procedural | 🟡 | No | Al pasar de mundos chicos a grandes |
| RISK-10 | Migración de entidades persistentes (individual↔agregado, versiones) | 🟠 | No | Al introducir LOD y evolución |
| RISK-11 | Simulación demasiado compleja para el prototipo (sobre-ingeniería) | 🟠 | **Sí** | Continuo |
| RISK-12 | Trabajo sin commitear / dos sesiones sobre el mismo árbol | 🟠 | **Sí** | Ya |
| RISK-13 | **Determinismo / reproducibilidad** de la simulación | 🟠 | Parcial | Al definir el reloj y la serialización |
| RISK-14 | **Huella de memoria** del estado persistente (individuos/historia) | 🟠 | No | Al escalar población/siglos |
| RISK-15 | **Streaming de mapas grandes** (mundo no residente en memoria) | 🟡 | No | Al pasar a mundos grandes |
| RISK-16 | **Compatibilidad de saves a largo plazo** (versiones de esquema) | 🟠 | No | Antes de publicar builds con saves que importen |
| RISK-17 | **Costo de la IA** de civilizaciones no jugadoras | 🟠 | No | Al introducir IA estratégica |
| RISK-18 | **Actualizaciones en cascada** / tormentas de feedback entre sistemas | 🟠 | Parcial | Al conectar economía↔población↔política |
| RISK-19 | **Depurar comportamiento emergente** (reproducir/entender bugs sistémicos) | 🟠 | **Sí** (sin esto, el MVP es indepurable) | Ya (herramientas desde el inicio) |

**Bloqueantes de MVP (atacar ya o durante el MVP):** RISK-01, RISK-02, RISK-06, RISK-11, RISK-12, RISK-19 (+ parcialmente RISK-05/13/18). El resto es deuda diferible que **no** impide el primer playable.

---

### RISK-01 — Simulación acoplada a la presentación 🔴
- **Por qué importa:** el plan técnico exige separar simulación de render; es lo que permite escalar y testear. El gameplay nuevo (settlers, nodos, stock de colonia, producción de edificios) vive en la **capa UE**, no en SimCore.
- **Afecta:** Población, Economía, Producción, Logística.
- **Consecuencias:** queda fuera de los tests de SimCore; difícil de escalar; difícil de simular sin render.
- **Mitigación:** definir el **hogar** de la lógica de colonia (recomendado: bajar a SimCore — DR pendiente) y dejar en UE solo presentación/entrada.
- **Cuándo:** antes de ampliar gameplay (cada feature nueva agranda la deuda).

### RISK-02 — Duplicación de datos 🔴
- **Por qué importa:** coexisten dos modelos de población/economía (agregado en SimCore ↔ individual/colonia en UE), contra "una identidad por sistema" (DC-07/DC-15).
- **Afecta:** Población, Recursos, Economía, Entidades.
- **Consecuencias:** estados divergentes, bugs de consistencia, lógica repetida.
- **Mitigación:** una sola fuente de verdad por entidad/recurso; unificar los dos modelos (ver SHARED_DATA_MODEL + DR-07/DR-10).
- **Cuándo:** ya.

### RISK-03 — Rendimiento con miles de entidades 🔴
- **Por qué importa:** `CivUnit` es un **Actor por habitante**; el plan pide explícitamente no hacerlo y soportar agregación.
- **Afecta:** Población, Tiempo, render.
- **Consecuencias:** a 20 va bien; a cientos/miles cae el rendimiento.
- **Mitigación:** nivel de simulación agregada (LOD) por tamaño/distancia/relevancia (DR-10); representar individuos como datos, con actores solo para los visibles/activos.
- **Cuándo:** antes de subir la población objetivo.

### RISK-04 — Contenido hardcodeado 🟠
- **Por qué importa:** recursos/edificios/recetas/tecnologías viven en C++ (`DefaultContent`), no en Data Tables/Assets.
- **Afecta:** Recursos, Producción, Construcción, Tecnología.
- **Consecuencias:** balancear/añadir contenido obliga a recompilar; difícil de iterar por diseño.
- **Mitigación:** migrar gradualmente a Data Tables/Data Assets (plan §4.2).
- **Cuándo:** cuando el catálogo crezca.

### RISK-05 — Evolución histórica scripteada / efectos aislados 🟠
- **Por qué importa:** si cada sistema implementa los efectos de una tecnología por su cuenta, el resultado es rígido y difícil de extender (contra DC-01/DC-09).
- **Afecta:** Tecnología, Producción, Construcción, Militar, Cultura.
- **Consecuencias:** árbol tecnológico rígido; reescritura al añadir sistemas.
- **Mitigación:** marco **compartido** de descubrimientos/condiciones/eventos al que los sistemas **reaccionan** (ver [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md)).
- **Cuándo:** antes de implementar Tecnología a fondo.

### RISK-06 — Acoplamiento fuerte entre sistemas 🟠
- **Por qué importa:** la visión pide que los sistemas interactúen sin conocer su implementación interna.
- **Afecta:** todos.
- **Consecuencias:** cambios locales rompen otros sistemas; difícil de testear aislado.
- **Mitigación:** comunicación por eventos tipados + interfaces; respetar el mapa de autoridad (Maestro §5).
- **Cuándo:** continuo.

### RISK-07 — Frecuencia de actualización excesiva 🟠
- **Por qué importa:** actualizar todo cada frame no escala.
- **Afecta:** todos.
- **Consecuencias:** CPU desperdiciada; caídas de FPS.
- **Mitigación:** cadencias por sistema + event-driven (ver SYSTEM_DEPENDENCY_MAP §E); el reloj independiente del FPS ya existe.
- **Cuándo:** continuo.

### RISK-08 — Complejidad de guardar/cargar 🟠
- **Por qué importa:** SimCore ya serializa; al sumar entidades/relaciones/historia, la persistencia se complica.
- **Afecta:** Entidades, Eventos, todos los que persisten.
- **Consecuencias:** saves frágiles o incompatibles entre versiones.
- **Mitigación:** identidad estable + versionado de save; serializar el modelo compartido, no copias por sistema.
- **Cuándo:** antes de persistir sistemas nuevos.

### RISK-09 — Escalabilidad de la generación procedural 🟡
- **Por qué importa:** el GDD contempla mundos grandes y variados; el terreno actual es un parche chico.
- **Afecta:** Generación, Geografía, render.
- **Consecuencias:** generación/mallas lentas o pesadas en mundos grandes.
- **Mitigación:** generación por capas/chunks, semilla reproducible, LOD de terreno.
- **Cuándo:** al pasar de mundos chicos a grandes.

### RISK-10 — Migración de entidades persistentes 🟠
- **Por qué importa:** pasar individuos a grupos (y viceversa) y evolucionar edificios conservando identidad es complejo.
- **Afecta:** Población, Entidades, Construcción.
- **Consecuencias:** pérdida de identidad/historia; inconsistencias al agregar/desagregar.
- **Mitigación:** identidad conceptual estable independiente del nivel de simulación (DR-10; Entidades §11).
- **Cuándo:** al introducir LOD y evolución histórica.

### RISK-11 — Sobre-ingeniería / sobre-simplificación 🟠
- **Por qué importa:** el plan pide evitar abstracciones vacías y, a la vez, no simplificar la arquitectura para que "entre" el prototipo.
- **Afecta:** todos.
- **Consecuencias:** complejidad inútil o rediseño caro.
- **Mitigación:** implementar lo pedido por etapa; mantener extensibilidad sin construir sistemas no solicitados.
- **Cuándo:** continuo.

### RISK-12 — Trabajo sin commitear / dos sesiones sobre el mismo árbol 🟠
- **Por qué importa:** todo lo posterior al commit HANDOFF está sin commitear; dos sesiones editan el mismo árbol (último-en-guardar gana).
- **Afecta:** todo el código.
- **Consecuencias:** pérdida de trabajo, colisiones.
- **Mitigación:** commitear en checkpoints verdes; una sola sesión escribe gameplay a la vez; el repo de docs ya está versionado aparte.
- **Cuándo:** ya.

### RISK-13 — Determinismo / reproducibilidad 🟠 · ¿Bloquea MVP? Parcial
- **Por qué importa:** una simulación histórica que guarda/carga y que algún día podría replicar o depurar necesita que, dada la misma semilla y entradas, produzca el mismo resultado. Punto flotante, orden de iteración de contenedores y RNG mal sembrado rompen esto.
- **Afecta:** Tiempo, todos los sistemas con azar (demografía, eventos, IA), guardar/cargar.
- **Consecuencias:** bugs irreproducibles; saves que divergen; imposible comparar "misma partida" entre builds.
- **Mitigación:** un RNG con semilla explícita y por-subsistema; evitar depender del orden de `TMap`/punteros; fijar el orden de actualización; aislar lo no determinista (render) de la simulación.
- **Cuándo:** al definir el reloj y la serialización (barato ahora, caro después).

### RISK-14 — Huella de memoria del estado persistente 🟠 · ¿Bloquea MVP? No
- **Por qué importa:** individuos con historia + registro histórico creciente + varias civilizaciones pueden consumir mucha RAM a lo largo de siglos.
- **Afecta:** Población, Entidades, Eventos/Registro histórico.
- **Consecuencias:** crecimiento de memoria sin techo; partidas largas que se degradan.
- **Mitigación:** agregación (DR-10) libera detalle; resumir/compactar historia antigua; estructuras compactas (SoA, ids en vez de punteros); presupuestos de memoria por subsistema.
- **Cuándo:** al escalar población/duración (no en el MVP de ~20–30).

### RISK-15 — Streaming de mapas grandes 🟡 · ¿Bloquea MVP? No
- **Por qué importa:** un mundo grande no cabe residente en memoria ni conviene simularlo entero al detalle.
- **Afecta:** Generación, Geografía, Logística, render.
- **Consecuencias:** picos de memoria/carga; hitches al moverse por el mundo.
- **Mitigación:** chunks con carga/descarga; simulación agregada de zonas lejanas (coherente con LOD); separar estado de simulación de mallas de render.
- **Cuándo:** al pasar de mundos chicos a grandes (relacionado con RISK-09).

### RISK-16 — Compatibilidad de saves a largo plazo 🟠 · ¿Bloquea MVP? No
- **Por qué importa:** el esquema del estado cambiará muchas veces; sin versionado, cada cambio invalida los saves existentes.
- **Afecta:** Entidades, Eventos, todos los que persisten.
- **Consecuencias:** pérdida de partidas al actualizar; migraciones ad-hoc frágiles.
- **Mitigación:** número de versión de esquema + migradores por versión; campos opcionales con defaults; tests de carga de saves antiguos. Distinto de RISK-08 (que es la *complejidad estructural*; esto es *evolución del esquema en el tiempo*).
- **Cuándo:** antes de publicar builds cuyos saves importen a usuarios.

### RISK-17 — Costo de la IA de civilizaciones 🟠 · ¿Bloquea MVP? No
- **Por qué importa:** cada civilización no jugadora decide (economía, diplomacia, guerra, expansión); hacerlo cada tick y para muchas no escala.
- **Afecta:** IA, Diplomacia, Economía, Guerra.
- **Consecuencias:** CPU saturada con pocas civilizaciones; o IA tan barata que es tonta.
- **Mitigación:** IA por cadencia baja/por hito (ver SYSTEM_DEPENDENCY_MAP §E); profundidad por relevancia/cercanía (IA §8); presupuesto de tiempo por frame repartido entre civilizaciones.
- **Cuándo:** al introducir IA estratégica (post-MVP).

### RISK-18 — Actualizaciones en cascada / tormentas de feedback 🟠 · ¿Bloquea MVP? Parcial
- **Por qué importa:** economía↔población↔política se retroalimentan; un evento puede disparar una cascada que oscile o explote (precios que rebotan, migraciones en bucle).
- **Afecta:** Economía, Población, Política, Eventos.
- **Consecuencias:** inestabilidad numérica, bucles infinitos de eventos, comportamiento no plausible.
- **Mitigación:** amortiguación/inercia en los modelos; procesar cascadas por cadencia (no recursión inmediata); límites y detección de ciclos; resolver en pasos discretos del reloj.
- **Cuándo:** al conectar los tres núcleos (empieza en el MVP con economía↔población).

### RISK-19 — Depurar comportamiento emergente 🟠 · ¿Bloquea MVP? Sí
- **Por qué importa:** en una simulación integrada, los bugs surgen de la interacción de muchos sistemas y a menudo horas adentro; sin herramientas, son casi imposibles de reproducir y entender.
- **Afecta:** todos.
- **Consecuencias:** tiempo de desarrollo perdido; bugs que no se pueden aislar; pérdida de confianza en la simulación.
- **Mitigación:** inspección del estado por entidad; línea de tiempo/registro de eventos consultable (lo aporta el sistema de Eventos); reproducibilidad (RISK-13); logs por cadencia; modo paso a paso del reloj; escenarios de validación ([CROSS_SYSTEM_VALIDATION_SCENARIOS.md](CROSS_SYSTEM_VALIDATION_SCENARIOS.md)) como pruebas repetibles.
- **Cuándo:** ya — las herramientas de depuración deben nacer con la simulación, no después.
