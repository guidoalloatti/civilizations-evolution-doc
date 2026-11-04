# GIT_WORKFLOW — Civilization Evolutions

> **Flujo de trabajo Git y de workspace para desarrollo multi-agente.** Define ramas, worktrees, commits, reporte, integración, conflictos, tests y entrega. Diseñado para que varios agentes trabajen en paralelo **sin pisarse** ni corromper binarios de Unreal.
>
> Versión 1.0 · 2026-11-04 · Estado: PROPUESTA. No se crean ni borran worktrees sin verificar el estado del repo.

## 0. Realidad del repositorio (leer primero)

**Hay DOS repositorios git distintos:**

| Repo | Raíz | Contiene | Remoto |
|---|---|---|---|
| **Docs** | `Civilization Evolutions/` | GDD, blueprint Phase 2, documentos de organización (estos) | repo de docs (rama actual `ga/multi-agent-dev-org-911442`, base `main`) |
| **UE** | `civilizations-evolution/` | SimCore + capa UE + `Content/` | `origin/main` (del proyecto UE) |

- `civilizations-evolution/` está en el **`.gitignore` del repo de docs** → **no aparece** en los worktrees del repo de docs (como este). Cada repo se versiona por separado.
- **Estado actual del repo UE (verificado 2026-11-04):** rama `main`, **8 commits por delante de `origin/main`** (sin pushear), y **cambios sin commitear** (`CivilizationSimulator.Build.cs`, `Core/CivGameModeBase.cpp`, `Core/CivHUD.cpp` modificados; `UI/` y `Docs/UI_STYLE_GUIDE.md` sin trackear). **RISK-12.**
- **Git LFS instalado (3.8.1) pero NO configurado** (no hay `.gitattributes`): los `.uasset` se versionan hoy como blobs normales.

> **Regla cero (M0-CORE-01):** antes de abrir ramas por workstream, **commitear y pushear** el trabajo pendiente del repo UE para partir de un árbol limpio. No ramificar sobre trabajo sin commitear.

## 1. Modelo de ramas

**No todos trabajan en la misma rama** (restricción del brief). Esquema por workstream, en **cada** repo donde trabaje:

```
main (UE)  /  main (docs)
  └── integration/<repo>        ← rama de integración que mantiene WS-TD
        ├── ws/core/<tarea>     ← WS-CORE
        ├── ws/econ/<tarea>     ← WS-ECON
        ├── ws/build/<tarea>    ← WS-BUILD
        ├── ws/world/<tarea>    ← WS-WORLD
        ├── ws/pop/<tarea>      ← WS-POP
        ├── ws/ui/<tarea>       ← WS-UI
        ├── ws/art/<tarea>      ← WS-ART
        └── ws/qa/<tarea>       ← WS-QA
```

- **Nombre de rama:** `ws/<workstream>/<task-id>-<slug>` (ej. `ws/build/M1-BUILD-01-unify-placement`).
- **`integration/<repo>`:** rama donde WS-TD integra y corre build+tests antes de promover a `main`. `main` solo recibe lo ya integrado y verde.
- **Una tarea = una rama = un workstream.** Nada de ramas compartidas entre workstreams.

## 2. Worktrees (aislamiento físico)

Para que dos agentes no editen el mismo *working tree* a la vez (RISK-12), cada workstream activo usa su **propio git worktree** del repo correspondiente.

- **Verificar antes de crear** (`git worktree list`) y **no borrar** worktrees ajenos. Crear: `git worktree add ../wt-<workstream>-<task> ws/<workstream>/<task>`.
- **SimCore es C++ puro** → worktrees baratos y sin binarios: ideal para WS-CORE/ECON/BUILD/POP/WORLD en lo que toca `SimCore/`.
- **La capa UE trae binarios** (`Content/*.uasset`) → ver §5. Un worktree UE no debe editar los `.uasset` de otro.
- **`Intermediate/`, `Saved/`, `DerivedDataCache/`, `Binaries/`** no se versionan (ya en `.gitignore`); cada worktree UE regenera lo suyo.
- **Limpieza:** al terminar e integrar, el dueño retira su worktree (`git worktree remove`) tras confirmar que no quedó trabajo sin commitear.

## 3. Cómo arranca un agente una tarea

1. Leer la tarea en [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md): archivos permitidos/restringidos, dependencias, criterios.
2. Confirmar que sus dependencias están `done` y sus contratos publicados ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)).
3. **Leer el estado en disco** de los archivos que va a tocar (el código evoluciona; no asumir).
4. Marcar la tarea `in-progress` y "tomar" cualquier archivo 🤝 compartido en el tablero.
5. Crear rama + worktree (§1/§2) desde `integration/<repo>` actualizado.

## 4. Commits

