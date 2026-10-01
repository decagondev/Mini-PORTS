# 06 — inih Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/benhoyt/inih |
| **License** | BSD-3-Clause |
| **From → to** | Callback-style C INI parser → **Rust** / **Go** / modern **C++** (callback or typed reader parity) |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~3k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

inih is a small **SAX-style** INI parser (~500 LOC) with an optional C++ `INIReader` convenience wrapper. The design *is* the callback contract: section / name / value (and error reporting). Compile-time `#define`s control multiline values, inline comments, etc.—your port must **document which flags you honor**.

**Why it ports well:** Fixture-driven; tiny core; callback parity is easy to assert; optional reader is a clean second module (SRP).

## Learning outcomes

- Port a callback/SAX API without collapsing into an ad-hoc map too early.
- Feature-flag matrix tests (multiline, comments, BOM, etc.).
- Layer a typed reader on top of the parser (OCP / SRP).
- Keep `examples/test.ini`-style fixtures as golden input.

## How to use this pack

1. Clone upstream; run C examples against fixtures as oracle.
2. New BSD-3-Clause-attributed port.
3. Follow [PRD.md](./PRD.md): parser callbacks first, then optional `INIReader`-alike.
4. Publish a short FLAGS.md (or README section) listing honored options.

## Suggested verification

- Replay upstream `examples/test.ini` (and extended fixtures) → ordered callback traces.
- Matrix: each feature flag on/off vs documented behavior.
- Error line numbers / handler abort semantics.

## Attribution / license notes

- **BSD-3-Clause** — include license text; respect non-endorsement clause.
- If you only port the C callback core, say so; the C++ reader is a separate deliverable.

## Links

- Upstream: https://github.com/benhoyt/inih
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
