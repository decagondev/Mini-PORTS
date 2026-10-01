# 07 — electron-quick-start Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/electron/electron-quick-start |
| **License** | CC0-1.0 |
| **From → to** | Minimal Electron shell (main + preload + renderer) → **Tauri 2** + Vite UI |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~11.4k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

electron-quick-start is the canonical minimal Electron repro/template: a **main** process, a **preload** bridge, and a thin HTML/JS **renderer**. There is almost no product logic—the exercise is mapping the Electron process/IPC security model onto **Tauri 2** (Rust commands + webview UI) while keeping the renderer surface familiar.

**Why it ports well:** Scaffold-only; clear 1:1 mapping (`ipcMain` / `contextBridge` → Tauri commands/events); packaging and capability ACL differences are the real learning, not feature inventory.

## Learning outcomes

- Map Electron main/preload/renderer roles onto Tauri’s Rust core + webview.
- Prefer capability-scoped commands over a wide Node-style bridge.
- Keep the first renderer HTML/CSS nearly identical, then harden IPC.
- Practice CC0 attribution and document intentional security-model upgrades.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle** (do not rewrite in-place).
2. Create a **new repo** for the Tauri 2 + Vite port; note CC0-1.0 on upstream assets you reuse.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Inventory Electron entrypoints (`main.js`, `preload.js`, `index.html`) into a mapping table before coding.
5. Map `ipcMain` / `contextBridge` → Tauri commands/events **1:1** for the demo surface; forbid inventing app features.

## Suggested verification

- Side-by-side: Electron oracle vs Tauri port open the same window title / version string / demo UI.
- Invoke every bridged API from the renderer; assert return values match the Electron demo.
- Confirm preload-less (or minimal) privilege: renderer cannot call arbitrary OS APIs outside allowlisted commands.
- Optional: `tauri build` dry-run packaging on one platform (document OS tested).

## Attribution / license notes

- Upstream is **CC0-1.0** (public domain dedication). Still cite electron/electron-quick-start in your README.
- Your *new* Rust/TS code may carry a clearer SPDX (e.g. MIT/Apache-2.0)—state it explicitly.
- Do not claim Electron API compatibility; this is a **shell port**, not an Electron polyfill.

## Links

- Upstream: https://github.com/electron/electron-quick-start
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
