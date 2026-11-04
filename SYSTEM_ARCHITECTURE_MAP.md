# SYSTEM_ARCHITECTURE_MAP — Civilization Evolutions

> Mapa **conceptual** de la arquitectura de juego: sistemas, responsabilidades, relaciones, entidades compartidas y flujos principales. No es una especificación técnica final. Fuente de contratos: [GDD_MAESTRO §5–6](docs/00-project/GDD_MAESTRO.md); detalle por dominio en los GDD de sistema (ver [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)).

## Principio rector

El mundo se simula como **un sistema coherente**, no como features sueltas. Los sistemas comparten **entidades y recursos con identidad única**, se coordinan por un **reloj común** y se comunican por **eventos**, sin que cada sistema conozca la implementación interna de los demás. La **lógica de simulación** (reglas) se mantiene **separada de la presentación** (render/UI).

## Capas conceptuales

```mermaid
flowchart TB
    subgraph FOUND["Fundaciones (datos y coordinación)"]
        TIME["Tiempo y Simulación<br/>(reloj global único)"]
        ENT["Entidades y Relaciones<br/>(identidad persistente)"]
        RES["Recursos y Materiales<br/>(catálogo único)"]
        GEO["Geografía / Clima / Medioambiente"]
        GEN["Generación Procedural del Mundo"]
    end
    subgraph CORE["Simulación civil (núcleo del juego)"]
        POP["Población y Sociedad"]
        ECO["Economía"]
        PROD["Producción y Trabajo"]
        LOG["Transporte, Comercio y Logística"]
        CON["Construcción y Arquitectura"]
    end
    subgraph SOC["Sociedad e instituciones"]
        CUL["Cultura e Identidad"]
        REL["Religión"]
        POL["Política y Gobierno"]
        ADM["Administración Territorial"]
    end
    subgraph WORLD["Mundo vivo"]
        TEC["Tecnología, Ciencia e Innovación"]
        EXP["Exploración y Expansión"]
        DIP["Diplomacia"]
        AI["IA y Autonomía de Civilizaciones"]
        MIL["Guerra y Sistemas Militares"]
        EVT["Eventos y Evolución Histórica"]
    end
    subgraph PRES["Presentación y meta"]
        UI["Interfaz UI/UX"]
        PRG["Progresión y Finalización"]
    end

    TIME --> CORE
    TIME --> WORLD
    ENT --> CORE
    ENT --> SOC
    RES --> ECO
    RES --> PROD
    RES --> CON
    RES --> LOG
    GEN --> GEO
    GEO --> CORE
    GEO --> EXP
    GEO --> CON

    POP --> ECO
    ECO --> PROD
    PROD --> LOG
    LOG --> ECO
    CON --> POP
    PROD --> CON

    TEC --> PROD
    TEC --> CON
    TEC --> MIL
    TEC --> LOG
    POL --> ADM
    POL --> ECO
    CUL --- REL
    REL --> POP
    CUL --> POP
    ADM --> POP

    DIP --- AI
    AI --> WORLD
    MIL --> EVT
    DIP --> EVT
    EXP --> ADM

    CORE --> UI
    SOC --> UI
    WORLD --> UI
    UI --> PRG
    CORE --> EVT
    SOC --> EVT
    EVT --> PRG
```

(Las flechas indican "provee datos / habilita / condiciona"; las líneas `---` son relaciones bidireccionales fuertes.)

## Responsabilidades (dueño de cada decisión)

