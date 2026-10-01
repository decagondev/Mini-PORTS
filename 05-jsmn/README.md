# 05 — jsmn Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/zserge/jsmn |
| **License** | MIT |
| **From → to** | Minimal C JSON **tokenizer** → **Rust** / **Go** / **Zig** (same token-index model) |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~4.2k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

jsmn is a minimalistic JSON **tokenizer** (single header, ~471 LOC classic API): no DOM allocations—callers get a flat token array (type, start, end, size). Ideal for teaching “parse enough to navigate” and for **strict differential dumps**.

**Why it ports well:** Tiny; token stream equality is an objective oracle; partial-parse and nest-limit policies are explicit design knobs.

## Learning outcomes

- Implement a non-allocating (or arena) tokenizer with clear error codes.
- Define UTF-8 / partial-parse policy and lock it in tests.
- Build a golden corpus → token-dump JSON pipeline.
- Map C structs to idiomatic target types without changing semantics.

## How to use this pack

1. Clone upstream; treat `jsmn.h` + tests/examples as oracle.
2. New MIT-attributed port repo.
3. Follow [PRD.md](./PRD.md): bootstrap → primitive tokens → objects/arrays → errors/limits.
4. Emit the same token fields; diff dumps, don’t hand-wave “equivalent DOM.”

## Suggested verification

- Golden JSON files → canonical token dump (type/start/end/size); strict equality vs upstream.
- Malformed input cases: expect matching error behavior (document intentional divergences).
- Nesting / token-buffer exhaustion fixtures.

## Attribution / license notes

- **MIT**. Preserve copyright; cite zserge/jsmn.
- This is a **tokenizer**, not a full JSON DOM—don’t silently expand scope to `serde_json`-style trees unless the PRD wave explicitly allows a separate optional layer.

## Links

- Upstream: https://github.com/zserge/jsmn
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
