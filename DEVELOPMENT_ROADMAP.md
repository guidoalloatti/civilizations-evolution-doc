# DEVELOPMENT_ROADMAP — Civilization Evolutions

> **Phase 2 — Hoja de ruta de implementación.** Separa el proyecto en etapas con objetivos, sistemas requeridos, dependencias, funcionalidad esperada, criterios de aceptación, riesgos y **qué NO implementar todavía**. Respeta los GDD y los MVP por sistema. No es cronograma aprobado. Fuentes: Plan técnico §17, MVP transversal (01_GDD_GENERAL §18, Maestro §3), fichas de GDD.
> Versión 1.0 · Estado: propuesta. Orden derivado de [SYSTEM_DEPENDENCY_MAP.md](SYSTEM_DEPENDENCY_MAP.md).

## Resumen de etapas

| Etapa | Nombre | Foco |
|---|---|---|
| E0 | Infraestructura fundacional | Tiempo, Entidades, Recursos, esqueleto modular |
| E1 | Primer playable | Mundo mínimo + aldea + recolección + producción + tiempo |
| E2 | Primera colonia completa | Población individual, necesidades, logística, consumo, economía básica |
| E3 | Civilización ampliada | Múltiples asentamientos, comercio regional, administración |
| E4 | Evolución histórica | Tecnología como marco compartido, evolución de edificios, cultura/religión |
| E5 | Sistemas avanzados | Política, diplomacia, IA de civilizaciones, guerra |
| E6 | Largo plazo | Eventos/historia profunda, progresión, balance, pulido, escala |

> **Estado actual del prototipo** (ver [IMPLEMENTATION_ALIGNMENT.md](IMPLEMENTATION_ALIGNMENT.md)): hay piezas de E0–E2 implementadas (reloj, recursos/recetas/producción/tecnología en SimCore; terreno, construcción con costo, settlers y recolección en UE) pero con **deuda arquitectónica** (gameplay en la capa UE, doble modelo). E3+ no iniciado.

---

## E0 — Infraestructura fundacional
- **Objetivos:** base modular con separación simulación/presentación; reloj; catálogo de recursos; modelo de entidades/identidad; eventos tipados.
- **Sistemas requeridos:** Tiempo, Entidades, Recursos, núcleo de simulación/eventos.
- **Dependencias:** ninguna (Nivel 0).
- **Funcionalidad:** avanzar/pausar/acelerar tiempo; definir recursos data-driven; crear entidades con id persistente; emitir/escuchar eventos.
- **Aceptación:** el reloj avanza independiente del FPS; se crean recursos/entidades por datos; un sistema reacciona a un evento sin acoplarse.
- **Riesgos:** RISK-01/02/06 (definir ahora dónde vive la lógica y la fuente única de verdad).
- **NO hacer aún:** economía de mercado, política, IA, render final.

## E1 — Primer playable (prototipo transversal)
- **Objetivos:** demostrar que las mecánicas base funcionan juntas (DR-12).
- **Sistemas:** Generación/Geografía mínima, Construcción, Población (individual chica), Producción, Logística simplificada, Economía básica, Tecnología básica, UI mínima.
- **Dependencias:** E0.
- **Funcionalidad (meta del GDD):** generar un mundo; iniciar una aldea; ~30 habitantes; construir viviendas y edificios productivos; asignar trabajadores; recolectar recursos; producir y consumir; almacenar y transportar; avanzar el tiempo; ver la aldea evolucionar.
- **Aceptación:** una aldea produce, transporta, almacena y consume; la población crece; se investiga algo básico y se ve una mejora.
- **Riesgos:** RISK-01/03 (no hacer un Actor pesado por habitante; mantener la lógica testeable).
- **NO hacer aún:** mercados con precios dinámicos, cultura/religión/política, diplomacia, guerra, finalización.

