# 03 — linenoise Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/antirez/linenoise |
| **License** | BSD-2-Clause |
| **From → to** | Classic C (readline-lite) → idiomatic **C++17+** (RAII, `string_view`) or **Rust** (`termios`) |
| **Effort band** | **Afternoon** (~2–4 h) — happy path; UTF-8 / Windows PTY are stretch |
| **~Stars** | ~4.4k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

linenoise is a guerrilla **line-editing** library (~2.5k LOC C): history, completion callbacks, basic terminal raw mode—without the weight of GNU readline. The public surface in `linenoise.h` is small and inventory-friendly.

**Why it ports well:** Single responsibility; few files; demos give byte-comparable I/O. Hard parts are escape sequences, UTF-8, and optional Windows PTYs—not business logic sprawl.

## Learning outcomes

- Map a C header API to safe ownership (RAII / Rust lifetimes).
- Separate terminal backend from editing state (SOLID: DIP for TTY I/O).
- Property-test history and completion callbacks.
- Practice BSD attribution and keeping demos as oracles.

## How to use this pack

1. Clone upstream read-only; inventory `linenoise.h` into an API table.
2. New port repo; retain BSD-2-Clause notices.
3. Follow [PRD.md](./PRD.md): bootstrap → raw mode + read line → history → completion → edge polish.
4. Compare demo programs’ observable output (where deterministic) against upstream builds.

## Suggested verification

- Build upstream `example.c` (or equivalent) as oracle; scripted input via PTY/`expect`.
- Unit tests for history ring, completion callback wiring, and key decoding tables.
- Stretch: UTF-8 cursor width; document Windows as out-of-scope unless Wave N.

## Attribution / license notes

- **BSD-2-Clause** — keep copyright; redistributions need license text.
- Do not claim GNU readline compatibility.

## Links

- Upstream: https://github.com/antirez/linenoise
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
