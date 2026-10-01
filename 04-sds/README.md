# 04 — SDS Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/antirez/sds |
| **License** | BSD-2-Clause |
| **From → to** | C dynamic strings → modern **C++** (`std::string` / SSO builder) or **Rust** `String` with SDS-like pedagogy APIs |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~5.6k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

SDS (“simple dynamic strings”) is the Redis-famous C string type: length-prefixed, binary-safe, heap-efficient. The README is effectively a tutorial; the header API is the curriculum.

**Why it ports well:** Small, famous API; golden examples in docs; easy to table-map ops → target equivalents. Decide early: **API-compatible spirit** vs **binary layout / Redis drop-in** (drop-in is usually out of scope for students).

## Learning outcomes

- Distinguish pedagogical API parity from ABI/layout compatibility.
- Handle binary-safe lengths and embedded null bytes in tests.
- Design clear module boundaries (alloc / header / public ops).
- Fuzz or property-test concatenation and range ops.

## How to use this pack

1. Clone upstream as oracle; extract ops table from `sds.h` / README.
2. New port repo under BSD-2-Clause attribution.
3. Follow [PRD.md](./PRD.md); implement create/len/avail → mutate → format helpers → free.
4. State in README: “spirit-compatible SDS API” unless you truly match layout.

## Suggested verification

- Golden tests transcribed from README examples.
- Differential smoke: same op sequences vs upstream where FFI harness is feasible.
- Fuzz with embedded `\0`; assert `len` ≠ `strlen` behavior preserved.

## Attribution / license notes

- **BSD-2-Clause**. Do not claim Redis drop-in unless binary layout and alloc headers match—and say so explicitly if they do not.

## Links

- Upstream: https://github.com/antirez/sds
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
