# DIPLOMACIA — Propuesta de completado (BORRADOR A VALIDAR)

> ⚠️ **NO CANÓNICO.** Este documento es un **borrador propuesto** para cubrir la parte **truncada** del `GDD_Diplomacia_y_Relaciones_Internacionales_v0.1` (corta a mitad de §18). Responde a **DR-13 / F-22** y fue autorizado por el usuario el **2026-11-04** solo como *propuesta a validar*.
> - **No** se editó el GDD original (los originales no se tocan).
> - **No** es contenido confirmado: nada de acá es canónico hasta tu validación explícita.
> - Está **derivado** de lo que el propio GDD ya fija (hasta §18), de IA (DC-11) y Guerra (DC-10). Cada punto que **no** está respaldado por la fuente va marcado como 🟡 **supuesto**.
> Al validarse, se promueve a `confirmed` en [DESIGN_DECISION_REGISTER.md](DESIGN_DECISION_REGISTER.md) y recién entonces se vuelca al GDD dueño.

## Qué falta en la fuente
El archivo corta en **§18 "Personalidad de los líderes"** tras listar rasgos (Ambición, Pragmatismo, Desconfianza, Carisma) y **no** incluye: el resto de §18, una sección de **MVP**, los **niveles de complejidad** (que otros GDD sí traen) ni una tabla de **decisiones confirmadas**. Esta propuesta cubre esos cuatro huecos.

---

## 1. Completar §18 — Personalidad de los líderes (propuesto)

**Anclado en:** §18 ya enumera rasgos; §1 (principios) y §17.4 (filosofía: la diplomacia es herramienta real, la guerra es posibilidad no objetivo); IA §4/§11 (DC-11: civilizaciones con las **mismas reglas** que el jugador, **sin omnisciencia**).

- **Rasgos como parámetros, no como guion.** Cada líder tiene valores en un conjunto de rasgos (los ya listados + 🟡 posibles: lealtad, belicosidad, apertura cultural/religiosa, tolerancia al riesgo). Modulan **cómo** una civilización persigue sus objetivos (§9), no **qué** puede hacer: no otorgan información ni acciones que el jugador no tendría (coherente con DC-11).
- **Influencia concreta** (🟡 propuesto, a calibrar): sesgan la disposición a negociar vs escalar (§17.2), el peso de la memoria histórica (§3) en decisiones, la probabilidad de romper tratados (§7.1), la agresividad territorial (§10) y el tono de la comunicación.
- **Asignación:** al surgir un líder, rasgos derivados de 🟡 cultura/religión de la civilización (§13/§14), su historia reciente (§3) y variación individual. Reproducible por semilla (ver RISK-13 en el registro de riesgos).
- **Evolución:** los rasgos son **estables pero no fijos**; eventos fuertes (traición, guerra prolongada, prosperidad) pueden desplazarlos lentamente. El cambio de líder puede reorientar la política exterior sin borrar la **memoria histórica** de la civilización (§3), que persiste por encima del individuo.
- **Transparencia al jugador:** la personalidad se **infiere** por el comportamiento observado y la inteligencia (§12), no se lee como dato omnisciente (coherente con DC-11 y §12).

---

## 2. MVP de Diplomacia (propuesto)

**Anclado en:** el patrón "MVP por sistema" del resto de GDD y la ubicación de Diplomacia en **etapa E5** del [DEVELOPMENT_ROADMAP.md](DEVELOPMENT_ROADMAP.md) (no bloquea el primer playable).

Incluye (mínimo viable):
- Relaciones bilaterales con un **estado de relación** (§2.1) que sube/baja por acciones y por **memoria histórica** básica (§3).
- Un puñado de **tratados** esenciales: paz/no agresión, comercio, alianza simple (§6.1/§6.2/§6.3), con **duración y ruptura** (§7) y consecuencias básicas.
- **Negociación nivel "Básico"** (§8): oferta/contraoferta simple, sin mecánicas avanzadas.
- Integración con **IA** (una contraparte autónoma con objetivos, §9) y con **Guerra** solo en el límite (casus belli → conflicto, DC-10).
- Personalidad de líderes en forma **mínima**: 2–3 rasgos que sesgan la IA.

Queda **fuera del MVP** (niveles avanzados, ya insinuados en el GDD): espionaje (§12), organizaciones internacionales (§15), estados dependientes/vasallaje (§16), diplomacia religiosa/cultural fina (§13/§14), mediación/arbitraje multiparte (§17.3).

---

## 3. Niveles de complejidad (propuesto, estilo del resto de GDD)

| Nivel | Alcance |
|---|---|
| **Básico** | Relaciones bilaterales, tratados esenciales, negociación simple, 2–3 rasgos de líder. |
| **Intermedio** | Más tipos de tratado (territoriales §6.4, religiosos/culturales §6.5), influencia/prestigio (§11), disputas fronterizas (§10), personalidad completa. |
| **Avanzado** | Espionaje (§12), organizaciones internacionales (§15), estados dependientes (§16), diplomacia multiparte, memoria histórica profunda. |

---

## 4. Decisiones confirmadas — PROPUESTAS (a validar)

> Marcadas `proposed`: ninguna es canónica hasta tu OK. Las que solo transcriben algo ya escrito en el GDD llevan 📄 (riesgo bajo); las 🟡 son propuestas nuevas (requieren más criterio).

| ID | Decisión propuesta | Base |
|---|---|---|
| DP-01 📄 | La diplomacia es una herramienta real de gestión de intereses; la guerra es **posibilidad, no objetivo**. | §17.4 + DC-10 |
| DP-02 📄 | Las relaciones tienen **memoria histórica** que evoluciona y condiciona decisiones futuras. | §3 |
| DP-03 📄 | Las civilizaciones actúan por **objetivos e intereses** propios (§9), con las mismas reglas que el jugador y sin omnisciencia. | §9 + DC-11 |
| DP-04 🟡 | La **personalidad del líder** es un conjunto de parámetros que **modula** la conducta, nunca otorga información/acciones extra. | §18 (parcial) + DC-11 |
| DP-05 🟡 | La personalidad es **estable pero evolutiva**; la memoria de la civilización **persiste** por encima del líder. | 🟡 propuesto |
| DP-06 🟡 | El MVP de Diplomacia es **bilateral + tratados esenciales + negociación básica**, en etapa **E5**; lo avanzado (espionaje, organizaciones, vasallaje) queda post-MVP. | roadmap E5 + patrón MVP |

---

## Cómo proceder
1. Revisá §1–§4 de esta propuesta.
2. Decime qué aceptás / cambiás / rechazás (o si tenés el texto original para reemplazarla).
3. Lo aceptado se marca `confirmed` en el registro y se vuelca al GDD dueño con fecha/versión; lo demás queda como propuesta o se descarta.
