# PRD — linenoise Line-Editing Library Port

**Pack:** `03-linenoise` · **Upstream:** [antirez/linenoise](https://github.com/antirez/linenoise) · **License:** BSD-2-Clause

---

## 1. Synopsis / problem statement

Port linenoise’s line-editing behavior to idiomatic **C++17+** or **Rust**: raw terminal mode, key decoding, editable buffer, history, and completion callbacks. Upstream is the oracle for the public API in `linenoise.h` and for demo programs. The student problem is **safe ownership + modular TTY I/O**, not inventing a new REPL framework.

## 2. Goals & non-goals

**Goals**

- Cover the core public API: read a line, history add/load/save (as upstream exposes), completion callback registration.
- Idiomatic memory safety (RAII / Rust ownership) without leaking raw-mode on panic/exception paths where feasible.
- Runnable example binary comparable to upstream `example.c`.
- Happy-path key editing (arrows, backspace, basic Ctrl keys documented in inventory).

**Non-goals**

- Full GNU readline / libedit compatibility.
- Windows ConPTY support in Wave 1–4 (optional stretch only).
- Multiplexed async event loops / embedding in GUIs.
- Changing default keybindings for “ergonomics.”

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `term/raw` | Enter/leave raw mode; restore on drop/guard |
| `term/tty` | Read bytes / write escapes (port interface) |
| `edit/buffer` | Cursor, insert/delete, UTF-8 policy (document) |
| `edit/keys` | Decode input sequences → edit commands |
| `history` | Ring/list storage; optional file persistence |
| `completion` | Callback registration + display/apply |
| `api` | Thin façade mirroring linenoise.h semantics |

**SOLID mapping (prose)**

- **S:** History persistence is separate from key decoding; raw-mode guard is separate from buffer edits.
- **O:** New key sequences extend the decoder table without rewriting history.
- **L:** Fake TTY in tests substitutes for real termios while preserving API.
- **I:** Completion clients only see register-callback + hints surface—not term internals.
- **D:** Editing core depends on `Tty` / `Clock` ports; OS adapters live at the edge.

## 4. EPICs

1. **Bootstrap & API inventory** — build system, header/module map, empty `linenoise` façade.
2. **Raw mode & byte I/O** — safe enter/leave; echo/write primitives.
3. **Line editor core** — buffer + key decode → return line on Enter.
4. **History & completion** — callbacks and persistence parity for exposed APIs.
5. **Hardening** — UTF-8/width, signals, edge keys; optional Windows stretch.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & API inventory

- As a **student**, I want a markdown API table from `linenoise.h`, so that I know what “parity” means.
- As a **developer**, I want a compiling skeleton with an example main, so that CI can build from Wave 1.

### Epic 2 — Raw mode & byte I/O

- As a **user of the library**, I want the terminal restored if the program exits abnormally where practical, so that my shell is not left broken.
- As a **developer**, I want a test double for TTY reads/writes, so that I can unit-test without a real PTY initially.

### Epic 3 — Line editor core

- As a **user**, I want to type and backspace with a visible cursor, so that I can edit a command line.
- As a **user**, I want Left/Right (and documented Ctrl keys) to move the cursor, so that I can fix mid-line errors.
- As a **user**, I want Enter to submit the line to the caller, so that my REPL works.

### Epic 4 — History & completion

- As a **user**, I want Up/Down to walk history, so that I can recall prior lines.
- As a **application author**, I want to register a completion callback, so that Tab can offer completions.
- As a **application author**, I want history load/save APIs if upstream exposes them, so that sessions persist.

### Epic 5 — Hardening

- As a **developer**, I want documented UTF-8 behavior, so that reviewers know what was deferred.
- As a **user**, I want common escape edge cases not to corrupt the buffer, so that the editor feels stable.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 API inventory + skeleton | E1 | P0 |
| F2 RawMode guard | E2 | P0 |
| F3 Line buffer insert/delete | E3 | P0 |
| F4 Key decoder (minimal set) | E3 | P0 |
| F5 `linenoise()`-alike read | E3 | P0 |
| F6 History walk + add | E4 | P0 |
| F7 Completion callback | E4 | P0 |
| F8 History file load/save | E4 | P1 |
| F9 UTF-8 column width | E5 | P1 |
| F10 Windows PTY | E5 | P2 |

## 7. Slices

1. **Slice A:** Example binary links; prints “hello” without raw mode.
2. **Slice B:** Raw mode + read until Enter (no editing) returns bytes.
3. **Slice C:** Full basic editing (insert/backspace/arrows) interactive.
4. **Slice D:** History + completion wired in example.
5. **Slice E:** Scripted PTY tests vs upstream demo on a fixed key script.

## 8. Waves

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Inventory + skeleton builds |
| **Wave 2** | Raw I/O | Guard restores terminal |
| **Wave 3** | Editor core | Interactive line edit works |
| **Wave 4** | History/completion | Example shows both |
| **Wave 5** | Polish / edge cases | PTY script green; P2s listed |

## 9. Acceptance criteria / parity checklist outline

- [ ] Public API inventory checked off (implemented / deferred with reason).
- [ ] Example binary supports type → edit → submit loop.
- [ ] History navigation works for N entries.
- [ ] Completion callback invoked on Tab (per upstream UX).
- [ ] Terminal state restored after normal exit.
- [ ] At least one scripted input test (expect/PTY) documented.
- [ ] BSD-2-Clause notices present.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Terminal left in raw mode | RAII/`Drop` guard; panic hooks where applicable |
| Escape sequence maze | Freeze minimal key set in PARITY.md |
| UTF-8 width complexity | Defer with explicit non-goal until Wave 5 |
| Over-claiming readline parity | README disclaimer |

**License:** BSD-2-Clause — reproduce copyright and license text in distributions; keep file headers.
