# PRD — Boostnote Electron → Tauri Slice (Editor + Local FS)

**Pack:** `14-boostnote` · **Upstream:** [BoostIO/Boostnote](https://github.com/BoostIO/Boostnote) · **License:** GPL-3.0

---

## 1. Synopsis / problem statement

Port a **weekend slice** of Boostnote to **Tauri + React/Svelte**: a notes editor that lists, opens, edits, and saves notes as files on the **local filesystem**. The Electron app remains the UX oracle for editor behaviors in-scope. Full Boostnote feature matrix (sync, storages, snippets modes, etc.) is **out of scope**—success is a shippable FS-backed editor strip plus a completed GPL license checklist.

## 2. Goals & non-goals

**Goals**

- Tauri 2 app shell with React **or** Svelte UI.
- Local directory as notes root: list files, open, edit, save (Markdown/plain text P0).
- Editor UX subset: title/body edit, dirty-state indicator, save (⌘/Ctrl+S), basic Markdown preview **optional** P1.
- FS operations only via Tauri commands (no Node in the webview).
- Written slice boundary + GPL compliance checklist completed for the intended distribution posture.

**Non-goals (crystal clear)**

- Cloud sync, accounts, sharing, or multi-device.
- Full Boostnote storage engines / snipet/folder taxonomy / every sidebar mode.
- Pixel-perfect clone of legacy Electron chrome.
- Porting “Boostnote Next” / other successors as a substitute without instructor approval.
- Relicensing GPL code as MIT/Apache; publishing without license texts.
- Full feature parity with upstream—**explicitly refused** for this pack.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `ui/editor` | Text/Markdown editor component; dirty state |
| `ui/sidebar` | Minimal file list for notes root |
| `app/notes` | Use-cases: list/open/save (no FS APIs) |
| `infra/tauri_fs` | Tauri commands: `list_notes`, `read_note`, `write_note` |
| `infra/paths` | Notes-root path resolution; sandbox policy |
| `domain/note` | Note id/path, title, body, dirty flag |

**SOLID mapping (prose)**

- **S:** Editor UI does not call `std::fs`; commands do not render Markdown.
- **O:** Preview can be added as a view module without rewriting FS commands.
- **L:** FS port can swap temp-dir test double ↔ real Tauri FS.
- **I:** UI depends on narrow note hooks—not a god “BoostnoteService.”
- **D:** Domain/use-cases depend on note repository port; Tauri stays in `infra`.

## 4. EPICs

1. **License gate & shell** — compliance posture documented; Tauri+UI hello world.
2. **FS notes root** — pick folder / configured root; list notes.
3. **Open & edit** — load file into editor; dirty tracking.
4. **Save & reload** — write through; reopen integrity.
5. **Slice parity & GPL close-out** — checklist, README, deferred matrix published.

## 5. User stories (by epic)

### Epic 1 — License gate & shell

- As a **student**, I want a documented distribution posture (private vs public GPL), so that I do not accidentally violate copyleft.
- As a **student**, I want a runnable Tauri shell, so that I can iterate on UI immediately.

### Epic 2 — FS notes root

- As a **user**, I want to point the app at a local notes folder, so that files are the source of truth.
- As a **user**, I want to see a list of note files, so that I can pick one to edit.

### Epic 3 — Open & edit

- As a **user**, I want to open a note into the editor, so that I can read and change text.
- As a **user**, I want a dirty indicator, so that I know unsaved changes exist.

### Epic 4 — Save & reload

- As a **user**, I want to save (shortcut + button), so that disk matches the editor.
- As a **user**, I want reopening a note to show saved content, so that persistence is trustworthy.

### Epic 5 — Slice parity & GPL close-out

- As a **reviewer**, I want an explicit “not in slice” feature list, so that scope is honest.
- As a **student**, I want `LICENSE`/`NOTICE` complete per checklist, so that publish readiness is clear.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 GPL posture + NOTICE scaffold | E1 | P0 |
| F2 Tauri + UI shell | E1 | P0 |
| F3 Notes-root list | E2 | P0 |
| F4 Open note into editor | E3 | P0 |
| F5 Dirty state + edit | E3 | P0 |
| F6 Save + reload integrity | E4 | P0 |
| F7 Keyboard save shortcut | E4 | P0 |
| F8 Markdown preview | — | P1 |
| F9 Cloud sync / accounts | — | **Out** |
| F10 Full storage/sidebar matrix | — | **Out** |

## 7. Slices (vertical, runnable)

1. **Slice A:** License checklist drafted; Tauri window shows “Boostnote slice” shell.
2. **Slice B:** List files from a temp/notes root via Tauri command.
3. **Slice C:** Open + edit with dirty flag (in-memory until save).
4. **Slice D:** Save/reload round-trip green (tests + manual).
5. **Slice E:** Deferred-matrix + GPL checklist complete; stop (no sync).

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | License + shell | Slice A (**gate:** posture chosen) |
| **Wave 2** | List FS notes | Slice B |
| **Wave 3** | Editor open/edit | Slice C |
| **Wave 4** | Save integrity | Slice D |
| **Wave 5** | Close-out only | Slice E — **do not start sync/themes matrix** |

**Wave scope rule:** If behind schedule after Wave 3, drop Markdown preview and polish—**never** open sync or multi-storage. Waves 1–4 are the only product waves; Wave 5 is documentation/license.

## 9. Acceptance criteria / parity checklist outline

- [ ] Notes root configurable; list shows files.
- [ ] Open → edit → save → reopen preserves text.
- [ ] Dirty state clears on successful save.
- [ ] All FS access via Tauri commands (no Node FS in webview).
- [ ] README states slice boundary and non-goals.
- [ ] GPL checklist (§10) completed for intended posture.
- [ ] Oracle commit recorded; no claim of full Boostnote parity.
- [ ] Sync/cloud explicitly marked out of scope.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Full-app temptation | Hard non-goals; Wave 5 = close-out only |
| GPL accidental MIT publish | License gate in Wave 1; block public release until checklist done |
| FS permission / path traversal | Sandbox notes root; normalize paths in Rust |
| Editor library bikeshed | Pick one editor (textarea/CodeMirror/Monaco subset) in Wave 1 |

### GPL-3.0 compliance checklist (required)

- [ ] Upstream SPDX recorded: **GPL-3.0** (confirm LICENSE file on oracle commit).
- [ ] Distribution posture chosen: **private learning only** / **public GPL-3.0 derivative**.
- [ ] If distributing: full GPL-3.0 license text shipped; source offer / corresponding source plan documented.
- [ ] Copyright notices retained; `NOTICE` / `THIRD_PARTY.md` lists Boostnote + other deps.
- [ ] No attempt to relicense upstream code as MIT/Apache.
- [ ] New-only files: license header compatible with GPL when combined into a GPL distribution.
- [ ] README states copyleft obligations in plain language for collaborators.
- [ ] Instructor/reviewer sign-off before **any** public release (if class policy requires).

**License:** GPL-3.0 upstream — copyleft. Private coursework use may be fine under classroom policy; **public distribution** requires GPL-compatible compliance. When unsure, keep the fork private and still practice the checklist.
