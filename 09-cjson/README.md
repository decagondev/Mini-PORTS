# 09 — cJSON Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/DaveGamble/cJSON |
| **License** | MIT |
| **From → to** | ANSI C JSON **DOM** → Rust (`serde_json` idioms *or* API-mimic crate) / C++ RAII (`nlohmann/json`-style) |
| **Effort band** | **Afternoon–Weekend** (~4–10 h; edge cases push past a pure afternoon) |
| **~Stars** | ~13k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

cJSON is an ultralightweight ANSI C JSON library (~3.5k LOC core): parse/print, tree navigation (`GetObjectItem`, arrays), creation helpers, and careful malloc-failure / nesting / escape edge cases. Unlike jsmn (tokenizer-only), this is a **DOM** port—richer surface, heavier oracle.

**Why it ports well:** Famous ops map cleanly; upstream tests are a ready differential mine; you can ship an afternoon MVP (parse/print/get) and spend weekend hours on escapes, depth, numbers, and OOM paths.

## Learning outcomes

- Port a C tree API to memory-safe ownership (Rust) or RAII (C++).
- Differential-test parse→print roundtrips against the upstream binary/harness.
- Bound nesting depth, malformed input, and allocator failure as first-class acceptance.
- Avoid scope explosion into JSON5 / streaming SAX unless marked P2.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle** (include tests).
2. Create a **new MIT-attributed** port repo; cite DaveGamble/cJSON.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Prefer **differential** parse/print vs upstream over “looks like JSON.”
5. Decide early: **idiomatic** target (`serde_json` + thin façade) vs **API-mimic** pedagogy crate—document the choice.

## Suggested verification

- Reuse or mirror upstream test vectors (license allows); parse→print roundtrip diffs.
- Malformed / deep / escaped / large-number fixtures with documented parity or intentional deltas.
- Optional: run sanitizers / Rust miri on alloc paths; simulate malloc failure if API-mimic includes hooks.
- Golden corpus of nested objects/arrays with key-order policy stated (cJSON order vs sorted).

## Attribution / license notes

- Upstream is **MIT**. Keep copyright; add yours on new files.
- Do not strip headers from translated tests derived from upstream.
- Record oracle commit hash; note any test files copied.

## Links

- Upstream: https://github.com/DaveGamble/cJSON
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
