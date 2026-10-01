# PRD — jsmn JSON Tokenizer Port

**Pack:** `05-jsmn` · **Upstream:** [zserge/jsmn](https://github.com/zserge/jsmn) · **License:** MIT

---

## 1. Synopsis / problem statement

Port jsmn’s **minimal JSON tokenizer** (token index model: type, start, end, size) to **Rust**, **Go**, or **Zig**. Success is strict **token-dump equality** against upstream on a golden corpus—not building a competing DOM parser. Partial parses, nest limits, and error codes are part of the product contract and must be documented.

## 2. Goals & non-goals

**Goals**

- Parse objects, arrays, strings, primitives into a flat token array matching jsmn’s model.
- Support caller-provided token buffer / count (or idiomatic equivalent with same failure modes).
- Differential tests: corpus → dump JSON/CSV of tokens → diff vs upstream binary/harness.
- Clear policy for UTF-8 string contents and partial JSON.

**Non-goals**

- Full DOM construction, queries, or `serde`-style deserialization (optional separate layer only if time remains—and it is not P0).
- JSON5 / comments / trailing commas.
- Claiming RFC 8259 strictness beyond what upstream actually enforces—**match upstream**, then note deltas.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `token` | Token type enum + struct (start/end/size/parent if used) |
| `parser` | State machine over input bytes |
| `error` | Error codes / insufficient-tokens / invalid |
| `dump` | Canonical token serializer for tests |
| `ffi_oracle` (optional) | Call upstream jsmn for differential CI |

**SOLID mapping (prose)**

- **S:** Dump/formatting for tests is not mixed into parser hot path modules.
- **O:** New token types are out of scope; extending error detail should not break the token schema.
- **L:** Parser can run over `&[u8]` / `[]byte` without requiring files.
- **I:** Callers need `parse(input, tokens) → (count, status)`—not a kitchen-sink options blob.
- **D:** Tests depend on a `Tokenizer` interface so an oracle adapter can sit beside the port.

## 4. EPICs

1. **Bootstrap & schema lock** — token struct + dump format frozen.
2. **Primitives & strings** — numbers, literals, strings with escapes you support.
3. **Structures** — arrays/objects + `size` relationships.
4. **Errors & limits** — truncated input, bad tokens, exhausted token slots.
5. **Corpus CI** — differential suite + docs.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & schema lock

- As a **developer**, I want a frozen token dump schema, so that diffs are stable.
- As a **student**, I want a hello parse of `{}` and `[]`, so that the pipeline works end-to-end early.

### Epic 2 — Primitives & strings

- As a **caller**, I want strings and primitives tokenized with correct start/end, so that slices reconstruct values.
- As a **caller**, I want escape handling to match documented upstream behavior, so that corpus tests pass.

### Epic 3 — Structures

- As a **caller**, I want object keys/values and array elements represented with correct `size`, so that tree walking is possible without a DOM.
- As a **caller**, I want nested structures to tokenize in the same order as jsmn, so that dumps match.

### Epic 4 — Errors & limits

- As a **caller**, I want distinct outcomes for invalid JSON vs insufficient token slots, so that I can resize and retry like upstream.
- As a **caller**, I want partial/truncated behavior documented and tested, so that streaming-ish use is predictable.

### Epic 5 — Corpus CI

- As a **reviewer**, I want a `make test-diff` (or equivalent) against fixtures, so that grading is objective.
- As a **student**, I want intentional deltas listed, so that slight policy differences are not silent.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Token types + dump | E1 | P0 |
| F2 Parse empty containers | E1 | P0 |
| F3 Primitives + strings | E2 | P0 |
| F4 Objects/arrays + size | E3 | P0 |
| F5 Escapes subset | E2 | P0 |
| F6 Error codes parity | E4 | P0 |
| F7 Token exhaustion | E4 | P0 |
| F8 Differential corpus | E5 | P0 |
| F9 DOM helper (optional) | — | P2 |

## 7. Slices

1. **Slice A:** Dump pipeline + parse `{}` / `[]` / `null`.
2. **Slice B:** Numbers, `true`/`false`, simple strings.
3. **Slice C:** Nested object/array fixtures match oracle dumps.
4. **Slice D:** Error + exhaustion fixtures green.
5. **Slice E:** Full corpus gate in CI script.

## 8. Waves

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Schema + harness runs |
| **Wave 2** | Primitives | Slice B |
| **Wave 3** | Structures | Slice C |
| **Wave 4** | Errors/limits | Slice D |
| **Wave 5** | Polish / edge cases | Corpus CI; deltas documented |

## 9. Acceptance criteria / parity checklist outline

- [ ] Token dump equality on ≥ N golden files (include nested + escapes).
- [ ] Insufficient-tokens path tested (retry with larger buffer if that’s the API).
- [ ] Invalid JSON rejected with stable error.
- [ ] No DOM required for P0.
- [ ] MIT attribution; upstream commit hash recorded.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Accidental DOM scope creep | P2 only; keep tokenizer crate/lib pure |
| UTF-8 strictness mismatch | Document “match jsmn, not stricter” unless flagged |
| Float/number boundary issues | Prefer raw start/end slices over parsing numbers in P0 |

**License:** MIT — keep copyright; cite zserge/jsmn.
