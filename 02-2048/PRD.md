# PRD — 2048 Game Port

**Pack:** `02-2048` · **Upstream:** [gabrielecirulli/2048](https://github.com/gabrielecirulli/2048) · **License:** MIT

---

## 1. Synopsis / problem statement

Port the classic 2048 browser game so that **game rules and scoring** match the upstream reference, while the presentation layer uses a modern SPA framework or Flutter. The educational core is extracting a **pure, testable move engine** and treating the original JS as a golden oracle—especially under a seeded RNG.

## 2. Goals & non-goals

**Goals**

- Parity for slide/merge rules, score deltas, win (2048) and lose detection.
- Seeded / injectable RNG for deterministic tests and replays.
- Playable UI (keyboard and/or swipe) in the chosen stack.
- Afternoon-scoped delivery: core first, animations optional.

**Non-goals**

- Online leaderboards, accounts, or multiplayer.
- Bit-identical CSS animations (timing may differ).
- Porting every HTML/CSS flourish before core correctness.
- Changing merge rules “for balance.”

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/grid` | 4×4 board; cell occupancy; serialize/deserialize |
| `domain/move` | Pure `move(board, direction) → { board, scoreDelta, moved }` |
| `domain/spawn` | Place random 2/4 given RNG port |
| `domain/game` | Status: playing / won / lost; score; optional keep-playing |
| `infra/rng` | Seeded PRNG adapter |
| `ui/input` | Keyboard / gesture → direction |
| `ui/board` | Render tiles; optional animation driver |

**SOLID mapping (prose)**

- **S:** Merge math lives only in `move`; rendering never mutates rules.
- **O:** New input devices (gamepad) extend `ui/input` without touching domain.
- **L:** Any RNG implementing `next() → u32/float` can replace Math.random for tests.
- **I:** UI consumes a small `GameFacade` (move, reset, score, status)—not grid internals.
- **D:** Domain depends on an RNG **port**; infrastructure supplies the adapter.

## 4. EPICs

1. **Bootstrap & shell** — app boots; static board chrome.
2. **Move engine** — slide, merge, score; no random yet (fixtures only).
3. **Spawn & lifecycle** — RNG spawn, win/lose, reset.
4. **Interactive UI** — input + rendering bound to engine.
5. **Polish** — animations, a11y basics, replay tooling.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & shell

- As a **student**, I want a runnable window with a 4×4 grid scaffold, so that I can attach the engine quickly.
- As a **reviewer**, I want the stack and oracle strategy documented, so that I can re-run diffs.

### Epic 2 — Move engine

- As a **developer**, I want pure move functions over serializable boards, so that I can golden-test without a browser.
- As a **user**, I want tiles to slide and merge exactly once per move per upstream rules, so that the game feels right.

### Epic 3 — Spawn & lifecycle

- As a **user**, I want a new tile to appear after a successful move, so that the game progresses.
- As a **user**, I want clear win and lose states, so that I know when the run ends.
- As a **developer**, I want a seedable RNG, so that replays match.

### Epic 4 — Interactive UI

- As a **user**, I want arrow keys (and optional swipe) to drive moves, so that I can play.
- As a **user**, I want to see score updates live, so that feedback is immediate.

### Epic 5 — Polish

- As a **user**, I want basic motion when tiles move, so that play is pleasant (P1).
- As a **developer**, I want a replay CLI or test harness, so that regressions are obvious.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Project scaffold + grid view | E1 | P0 |
| F2 Directional slide without merge | E2 | P0 |
| F3 Merge + scoreDelta | E2 | P0 |
| F4 Spawn 2/4 with probabilities | E3 | P0 |
| F5 Win / lose / reset | E3 | P0 |
| F6 Keyboard input binding | E4 | P0 |
| F7 Seeded RNG + fixture runner | E3/E5 | P0 |
| F8 Animations / swipe | E4–E5 | P1 |
| F9 Undo | — | P2 (usually out) |

## 7. Slices

1. **Slice A:** Empty grid UI + hardcoded fixture board render.
2. **Slice B:** Headless move engine passes fixture suite.
3. **Slice C:** Full game loop with seeded spawn; CLI or unit “play” script.
4. **Slice D:** Interactive SPA/Flutter playable end-to-end.
5. **Slice E:** Optional animations + documented known visual deltas.

## 8. Waves

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Grid renders |
| **Wave 2** | Move engine + fixtures | Diff vs upstream on fixed boards |
| **Wave 3** | Spawn + win/lose | Seeded game completes deterministically |
| **Wave 4** | UI binding | Human can play |
| **Wave 5** | Polish / edge cases | P1 animations optional; parity doc updated |

## 9. Acceptance criteria / parity checklist outline

- [ ] Fixture boards: each direction → expected board + scoreDelta (vs oracle).
- [ ] Merges happen at most once per tile per move (no chain merges in one move).
- [ ] Spawn only occurs when `moved == true`.
- [ ] Win detected at 2048; lose when no moves remain.
- [ ] Seeded sequence of N moves matches oracle grids.
- [ ] README: how to run tests + play; MIT attribution.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Animation timing distracts from rules | Defer to Wave 5 |
| Non-deterministic tests | Inject seed; ban unseeded Math.random in domain |
| Subtle merge-order bugs | Prefer differential tests over manual play alone |

**License:** MIT — credit gabrielecirulli/2048; keep notices on derived assets.