| Sistema | Responsabilidad (de qué es autoridad) |
|---|---|
| **Tiempo y Simulación** | Reloj global único, calendario, velocidades, ciclos internos de distinta frecuencia. Coordina a todos. |
| **Entidades y Relaciones** | Identidad persistente y relaciones de habitantes, familias, edificios, asentamientos, regiones y organizaciones. Base compartida. |
| **Recursos y Materiales** | Catálogo e inventarios únicos, unidades, moneda, calidad/deterioro/almacenamiento. |
| **Geografía/Clima/Medioambiente** | Estado y procesos naturales: relieve, hidrología, biomas, clima, recursos naturales, flora/fauna, desastres. |
| **Generación del Mundo** | Estado inicial procedural del mapa + editor. Consume el modelo climático/recursos. |
| **Población y Sociedad** | Dinámica demográfica y social: individuos, familias, clases, necesidades, salud, educación, migración. |
| **Economía** | Propiedad, precios, mercados, consumo, moneda, tesorería, impuestos. |
| **Producción y Trabajo** | Edificios productivos, recetas, productividad, empleo, calidad, evolución funcional/visual de edificios. |
| **Transporte/Comercio/Logística** | Movimiento físico de recursos, rutas, infraestructura, mercados físicos, comercio. |
| **Construcción y Arquitectura** | Obra, materiales constructivos, urbanismo, arquitectura, patrimonio, soporte físico. |
| **Cultura / Religión / Política / Administración** | Identidad y prácticas / creencias e instituciones espirituales / poder, leyes, instituciones / organización territorial. |
| **Tecnología** | Conocimiento e invenciones que habilitan métodos y cambian capacidades (sin adopción automática). |
| **Exploración** | Adquisición y circulación del conocimiento espacial; ocupación/colonización. |
| **Diplomacia / IA / Guerra** | Relaciones y tratados / decisiones autónomas de civilizaciones / conflicto armado. |
| **Eventos** | Registra hechos producidos por los sistemas dueños; memoria y cronología. No crea efectos sin regla propietaria. |
| **Interfaz / Progresión** | Presenta estado y órdenes (no es autoridad de reglas) / objetivos, puntuación y finalización opcionales. |

## Entidades y datos compartidos (no duplicar)

- **Recurso/material:** un identificador e inventario común, usado por Economía, Producción, Construcción, Logística, Comercio, Tecnología, Guerra. (Dueño: Recursos y Materiales.)
- **Entidad del mundo** (habitante, familia, edificio, asentamiento, región, organización): identidad persistente e historial compartidos. (Dueño: Entidades y Relaciones.)
- **Reloj/fecha:** única fuente temporal para todos los ciclos. (Dueño: Tiempo.)
- **Conocimiento/tecnología:** habilita capacidades en varios dominios; disponibilidad ≠ adopción. (Dueño: Tecnología.)
- **Registro histórico:** hechos verificables emitidos por los sistemas dueños. (Dueño: Eventos.)

## Flujos principales

**1. Cadena productiva + logística + consumo**
```mermaid
flowchart LR
    GEO["Geografía: recursos en el mapa"] --> EXTR["Extracción (recolector, cantera, tala)"]
    EXTR --> INV1["Inventario de edificio"]
    INV1 --> TRANSP["Transporte físico (logística)"]
    TRANSP --> PROD["Transformación (molino, panadería)"]
    PROD --> STORE["Almacén / depósito"]
    STORE --> MKT["Mercado / distribución"]
    MKT --> POP["Consumo de la población"]
    POP -. necesidades/felicidad .-> ECO["Economía (precios, demanda)"]
    ECO -. prioridades .-> PROD
```
Regla: **nada se consume en destino si no fue entregado físicamente** (logística real, simplificada).

**2. Tecnología como disparador transversal**
```mermaid
flowchart LR
    TEC["Descubrimiento / tecnología"] --> PRODC["Métodos de producción"]
    TEC --> CONC["Materiales y arquitectura"]
    TEC --> LOGC["Vehículos / infraestructura"]
    TEC --> MILC["Capacidad militar"]
    TEC --> SOCC["Organización social"]
    PRODC --> VIS["Evolución visible de edificios y mundo"]
    CONC --> VIS
```
Regla: la tecnología **habilita**; la adopción y sus reglas pertenecen a cada dominio.

**3. El tiempo como eje de coordinación**
```mermaid
flowchart TB
    CLK["Reloj global único"] --> D["ciclo diario"]
    CLK --> S["estaciones"]
    CLK --> Y["años"]
    D --> POPt["Población / necesidades"]
    S --> AGR["Agricultura / producción estacional"]
    Y --> TECt["Investigación / evolución histórica"]
    S --> ENVt["Clima / medioambiente"]
    Y --> EVTt["Evaluación de acontecimientos"]
```

## Reglas de integración (del Maestro §6)

- La **interfaz** presenta, no decide; las notificaciones nacen de cambios simulados.
- **Eventos** consume hechos reales; no inventa efectos.
- **Recursos** no se copian por sistema; identidad única.
- **Población** agregada debe mantener coherencia con la población total.
- La **disponibilidad** tecnológica no implica adopción automática.
- Ningún sistema impone una **cronología histórica obligatoria**.

> Para el estado de qué está implementado de este mapa, ver [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md).
