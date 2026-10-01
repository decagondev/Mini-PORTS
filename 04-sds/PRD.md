# PRD — SDS Dynamic Strings Port

**Pack:** `04-sds` · **Upstream:** [antirez/sds](https://github.com/antirez/sds) · **License:** BSD-2-Clause

---

## 1. Synopsis / problem statement

Re-implement the pedagogical surface of SDS (simple dynamic strings) in modern **C++** or **Rust**: length-aware, binary-safe strings with a clear create/mutate/free (or RAII) lifecycle. Upstream README examples and `sds.h` operations are the contract. Students must explicitly choose **spirit-compatible API** (default) versus ABI/layout compatibility (advanced, usually non-goal).

## 2. Goals & non-goals

**Goals**

- Implement core ops: new, empty, dup, free/drop, len, avail, clear, cat, catlen, cpy, cpylen, range, trim, and common helpers present in the header you inventory.
- Preserve **binary safety** (length ≠ C string terminator assumptions).
- Provide tests transcribed from README + null-byte cases.
- Document API mapping table (SDS → target).

**Non-goals**

- Redis drop-in / identical alloc headers / type-5 packing unless explicitly scoped as stretch.
- Locale-aware Unicode collation.
- Becoming a general ropes/persistent-string library.
- Silent performance claims without benchmarks.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `sds/header` | Length/alloc metadata representation (idiomatic, not necessarily wire-identical) |
| `sds/alloc` | Allocator port (std/global/custom) |
| `sds/ops` | Pure-ish operations on the string type |
| `sds/fmt` | Optional printf-like helpers if in inventory |
| `tests/golden` | README + binary fixtures |

**SOLID mapping (prose)**

- **S:** Formatting helpers do not own allocation policy; allocator is injected.
- **O:** New ops add functions/modules without rewriting core len/avail invariants.
- **L:** Any allocator satisfying the port can back the string type in tests (fail-alloc simulation).
- **I:** Callers use a small owned-string API; they do not poke header bytes (unless stretch ABI mode).
- **D:** Ops depend on allocator abstractions; not on Redis zmalloc.

## 4. EPICs

1. **Bootstrap & ops inventory** — map `sds.h` → target types; skeleton crate/lib.
2. **Create / length / destroy** — invariants for len/avail.
3. **Mutation ops** — cat/cpy/clear/range/trim.
4. **Helpers & formatting** — remaining inventoried helpers.
5. **Hardening** — fuzz null bytes; OOM paths; docs for ABI stance.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & ops inventory

- As a **student**, I want a complete ops table, so that I do not miss functions reviewers expect.
- As a **developer**, I want a compiling library skeleton, so that tests can land early.

### Epic 2 — Create / length / destroy

- As a **caller**, I want to create a string from a buffer with explicit length, so that embedded NULs work.
- As a **caller**, I want `len` and `avail` to reflect SDS semantics, so that tutorials still teach the same ideas.
- As a **caller**, I want deterministic destruction/RAII, so that leaks are hard.

### Epic 3 — Mutation ops

- As a **caller**, I want cat/cpy with both C-string and length-based variants, so that binary data is safe.
- As a **caller**, I want range/trim to adjust content in place (or return new per idiomatic choice), so that README recipes port.

### Epic 4 — Helpers & formatting

- As a **caller**, I want any printf-style helpers you committed to in the inventory, so that examples compile.
- As a **reviewer**, I want missing helpers listed as deferred, so that scope stays honest.

### Epic 5 — Hardening

- As a **developer**, I want fuzz or property tests with `\0` inside payloads, so that C-string mistakes fail CI.
- As a **student**, I want the README to state “not Redis ABI compatible” (unless it is), so that nobody misuses the port.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Ops inventory markdown | E1 | P0 |
| F2 `new` / `empty` / `dup` / drop | E2 | P0 |
| F3 `len` / `avail` / `clear` | E2 | P0 |
| F4 `cat` / `catlen` / `cpy` / `cpylen` | E3 | P0 |
| F5 `range` / `trim` | E3 | P0 |
| F6 README golden tests | E2–E4 | P0 |
| F7 Format helpers (if inventoried) | E4 | P1 |
| F8 Fail-alloc tests | E5 | P1 |
| F9 Layout-compatible headers | E5 | P2 |

## 7. Slices

1. **Slice A:** Library builds; `new` + `len` + drop tested.
2. **Slice B:** cat/cpy including binary payloads.
3. **Slice C:** range/trim + README examples green.
4. **Slice D:** Helper subset + mapping table published.
5. **Slice E:** Fuzz/smoke + explicit ABI disclaimer.

## 8. Waves

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Inventory + empty lib |
| **Wave 2** | Create/len | Slice A |
| **Wave 3** | Mutations | Slice B–C |
| **Wave 4** | Helpers | Documented P1 done or deferred |
| **Wave 5** | Polish / edge cases | Binary fuzz; license/ABI notes final |

## 9. Acceptance criteria / parity checklist outline

- [ ] Ops table: each symbol `done` / `deferred` / `N/A (ABI)`.
- [ ] README examples ported as tests (or clearly adapted with notes).
- [ ] Embedded NUL round-trips preserve `len`.
- [ ] No unexplained panics on empty-string edge ops you claim to support.
- [ ] BSD-2-Clause attribution present; ABI stance documented.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Students chase Redis layout | Default non-goal; PRD Gate in README |
| Mixing C-string APIs unsafely | Prefer length-based APIs in idiomatic wrappers |
| Incomplete inventory | Freeze `sds.h` version/commit in PORT_MAP |

**License:** BSD-2-Clause — retain copyright; include license text when redistributing.
