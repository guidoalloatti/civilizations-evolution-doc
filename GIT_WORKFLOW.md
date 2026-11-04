# GIT_WORKFLOW — Civilization Evolutions

> **Git and workspace workflow for multi-agent development.** Defines branches, worktrees, commits, reporting, integration, conflicts, tests and delivery. Designed so several agents can work in parallel **without stepping on each other** or corrupting Unreal binaries.
>
> Version 1.0 · 2026-11-04 · Status: PROPOSAL. Worktrees are neither created nor deleted without verifying the repo state.

## 0. Repository reality (read first)

**There are TWO distinct git repositories:**

| Repo | Root | Contains | Remote |
|---|---|---|---|
| **Docs** | `Civilization Evolutions/` | GDDs, Phase 2 blueprint, organization documents (these) | docs repo (current branch `ga/multi-agent-dev-org-911442`, base `main`) |
| **UE** | `civilizations-evolution/` | SimCore + UE layer + `Content/` | `origin/main` (of the UE project) |

- `civilizations-evolution/` is in the **docs repo's `.gitignore`** → it **does not appear** in the docs repo's worktrees (like this one). Each repo is versioned separately.
- **Current state of the UE repo (verified 2026-11-04):** branch `main`, **8 commits ahead of `origin/main`** (unpushed), and **uncommitted changes** (`CivilizationSimulator.Build.cs`, `Core/CivGameModeBase.cpp`, `Core/CivHUD.cpp` modified; `UI/` and `Docs/UI_STYLE_GUIDE.md` untracked). **RISK-12.**
- **Git LFS installed (3.8.1) but NOT configured** (there is no `.gitattributes`): `.uasset` files are versioned today as regular blobs.

> **Rule zero (M0-CORE-01):** before opening workstream branches, **commit and push** the UE repo's pending work to start from a clean tree. Do not branch over uncommitted work.

## 1. Branch model

**Not everyone works on the same branch** (brief constraint). Scheme by workstream, in **each** repo it works in:

```
main (UE)  /  main (docs)
  └── integration/<repo>        ← integration branch maintained by WS-TD
        ├── ws/core/<task>      ← WS-CORE
        ├── ws/econ/<task>      ← WS-ECON
        ├── ws/build/<task>     ← WS-BUILD
        ├── ws/world/<task>     ← WS-WORLD
        ├── ws/pop/<task>       ← WS-POP
        ├── ws/ui/<task>        ← WS-UI
        ├── ws/art/<task>       ← WS-ART
        └── ws/qa/<task>        ← WS-QA
```

- **Branch name:** `ws/<workstream>/<task-id>-<slug>` (e.g. `ws/build/M1-BUILD-01-unify-placement`).
- **`integration/<repo>`:** the branch where WS-TD integrates and runs build+tests before promoting to `main`. `main` only receives what is already integrated and green.
- **One task = one branch = one workstream.** No branches shared between workstreams.

## 2. Worktrees (physical isolation)

So that two agents do not edit the same *working tree* at once (RISK-12), each active workstream uses its **own git worktree** of the corresponding repo.

- **Verify before creating** (`git worktree list`) and **do not delete** others' worktrees. Create: `git worktree add ../wt-<workstream>-<task> ws/<workstream>/<task>`.
- **SimCore is pure C++** → cheap, binary-free worktrees: ideal for WS-CORE/ECON/BUILD/POP/WORLD in what touches `SimCore/`.
- **The UE layer carries binaries** (`Content/*.uasset`) → see §5. A UE worktree must not edit another's `.uasset` files.
- **`Intermediate/`, `Saved/`, `DerivedDataCache/`, `Binaries/`** are not versioned (already in `.gitignore`); each UE worktree regenerates its own.
- **Cleanup:** when done and integrated, the owner removes their worktree (`git worktree remove`) after confirming no work was left uncommitted.

## 3. How an agent starts a task

1. Read the task in [AGENT_TASK_BOARD.md](AGENT_TASK_BOARD.md): allowed/restricted files, dependencies, criteria.
2. Confirm its dependencies are `done` and its contracts are published ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)).
3. **Read the state on disk** of the files it is going to touch (code evolves; do not assume).
4. Mark the task `in-progress` and "take" any 🤝 shared file on the board.
5. Create branch + worktree (§1/§2) from an up-to-date `integration/<repo>`.

## 4. Commits

