# PRD — cJSON JSON DOM Port

**Pack:** `09-cjson` · **Upstream:** [DaveGamble/cJSON](https://github.com/DaveGamble/cJSON) · **License:** MIT

---

## 1. Synopsis / problem statement

Port cJSON’s ANSI C JSON **DOM** (parse, print, navigate, mutate) to a memory-safe **Rust** design (idiomatic `serde_json` façade *or* pedagogical API-mimic) or **C++ RAII** (`nlohmann/json`-style ownership). Success is behavioral parity on a golden corpus—including escapes, nesting limits, number formatting policy, and (where claimed) allocator failure—not a from-scratch “JSON library wishlist.”

## 2. Goals & non-goals

**Goals**

- Parse objects/arrays/strings/numbers/bools/null into an owned tree.
- Print/minify (and optionally prettify) with roundtrip tests vs upstream.
- Navigation helpers equivalent to `GetObjectItem` / array access.
- Creation/mutation helpers sufficient for the P0 test corpus.
- Document number formatting and key-order policies.

**Non-goals**

- JSON5, comments, streaming SAX, or JSON Schema.
- Guaranteeing every historical cJSON compile flag on day one—publish a matrix.
- Binary-identical pointer layouts with C cJSON.
- Full weekend spent on prettifier cosmetics while parse edges fail.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `value` | Owned JSON value enum/class (object/array/prims) |
| `parse` | Bytes → value; depth/escape errors |
| `print` | Value → bytes (minified / optional pretty) |
| `access` | Get by key/index; iterate |
| `mutate` | Add/replace/detach/delete (as needed for P0) |
| `alloc` (optional) | Hooks / arenas if mimicking cJSON malloc hooks |
| `tests/diff` | Oracle differential harness |

**SOLID mapping (prose)**

- **S:** Printer does not parse; accessors do not own I/O.
- **O:** New print modes (pretty) extend printer without changing the value model.
- **L:** Any `Value` backend used in tests must honor the same access trait/interface.
- **I:** Callers needing only parse+get should not depend on mutation or malloc hooks.
- **D:** Tests depend on a `JsonDom` port so an FFI oracle adapter can sit beside the implementation.

## 4. EPICs

1. **Bootstrap & policy** — idiomatic vs API-mimic; number/order policies locked.
2. **Parse & print MVP** — core types + minify roundtrip.
3. **Access & mutate** — object/array navigation + basic builders.
4. **Edge suite** — escapes, depth, malformed, numbers.
5. **Hardening** — OOM hooks / sanitizers / corpus CI (weekend stretch).

## 5. User stories (by epic)

### Epic 1 — Bootstrap & policy

- As a **student**, I want a written choice (idiomatic façade vs API-mimic), so that APIs stay coherent.
- As a **developer**, I want number and key-order policies documented, so that diffs are fair.

### Epic 2 — Parse & print MVP

- As a **caller**, I want to parse standard JSON into a tree, so that I can inspect values.
- As a **caller**, I want minified print roundtrips for valid fixtures, so that data survives a cycle.
- As a **caller**, I want clear errors on invalid JSON, so that I can reject bad input.

### Epic 3 — Access & mutate

- As a **caller**, I want get-by-key / get-by-index, so that I can navigate like cJSON users do.
- As a **caller**, I want to build objects/arrays programmatically, so that tests and apps can construct DOM nodes.

### Epic 4 — Edge suite

- As a **reviewer**, I want escape and Unicode fixtures green, so that strings are trustworthy.
- As a **reviewer**, I want nest-depth and malformed cases tested, so that parsers fail closed.
- As a **caller**, I want number edge cases (large exponents, `-0`, etc.) documented vs upstream.

### Epic 5 — Hardening

- As a **student**, I want optional malloc-failure / alloc-hook tests if claiming cJSON-like hooks, so that the claim is honest.
- As a **reviewer**, I want `make test-diff` (or equiv.) in CI, so that grading is objective.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Value model + scaffold | E1 | P0 |
| F2 Policy doc (numbers/order/API style) | E1 | P0 |
| F3 Parse core types | E2 | P0 |
| F4 Minify print + roundtrip | E2 | P0 |
| F5 GetObjectItem / array get | E3 | P0 |
| F6 Builders (Create*/Add*) subset | E3 | P0 |
| F7 Escapes + deep nest fixtures | E4 | P0 |
| F8 Malformed rejection | E4 | P0 |
| F9 Pretty printer | E2 | P1 |
| F10 Malloc-failure hooks | E5 | P1–P2 |
| F11 Full upstream test mirror | E5 | P1 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Parse/print primitives + empty containers.
2. **Slice B:** Nested objects/arrays roundtrip vs oracle.
3. **Slice C:** Accessors + builders for corpus construction.
4. **Slice D:** Escape/depth/malformed suite green.
5. **Slice E:** Weekend hardening (pretty, OOM, broader upstream tests).

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Policy + hello parse/print |
| **Wave 2** | Core DOM | Slice B roundtrips |
| **Wave 3** | Access/mutate | Slice C |
| **Wave 4** | Edges | Slice D (afternoon “done”) |
| **Wave 5** | Polish / weekend | Slice E items marked done or deferred |

## 9. Acceptance criteria / parity checklist outline

- [ ] Parse→minify-print roundtrip equality (or documented canonicalization) on ≥ N golden files.
- [ ] Object/array navigation works for nested fixtures.
- [ ] Escapes and nest-depth limits tested; failures are stable.
- [ ] Number policy documented; deltas vs cJSON listed if any.
- [ ] MIT attribution; oracle commit hash recorded.
- [ ] Afternoon exit: Slices A–D. Weekend: close P1 items or explicitly defer with rationale.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Afternoon overrun on edges | Ship MVP + list deferred fixtures; don’t fake green |
| `serde_json` façade hides learning | If idiomatic, still write differential tests + thin cJSON-like helpers |
| Key order / float formatting diffs | Canonicalize in test harness; document |
| Claiming OOM parity without tests | Keep hooks P2 unless tested |

**License:** MIT — retain DaveGamble/cJSON notices; new code may remain MIT. Do not remove copyright from mirrored tests.