- **Incrementos chicos → compilar en verde → commitear.** No acumular cambios grandes sin commitear (RISK-12).
- **Solo archivos propios** (según [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md)). Si hay que tocar un 🤝 compartido, cambio mínimo y coordinado.
- **Separar commits de código y de arte** (`.uasset`), para poder revertir visual sin tocar lógica.
- **Mensaje:** `<task-id>: <qué cambió>` en español, imperativo. Terminar con la línea de atribución del proyecto.
- **Nunca commitear** `Intermediate/Saved/DerivedDataCache/Binaries/`, `.sln`, archivos de IDE, ni `ArtStaging/` (todos ya ignorados).

## 5. Binarios de Unreal (`.uasset`) — merge imposible

Los `.uasset`/`.umap` **no se fusionan** textualmente. Reglas (ver también [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) §binarios):

1. **Propiedad única por asset.** Hoy el arte está en `Content/Chars` y `Content/PBR` (ambos WS-ART); **no hay `.umap` ni `.uasset` de UI** → no hay un asset grande compartido. Mantenerlo así: cada `.uasset` pertenece a una sola tarea.
2. **Nunca dos agentes editan el mismo `.uasset` a la vez.** El tablero asigna el asset a una tarea.
3. **Configurar LFS antes de sumar binarios pesados** (tarea M0-TD-03, **requiere aprobación** por ser cambio de config): crear `.gitattributes` (`*.uasset filter=lfs diff=lfs merge=lfs -text`, ídem `*.umap`, `*.fbx`, texturas) y migrar los existentes (`git lfs migrate import`).
4. Si se adopta LFS, usar **file locking** (`git lfs lock <asset>` / `git lfs unlock`) en assets que más de un workstream podría querer, para impedir ediciones simultáneas.
5. Si igual ocurre un conflicto binario: **no** auto-resolver; el dueño del asset regenera su versión y WS-TD elige cuál queda (nunca un merge a ciegas).

## 6. Cómo reportar cambios

- Al terminar: push de la rama, marcar la tarea `review` en el tablero y abrir **solicitud de integración** (PR o mensaje a WS-TD) con: task-id, archivos tocados, contratos afectados, cómo se probó (build + tests), y riesgos.
- Si se tocó un contrato ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)): decirlo explícito y listar consumidores impactados.
- Si se tomó una decisión de arquitectura/diseño: proponerla a WS-TD para registrarla en `DESIGN_DECISION_REGISTER.md` / `AUDIT_REPORT.md` (no cambiar GDD confirmados en silencio).

## 7. Solicitar integración

Ver [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md). Resumen del lado del agente:
- La rama debe estar **al día con `integration/<repo>`** (rebase o merge de integración hacia la rama propia primero).
- Build verde + tests verdes **en la rama propia** antes de pedir integración.
- WS-TD integra en `integration/<repo>`, corre build+tests de nuevo, y promueve a `main` cuando queda verde.

## 8. Conflictos

- **Código (texto):** resolver en la rama propia tras traer `integration/<repo>`; preferir cambios aditivos; si el conflicto es en un 🤝 compartido, coordinar con el dueño.
- **Contrato:** si dos tareas cambian la misma firma, **para** y escala a WS-TD; no resolver unilateralmente.
- **Binario:** §5 (no merge; el dueño regenera).
- **M0 reduce conflictos de raíz** al partir `Simulation.cpp`/`World.cpp` por dominio.

## 9. Tests antes de entregar

- **SimCore:** `cmake -S SimCore -B SimCore/build -DCMAKE_BUILD_TYPE=Release && cmake --build SimCore/build -j && ./SimCore/build/simcore_tests` (87 checks en verde) — obligatorio para cualquier cambio que toque `SimCore/`.
- **Capa UE:** build del editor (`Build.bat CivilizationSimulatorEditor Win64 Development`, editor cerrado) + tests de automation donde existan (WS-QA los agrega).
- **Determinismo:** cambios en reloj/serialización corren el test `save_load_roundtrip` (RISK-13/16).

## 10. Entrega de trabajo completado

1. Rama al día + build/tests verdes.
2. Solicitud de integración aceptada por WS-TD → merge a `integration/<repo>` → promoción a `main`.
3. Tarea `done` en el tablero; `IMPLEMENTATION_ALIGNMENT.md` actualizado.
4. Worktree retirado; ramas de tarea borradas tras merge.

## 11. Qué NO hacer

- No ramificar sobre trabajo sin commitear; no commitear binarios transitorios.
- No editar archivos fuera de los permitidos de tu tarea.
- No crear/borrar worktrees sin verificar el estado del repo ni tocar los de otros.
- No resolver conflictos de contrato o binarios a ciegas.
- No pushear a `main` directo (pasa por `integration/<repo>` y WS-TD).
- No cambiar `.gitignore`/`.gitattributes`/config de build sin aprobación (WS-TD).
