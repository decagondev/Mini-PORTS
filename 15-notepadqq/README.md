# 15 — notepadqq Port Pack *(SLICE)*

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/notepadqq/notepadqq |
| **License** | **GPL-3.0** (copyleft — see checklist) |
| **From → to** | Qt/C++ Linux editor → **WinUI 3** / **Flutter** desktop / **Tauri + Monaco** (pick one) |
| **Effort band** | **Weekend** (~8–16 h) — **SLICE only** |
| **~Stars** | ~2.3k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |
| **Slice** | **Document model first** (buffers, tabs, open/save); search/encoding later |

## Synopsis

Notepadqq is a Notepad++-inspired Qt editor for Linux. A full editor port (encodings matrix, search engine, syntax themes, printing) overruns a weekend. This pack mandates **document model first**: multi-doc buffers, tabs, dirty state, open/save—keep the Qt app running as oracle for those flows. Pick one UI stack; do not boil the ocean on syntax highlighting.

**Why it ports well:** Familiar editor UX; document model is separable from widgets. **Hard:** Encoding/search/multi-doc edge cases + GPL.

## Learning outcomes

- Separate **document model** (buffers/tabs) from widget toolkits.
- Port open/save/dirty/multi-tab before search and themes.
- Use the Qt binary/app as a live oracle for file flows.
- Complete a **GPL-3.0 license checklist** before redistribution.

## How to use this pack

1. **Fork / clone** upstream read-only; keep Qt notepadqq runnable as **oracle** for open/save/search later.
2. Create a **new repo**; choose WinUI / Flutter / Tauri+Monaco **once**; complete GPL posture in [PRD.md](./PRD.md) §10 before publishing.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 document-model stories.
4. Inventory: buffer + tab model **before** syntax themes or advanced search.
5. Encoding matrix and “Notepad++ parity” claims are out of scope for this pack.

## Suggested verification

- Manual oracle compare: open same fixture files → edit → save → bytes match policy (UTF-8 P0).
- Multi-tab: two docs dirty independently; close prompts on dirty ( agreedsimple policy).
- Automated tests for document model (pure) without UI where possible.
- License checklist signed-off before any public release.

## Attribution / license notes

- Upstream is **GPL-3.0**. Distributing a derivative requires GPL-3.0-compatible obligations.
- Prefer private learning forks if compliance is unclear; never strip GPL texts or relicense upstream as MIT.
- Record oracle commit and slice boundary (document model) in README.

## Links

- Upstream: https://github.com/notepadqq/notepadqq
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
