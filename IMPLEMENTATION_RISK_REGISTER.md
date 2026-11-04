# IMPLEMENTATION_RISK_REGISTER — Civilization Evolutions

> **Phase 2 — Registro de riesgos de arquitectura y desarrollo.** Para cada riesgo: por qué importa, sistemas afectados, consecuencias, mitigación y cuándo abordarlo. No es implementación. Relacionado con [DESIGN_CONSISTENCY_AUDIT.md](DESIGN_CONSISTENCY_AUDIT.md) (sección E) e [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md).
> Versión 1.0 · Severidad: 🔴 alta · 🟠 media · 🟡 baja.

| ID | Riesgo | Sev | Cuándo abordar |
|---|---|---|---|
| RISK-01 | Lógica de simulación en la capa de presentación (UE) | 🔴 | Ya (antes de ampliar gameplay) |
| RISK-02 | Duplicación de datos (doble modelo de población/economía) | 🔴 | Ya |
| RISK-03 | Rendimiento con miles de entidades (un Actor por habitante) | 🔴 | Antes de escalar población |
| RISK-04 | Contenido hardcodeado en vez de data-driven | 🟠 | Al crecer el catálogo |
| RISK-05 | Evolución histórica "scripteada" / efectos tecnológicos aislados | 🟠 | Antes de implementar Tecnología a fondo |
| RISK-06 | Acoplamiento fuerte entre sistemas | 🟠 | Continuo |
| RISK-07 | Frecuencia de actualización excesiva (todo por Tick) | 🟠 | Continuo |
| RISK-08 | Complejidad de guardar/cargar al crecer el estado | 🟠 | Antes de persistir sistemas nuevos |
| RISK-09 | Escalabilidad de la generación procedural | 🟡 | Al pasar de mundos chicos a grandes |
| RISK-10 | Migración de entidades persistentes (individual↔agregado, versiones) | 🟠 | Al introducir LOD y evolución |
| RISK-11 | Simulación demasiado compleja para el prototipo (sobre-ingeniería) | 🟠 | Continuo |
| RISK-12 | Trabajo sin commitear / dos sesiones sobre el mismo árbol | 🟠 | Ya |

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
