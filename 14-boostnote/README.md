# 14 — Boostnote Port Pack *(SLICE)*

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/BoostIO/Boostnote |
| **License** | **GPL-3.0** (copyleft — see checklist) |
| **From → to** | Electron + JS notes app → **Tauri** + **React** or **Svelte** (pick one) |
| **Effort band** | **Weekend** (~8–16 h) — **SLICE only** |
| **~Stars** | ~16.9k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |
| **Slice** | **Editor + local filesystem notes only** |

## Synopsis

Boostnote is a popular Electron programmer’s notes app. A full feature-matrix port exceeds a weekend. This pack mandates a **vertical slice**: open/edit/save Markdown (or plain) notes on the **local filesystem** via Tauri commands—no cloud sync, no full storage-engine archaeology, no every sidebar mode. GPL-3.0 means you need a **license compliance checklist before any publish/distribute** step.

**Why it ports well:** Clear notes UX metaphor; Electron→Tauri is a standard modernization story. **Hard:** Scope + GPL obligations.

## Learning outcomes

- Map Electron `ipcMain` / Node FS → Tauri commands + Rust `std::fs` / fs plugin.
- Ruthlessly slice a large UX app to editor + local FS.
- Run a **GPL-3.0 license gate** before redistribution.
- Keep upstream checkout as UX/oracle; do not big-bang rewrite.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; inventory screens → must-have vs later (**slice = editor + local FS**).
2. Create a **new repo**; decide distribution posture (private learning fork vs public GPL-compatible release)—complete the license checklist in [PRD.md](./PRD.md) §10 **before** publishing.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 slice stories.
4. Abstract FS behind a platform API (Tauri commands); UI never touches raw paths unsafely.
5. Forbid cloud sync, multi-storage backends, and “Boostnote Next” feature chasing in this pack.

## Suggested verification

- Manual: create note file on disk → edit in UI → save → reopen matches bytes/text.
- Scripted: Tauri command integration tests for list/read/write in a temp directory.
- UX parity checklist for the **slice only** (sidebar minimal file list OK).
- License checklist signed-off in README/`NOTICE` before any public release.

## Attribution / license notes

- Upstream is **GPL-3.0**. If you distribute a derivative that includes GPL-covered code, your distribution must meet GPL-3.0 obligations (source offer, license texts, compatible licensing of combined work).
- Prefer **private learning forks** if you are unsure about compliance; do not relicense upstream code as MIT.
- Keep copyright headers; add `NOTICE` / `THIRD_PARTY.md`; never strip GPL texts.
- Record oracle commit and the explicit slice boundary in README.

## Links

- Upstream: https://github.com/BoostIO/Boostnote
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
