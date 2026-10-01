# PRD — notepadqq Qt → Modern Desktop Slice (Document Model First)

**Pack:** `15-notepadqq` · **Upstream:** [notepadqq/notepadqq](https://github.com/notepadqq/notepadqq) · **License:** GPL-3.0

---

## 1. Synopsis / problem statement

Port a **weekend slice** of Notepadqq focusing on the **document model**: one-or-more text buffers, tab shell, dirty tracking, and open/save on a modern stack (WinUI 3, Flutter desktop, or Tauri+Monaco). The Qt app is the behavioral oracle for in-scope file flows. Search, encoding matrices, syntax themes, and IDE-like features are deferred—**document model green beats chrome**.

## 2. Goals & non-goals

**Goals**

- Runnable editor shell on **one** chosen stack.
- Document model: create, open, edit, save, save-as; multi-tab buffers with independent dirty flags.
- UTF-8 text files as P0 encoding; document encoding policy for unknowns (fail soft).
- Close/dirty prompt policy documented and implemented for P0.
- GPL compliance checklist completed for intended distribution posture.

**Non-goals (crystal clear)**

- Full Notepad++ / Notepadqq feature parity.
- Encoding matrix (UTF-16, legacy code pages) beyond UTF-8 P0 (P2).
- Find/replace engine, regex search, incremental search (P2—after document model).
- Syntax highlighting theme packs, plugins, printing, sessions cloud (Out / P2).
- Porting to multiple UI stacks in one submission.
- Relicensing GPL upstream as MIT; publishing without license texts.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/document` | Buffer text, path, dirty, encoding tag (utf-8) |
| `domain/workspace` | Tab list, active doc, close policies |
| `app/commands` | New/Open/Save/SaveAs/Close use-cases |
| `infra/fs` | Read/write text files (platform FS) |
| `ui/tabs` | Tab strip bound to workspace |
| `ui/editor` | Editor control (Monaco/TextBox/Flutter TextField, etc.) |

**SOLID mapping (prose)**

- **S:** Workspace does not render widgets; editor UI does not own disk I/O.
- **O:** Search can later subscribe to `Document` without rewriting FS.
- **L:** FS port swappable for tests (memory fake).
- **I:** UI binds to narrow workspace APIs—not a Qt god-object clone.
- **D:** Domain has no Qt/WinUI/Flutter imports.

## 4. EPICs

1. **License gate & shell** — posture + empty editor window.
2. **Single document** — new/edit/save one file UTF-8.
3. **Multi-tab workspace** — tabs, active doc, independent dirty.
4. **Close policies** — prompt on dirty; open multiple fixtures.
5. **Slice close-out** — docs, GPL checklist, defer search/encoding matrix.

## 5. User stories (by epic)

### Epic 1 — License gate & shell

- As a **student**, I want GPL posture documented, so that distribution risk is conscious.
- As a **student**, I want an empty editor window on my chosen stack, so that I can iterate.

### Epic 2 — Single document

- As a **user**, I want to create and type into a new document, so that the editor is usable.
- As a **user**, I want to save and reopen a UTF-8 file, so that disk persistence works.

### Epic 3 — Multi-tab workspace

- As a **user**, I want multiple tabs, so that I can edit more than one file.
- As a **user**, I want dirty flags per tab, so that unsaved work is visible.

### Epic 4 — Close policies

- As a **user**, I want a prompt when closing a dirty tab, so that I do not lose work.
- As a **student**, I want oracle comparison on open/save for fixtures, so that parity is objective.

### Epic 5 — Slice close-out

- As a **reviewer**, I want search/encoding/themes listed as deferred, so that the slice is honest.
- As a **student**, I want LICENSE/NOTICE complete, so that publish readiness is clear.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 GPL posture + NOTICE | E1 | P0 |
| F2 Editor shell | E1 | P0 |
| F3 New / edit buffer | E2 | P0 |
| F4 Open / save UTF-8 | E2 | P0 |
| F5 Multi-tab workspace | E3 | P0 |
| F6 Per-tab dirty | E3 | P0 |
| F7 Close dirty prompt | E4 | P0 |
| F8 Find/replace | — | P2 |
| F9 Encoding matrix | — | P2 |
| F10 Syntax themes | — | P2 / Out |

## 7. Slices (vertical, runnable)

1. **Slice A:** License checklist drafted; empty editor shell runs.
2. **Slice B:** Single-doc new/save/open UTF-8 round-trip.
3. **Slice C:** Multi-tab + per-tab dirty.
4. **Slice D:** Close prompts + multi-file oracle fixtures.
5. **Slice E:** Deferred list + GPL checklist complete; **stop before search**.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | License + shell | Slice A |
| **Wave 2** | Single document | Slice B |
| **Wave 3** | Tabs / dirty | Slice C |
| **Wave 4** | Close + fixtures | Slice D |
| **Wave 5** | Close-out only | Slice E — **no search/themes** |

**Wave scope rule:** Document model must be green before any find/replace or highlighter work. If late, cut chrome—not save integrity.

## 9. Acceptance criteria / parity checklist outline

- [ ] New / open / save / save-as work for UTF-8 fixtures.
- [ ] ≥2 tabs with independent dirty state.
- [ ] Closing dirty tab prompts (documented policy).
- [ ] Domain model testable without UI (at least workspace/document unit tests).
- [ ] README: stack choice, slice boundary, oracle commit.
- [ ] GPL checklist (§10) complete for posture.
- [ ] No claim of full Notepadqq/Notepad++ parity.
- [ ] Search/encoding/themes explicitly deferred.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Encoding rabbit hole | UTF-8 only P0; document failures |
| Search early | Hard Wave rule: after Slice E only |
| Toolkit bikeshed | Freeze WinUI vs Flutter vs Tauri in Wave 1 |
| GPL publish accident | Wave 1 license gate |

### GPL-3.0 compliance checklist (required)

- [ ] Upstream SPDX recorded: **GPL-3.0** (confirm LICENSE on oracle commit).
- [ ] Distribution posture: **private learning only** / **public GPL-3.0 derivative**.
- [ ] If distributing: GPL-3.0 text shipped; corresponding source plan documented.
- [ ] Copyright notices retained; `NOTICE` / `THIRD_PARTY.md` updated.
- [ ] No relicensing of upstream code to MIT/Apache.
- [ ] Combined work license compatibility reviewed for UI stack deps (e.g. some components’ licenses).
- [ ] README explains copyleft for collaborators.
- [ ] Reviewer/instructor sign-off before public release if required.

**License:** GPL-3.0 upstream — copyleft. Prefer private coursework forks when unsure; public distribution requires GPL-compatible compliance.
