# PRD — inih INI Parser Port

**Pack:** `06-inih` · **Upstream:** [benhoyt/inih](https://github.com/benhoyt/inih) · **License:** BSD-3-Clause

---

## 1. Synopsis / problem statement

Port inih’s **callback-style (SAX) INI parser** to **Rust**, **Go**, or modern **C++**, preserving the handler contract (section, name, value) and fixture-driven behavior. Optionally layer an `INIReader`-style convenience map on top as a separate module. Compile-time feature flags in upstream (`#define`s) become **documented feature toggles**—students must state which are honored.

## 2. Goals & non-goals

**Goals**

- Callback/handler parity on upstream-style fixtures (`examples/test.ini` and extensions).
- Ordered callback traces as the primary oracle.
- Error reporting with line context where upstream provides it.
- Optional typed reader module that consumes the parser (not a rewrite of it).

**Non-goals**

- Full TOML/YAML compatibility.
- Silent “helpful” coercion of types beyond what the reader API documents.
- Guaranteeing every historical `#define` combination on day one—pick a matrix and publish it.
- GUI / schema editors.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `ini/lexer` or scanner | Line/comments/BOM handling per flags |
| `ini/parser` | Invoke handler for section/key/value |
| `ini/handler` | User callback port / trait / interface |
| `ini/reader` | Optional map-like façade (`Get`, `GetInteger`, …) |
| `ini/flags` | Feature toggle struct (multiline, inline comments, …) |
| `tests/trace` | Ordered callback capture |

**SOLID mapping (prose)**

- **S:** Reader convenience getters do not embed parsing; they sit on parse results or re-parse via parser.
- **O:** New flags extend `flags` + tests without rewriting handler signatures.
- **L:** Handlers are replaceable; a tracing handler is used in tests interchangeably with app handlers.
- **I:** Minimal handler: `(section, name, value) → continue/abort`—no forced map dependency.
- **D:** Parser depends on abstract handler + I/O reader ports; file/OS details at the edges.

## 4. EPICs

1. **Bootstrap & flag matrix** — document honored `#define` equivalents.
2. **Core callback parser** — sections, keys, values, comments.
3. **Error paths** — malformed lines, handler abort.
4. **Optional INIReader** — typed getters.
5. **Fixture CI** — matrix tests + docs.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & flag matrix

- As a **student**, I want a FLAGS table (on/off/deferred), so that reviewers know the contract.
- As a **developer**, I want a skeleton library + `parse_string`, so that fixtures run without files first.

### Epic 2 — Core callback parser

- As an **application author**, I want a callback per key, so that I can stream config without a full DOM.
- As an **application author**, I want section changes visible to the handler, so that I can build nested maps.
- As a **user of fixtures**, I want comment lines ignored per flags, so that real INI files parse.

### Epic 3 — Error paths

- As a **caller**, I want parse errors to include line numbers when upstream does, so that I can fix config files.
- As a **caller**, I want handler abort to stop parsing, so that I can enforce policies.

### Epic 4 — Optional INIReader

- As an **application author**, I want `Get` / `GetInteger` / `GetBoolean`-style APIs, so that simple apps stay ergonomic.
- As a **developer**, I want the reader to reuse the parser module, so that behavior cannot diverge.

### Epic 5 — Fixture CI

- As a **reviewer**, I want trace diffs for `test.ini`, so that grading is mechanical.
- As a **student**, I want a matrix job for each honored flag, so that feature toggles stay honest.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 FLAGS.md / README matrix | E1 | P0 |
| F2 `parse_string` + handler | E2 | P0 |
| F3 Sections + keys + values | E2 | P0 |
| F4 Comments / BOM per flags | E2 | P0 |
| F5 Multiline values (if flagged) | E2 | P1 |
| F6 Inline comments (if flagged) | E2 | P1 |
| F7 Errors + abort | E3 | P0 |
| F8 INIReader façade | E4 | P1 |
| F9 Trace fixture CI | E5 | P0 |

## 7. Slices

1. **Slice A:** Parse a minimal `[section]\nkey=value` with trace assert.
2. **Slice B:** Full `examples/test.ini` trace matches oracle (for default flags).
3. **Slice C:** Error + handler-abort cases.
4. **Slice D:** Reader getters on top of parser.
5. **Slice E:** Flag matrix tests for each honored option.

## 8. Waves

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | FLAGS published; skeleton parses nothing successfully |
| **Wave 2** | Core parser | Slice A–B |
| **Wave 3** | Errors | Slice C |
| **Wave 4** | Reader (optional) | Slice D or explicit skip |
| **Wave 5** | Polish / edge cases | Matrix CI; BSD notices |

## 9. Acceptance criteria / parity checklist outline

- [ ] Callback trace equality on primary fixture(s).
- [ ] FLAGS table complete (`honored` / `deferred`).
- [ ] Handler abort stops further callbacks.
- [ ] Error line reported for at least one malformed fixture (if upstream does).
- [ ] Reader (if shipped) uses parser—no duplicate grammar.
- [ ] BSD-3-Clause license text + copyright retained.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Flag combinatorial explosion | Cap honored set; defer rest explicitly |
| Reader diverging from SAX | Single parse implementation |
| Comment/edge INI dialects | Fixtures over anecdotes |

**License:** BSD-3-Clause — include license; respect non-endorsement; keep benhoyt/inih attribution.
