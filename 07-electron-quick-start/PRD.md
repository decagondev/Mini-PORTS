# PRD — Electron Quick Start → Tauri 2 Port

**Pack:** `07-electron-quick-start` · **Upstream:** [electron/electron-quick-start](https://github.com/electron/electron-quick-start) · **License:** CC0-1.0

---

## 1. Synopsis / problem statement

Students replace the minimal Electron template (main + preload + renderer) with a **Tauri 2** app driven by a Vite UI. The product is the shell itself: window lifecycle, a safe bridge for a tiny demo API (e.g. versions / ping), and a renderer that looks and behaves like the upstream template. Success is **architectural parity of roles**, not Electron API emulation.

## 2. Goals & non-goals

**Goals**

- Runnable Tauri 2 + Vite project that opens a window with the upstream demo content (title, versions, links) or a faithful equivalent.
- Explicit mapping document: Electron main/preload/IPC → Tauri commands/events/capabilities.
- Renderer starts with near-identical HTML; JS uses `invoke` (or events) instead of `window.electronAPI`.
- Documented capability/ACL choices (least privilege).

**Non-goals**

- Porting a full Electron app ecosystem or Node built-ins.
- Pixel-perfect Chromium chrome parity (window decorations differ by OS).
- Auto-updater, multi-window apps, native menus beyond a minimal stub (P2).
- Claiming drop-in compatibility with `electron` npm APIs.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `src-tauri/main` (or `lib`) | App bootstrap, window create, plugin wiring |
| `src-tauri/commands` | Allowlisted IPC handlers (versions, ping, …) |
| `src-tauri/capabilities` | ACL / permissions JSON for commands |
| `ui/` (Vite) | Renderer HTML/CSS/TS; no privileged FS |
| `bridge/` (TS) | Thin typed wrappers around `invoke` / listen |
| `docs/IPC_MAP.md` | Electron symbol → Tauri symbol table |

**SOLID mapping (prose)**

- **S:** Window bootstrap stays separate from command handlers; UI does not embed Rust policy.
- **O:** New demo commands add a handler + capability entry without rewriting the shell.
- **L:** Command handlers are swappable behind the same TS bridge types for mocks in tests.
- **I:** Renderer depends on a narrow bridge API (few functions), not a god `electronAPI` object with Node power.
- **D:** UI depends on abstract bridge ports; OS/FS access only via Rust commands injected by Tauri.

## 4. EPICs

1. **Bootstrap & shell** — `create-tauri-app` (or manual) + Vite; empty window loads UI.
2. **IPC mapping** — port demo bridge calls to Tauri commands; freeze `IPC_MAP.md`.
3. **Security hardening** — capabilities, CSP, no Node integration equivalent.
4. **Parity polish** — version strings, links, README runbooks.
5. **Packaging smoke** — one-platform build (optional stretch).

## 5. User stories (by epic)

### Epic 1 — Bootstrap & shell

- As a **student**, I want `npm run tauri dev` (or equiv.) to open a window, so that I can iterate from hour one.
- As a **reviewer**, I want a README stating Electron oracle commit + Tauri/Vite versions, so that I can reproduce.

### Epic 2 — IPC mapping

- As a **user**, I want to see app/runtime version info in the UI, so that the demo matches the template spirit.
- As a **developer**, I want every Electron IPC call listed with its Tauri replacement, so that nothing is ad-hoc.
- As a **developer**, I want a typed `bridge` module, so that the renderer never calls raw strings inconsistently.

### Epic 3 — Security hardening

- As a **reviewer**, I want capabilities to allowlist only demo commands, so that the port teaches least privilege.
- As a **developer**, I want the webview without a Node-like escape hatch, so that the security model is clearly Tauri-native.

### Epic 4 — Parity polish

- As a **user**, I want the same primary copy/links as the quick-start (or clearly noted substitutes), so that side-by-side demos feel familiar.
- As a **student**, I want a short “what differs” section (process model, packaging), so that learning is explicit.

### Epic 5 — Packaging smoke

- As a **student**, I want at least one `tauri build` attempt documented, so that packaging friction is acknowledged even if CI artifacts are not published.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Tauri + Vite scaffold | E1 | P0 |
| F2 Window loads renderer | E1 | P0 |
| F3 Version / ping commands | E2 | P0 |
| F4 Typed TS bridge | E2 | P0 |
| F5 `IPC_MAP.md` | E2 | P0 |
| F6 Capability allowlist | E3 | P0 |
| F7 Demo UI parity copy | E4 | P0 |
| F8 Differences write-up | E4 | P0 |
| F9 Native menu / tray | — | P2 |
| F10 Multi-platform release CI | E5 | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Empty Tauri window + “Hello” from Vite.
2. **Slice B:** One `invoke('ping')` round-trip displayed in UI.
3. **Slice C:** Version info panel matching template spirit; `IPC_MAP.md` complete for shipped APIs.
4. **Slice D:** Capabilities tightened; README security notes.
5. **Slice E:** Optional build smoke + known issues list.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Dev window opens with Vite UI |
| **Wave 2** | First command | Ping/version invoke works |
| **Wave 3** | Full demo bridge | Slice C; map doc merged |
| **Wave 4** | Capabilities / CSP | Least-privilege notes in README |
| **Wave 5** | Polish / edge cases | P0 checklist green; P2s listed |

## 9. Acceptance criteria / parity checklist outline

- [ ] Tauri 2 project runs in dev mode with Vite UI.
- [ ] Every shipped renderer→host call is a documented Tauri command/event (no hidden Node bridge).
- [ ] `IPC_MAP.md` covers all Electron IPC used in the oracle template.
- [ ] Capabilities allowlist matches shipped commands.
- [ ] README: how to run Electron oracle vs Tauri port; upstream citation; license note (CC0).
- [ ] No invented product features beyond the quick-start demo surface.
- [ ] Known OS/decoration differences listed (not treated as bugs).

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Scope creep into “real app” | Cap at template parity; forbid CRUD features |
| Copying Electron patterns that fight Tauri | Prefer idiomatic commands; document upgrades |
| Packaging rabbit hole | Mark build as P2; afternoon stops at `dev` if needed |
| Capability misconfig (too open) | Review allowlist against actual `invoke` list |

**License:** Upstream **CC0-1.0** — citation still required for academic honesty. New code should declare its own SPDX. Do not relicense third-party Electron trademarks as your own product branding.
