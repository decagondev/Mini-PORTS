# 02 — 2048 Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/gabrielecirulli/2048 |
| **License** | MIT |
| **From → to** | Vanilla JS + CSS → React / Vue / Svelte SPA, **or** Flutter/Dart |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~13.4k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

Classic browser 2048 (~1.6k LOC JS/CSS/HTML). The interesting part is a **pure, deterministic game core** (`GameManager` / `Grid` / `Tile`): given a grid and a direction, the next grid is fully determined (modulo RNG for new tiles). That makes differential tests and seeded replays excellent oracles.

**Why it ports well:** Tiny surface; logic separable from DOM/CSS; animations and polish are optional stretch goals.

## Learning outcomes

- Extract pure functions from DOM-coupled game code.
- Design seeded RNG / replay for reproducible tests.
- Port presentation last (CSS or Flutter widgets) after core parity.
- Keep upstream JS runnable as a headless oracle where possible.

## How to use this pack

1. Clone upstream as a **read-only oracle**.
2. New fork-and-port repo; MIT attribution + NOTICE.
3. Follow [PRD.md](./PRD.md): Wave 1 shell → core `move` → UI → polish.
4. Implement `move(grid, direction)` (and spawn rules) before any framework chrome.
5. Port CSS / animations last; do not block on pixel fidelity.

## Suggested verification

- **Differential:** same seeded sequence of moves → compare grids / scores vs upstream.
- Snapshot fixtures: fixed boards + direction → expected board JSON.
- Manual: win (2048 tile), lose (no moves), score increments, undo if in scope (usually P2).

## Attribution / license notes

- Upstream **MIT** (gabrielecirulli/2048). Preserve attribution in UI “About” if you ship one.
- “2048” as a game concept is widely cloned; still credit the reference implementation you ported from.

## Links

- Upstream: https://github.com/gabrielecirulli/2048
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