- **Small increments → compile green → commit.** Do not accumulate large uncommitted changes (RISK-12).
- **Only your own files** (per [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md)). If a 🤝 shared file must be touched, make a minimal, coordinated change.
- **Separate code and art commits** (`.uasset`), so you can revert visuals without touching logic.
- **Message:** `<task-id>: <what changed>` in Spanish, imperative. End with the project's attribution line.
- **Never commit** `Intermediate/Saved/DerivedDataCache/Binaries/`, `.sln`, IDE files, or `ArtStaging/` (all already ignored).

## 5. Unreal binaries (`.uasset`) — merge impossible

`.uasset`/`.umap` files are **not** merged textually. Rules (see also [AGENT_OWNERSHIP_MAP.md](AGENT_OWNERSHIP_MAP.md) §binaries):

1. **Single ownership per asset.** Today the art is in `Content/Chars` and `Content/PBR` (both WS-ART); **there is no `.umap` nor UI `.uasset`** → there is no large shared asset. Keep it that way: each `.uasset` belongs to a single task.
2. **Never two agents editing the same `.uasset` at once.** The board assigns the asset to a task.
3. **Configure LFS before adding heavy binaries** (task M0-TD-03, **requires approval** as it is a config change): create `.gitattributes` (`*.uasset filter=lfs diff=lfs merge=lfs -text`, same for `*.umap`, `*.fbx`, textures) and migrate the existing ones (`git lfs migrate import`).
4. If LFS is adopted, use **file locking** (`git lfs lock <asset>` / `git lfs unlock`) on assets more than one workstream might want, to prevent simultaneous edits.
5. If a binary conflict happens anyway: do **not** auto-resolve; the asset owner regenerates their version and WS-TD picks which one stays (never a blind merge).

## 6. How to report changes

- When done: push the branch, mark the task `review` on the board, and open an **integration request** (PR or message to WS-TD) with: task-id, files touched, contracts affected, how it was tested (build + tests), and risks.
- If a contract ([SYSTEM_CONTRACTS.md](SYSTEM_CONTRACTS.md)) was touched: say so explicitly and list impacted consumers.
- If an architecture/design decision was made: propose it to WS-TD to record it in `DESIGN_DECISION_REGISTER.md` / `AUDIT_REPORT.md` (do not silently change confirmed GDDs).

## 7. Requesting integration

See [INTEGRATION_STRATEGY.md](INTEGRATION_STRATEGY.md). Summary on the agent's side:
- The branch must be **up to date with `integration/<repo>`** (rebase or merge integration into your own branch first).
- Green build + green tests **on your own branch** before requesting integration.
- WS-TD integrates into `integration/<repo>`, runs build+tests again, and promotes to `main` once it is green.

## 8. Conflicts

- **Code (text):** resolve on your own branch after pulling `integration/<repo>`; prefer additive changes; if the conflict is in a 🤝 shared file, coordinate with the owner.
- **Contract:** if two tasks change the same signature, **stop** and escalate to WS-TD; do not resolve unilaterally.
- **Binary:** §5 (no merge; the owner regenerates).
- **M0 reduces conflicts at the root** by splitting `Simulation.cpp`/`World.cpp` by domain.

## 9. Tests before delivering

- **SimCore:** `cmake -S SimCore -B SimCore/build -DCMAKE_BUILD_TYPE=Release && cmake --build SimCore/build -j && ./SimCore/build/simcore_tests` (87 green checks) — mandatory for any change that touches `SimCore/`.
- **UE layer:** editor build (`Build.bat CivilizationSimulatorEditor Win64 Development`, editor closed) + automation tests where they exist (WS-QA adds them).
- **Determinism:** changes to clock/serialization run the `save_load_roundtrip` test (RISK-13/16).

## 10. Delivery of completed work

1. Up-to-date branch + green build/tests.
2. Integration request accepted by WS-TD → merge to `integration/<repo>` → promotion to `main`.
3. Task `done` on the board; `IMPLEMENTATION_ALIGNMENT.md` updated.
4. Worktree removed; task branches deleted after merge.

## 11. What NOT to do

- Do not branch over uncommitted work; do not commit transient binaries.
- Do not edit files outside your task's allowed set.
- Do not create/delete worktrees without verifying the repo state, nor touch others'.
- Do not resolve contract or binary conflicts blindly.
- Do not push to `main` directly (it goes through `integration/<repo>` and WS-TD).
- Do not change `.gitignore`/`.gitattributes`/build config without approval (WS-TD).