## E2 — Primera colonia completa
- **Objetivos:** profundidad social y económica de un asentamiento.
- **Sistemas:** Población (familias, necesidades, salud, educación básica), Economía (consumo, propiedad mixta, moneda básica), Logística física, Construcción (evolución simple).
- **Dependencias:** E1.
- **Funcionalidad:** familias y necesidades evolutivas; empleo/salarios básicos; inventarios por ubicación; cadenas productivas completas; estaciones afectan agricultura (DR-03).
- **Aceptación:** una colonia estable con demografía, trabajo y consumo coherentes durante años simulados.
- **Riesgos:** RISK-02/08/10 (unificar modelo de población; persistencia; transición LOD al crecer).
- **NO hacer aún:** múltiples civilizaciones, comercio internacional.

## E3 — Civilización ampliada
- **Objetivos:** varios asentamientos y economía regional.
- **Sistemas:** Exploración/Expansión, Administración Territorial, Transporte/Comercio (regional), Economía (mercados regionales).
- **Dependencias:** E2.
- **Funcionalidad:** fundar asentamientos; agrupar en regiones; rutas y comercio regional; especialización.
- **Aceptación:** dos+ asentamientos conectados por comercio con especialización regional.
- **Riesgos:** RISK-07/09 (cadencias; escala de mundo).
- **NO hacer aún:** otras civilizaciones autónomas, guerra.

## E4 — Evolución histórica
- **Objetivos:** que la civilización cambie visible y funcionalmente con el tiempo.
- **Sistemas:** Tecnología (marco compartido), evolución de edificios, Cultura, Religión.
- **Dependencias:** E3 + [HISTORICAL_EVOLUTION_FRAMEWORK.md](HISTORICAL_EVOLUTION_FRAMEWORK.md).
- **Funcionalidad:** descubrimientos por condiciones; edificios que evolucionan; identidad cultural/religiosa; evolución visual del mundo.
- **Aceptación:** una misma civilización se ve y funciona distinto tras avances; caminos alternativos posibles.
- **Riesgos:** RISK-05 (no efectos aislados; reacción a descubrimientos comunes).
- **NO hacer aún:** diplomacia/guerra complejas.

## E5 — Sistemas avanzados
- **Objetivos:** mundo vivo con otras civilizaciones e instituciones.
- **Sistemas:** Política, Diplomacia (completar GDD, DR-13), IA de Civilizaciones, Guerra.
- **Dependencias:** E4.
- **Funcionalidad:** gobierno/leyes con consecuencias; civilizaciones autónomas; relaciones diplomáticas; conflictos secundarios.
- **Aceptación:** civilizaciones IA evolucionan y se relacionan sin el jugador; la guerra es coherente y secundaria.
- **Riesgos:** RISK-03/06/11 (escala de IA; acoplamiento; no sobre-simular).
- **NO hacer aún:** balance final, multijugador (fuera de alcance).

## E6 — Largo plazo / producto
- **Objetivos:** historia emergente profunda, objetivos/finalización, pulido, escala.
- **Sistemas:** Eventos/Historia completa, Progresión, optimización, animación/sonido/UI final, tutoriales, contenido histórico.
- **Dependencias:** E5.
- **Funcionalidad:** crónica/legado; objetivos y límite histórico opcionales; rendimiento a gran escala.
- **Aceptación:** partida larga coherente, con historia propia y finalización opcional.
- **Riesgos:** RISK-08/09/10 (persistencia, escala, migración de entidades).
- **NO hacer aún:** nada nuevo sin decisión de producto.

## Decisiones que condicionan el roadmap (confirmar antes de avanzar)

- **E1:** DR-02 (perfil del mundo), DR-03 (clima/estaciones), DR-06 (escala), DR-12 (alcance del prototipo).
- **E2:** DR-07 (propiedad), DR-10 (LOD individual↔agregado), unificación del modelo (RISK-02).
- **E4:** DR-01 (corte histórico), DR-11 (adopción/difusión tecnológica), marco de evolución compartido.
- **E5:** DR-05 (rol del jugador), DR-13 (completar Diplomacia).
