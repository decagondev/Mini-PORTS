# 16 — calculator Port Pack *(CalcManager SLICE)*

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/microsoft/calculator |
| **License** | MIT |
| **From → to** | UWP C++/C# Calculator → **WinUI 3** modernization **or** `CalcManager` engine → other UI (Flutter/RN/web)—**engine slice first** |
| **Effort band** | **Weekend** (~8–16 h) — **SLICE CalcManager parity first** |
| **~Stars** | ~31.1k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |
| **Slice** | **CalcManager** expression/standard (and agreed scientific subset) parity; full XAML/a11y/i18n UI deferred |

## Synopsis

Microsoft’s Windows Calculator is a large UWP C++/C# app with a separable calculation engine (`CalcManager`) and rich XAML UI. Full UI + accessibility + localization exceeds a weekend. This pack mandates **CalcManager parity first**: golden-test calculator expressions (standard, optional scientific subset); ignore graphing and chrome until the engine is green. Read upstream `docs/ApplicationArchitecture.md` (or equivalent architecture docs) before designing the port.

**Why it ports well:** Engine is separable and documented; MIT; expression fixtures make objective oracles. **Hard:** Scientific/graphing/UI a11y sprawl if you skip the slice.

## Learning outcomes

- Separate calculation engine from presentation; port or wrap `CalcManager` first.
- Build golden expression tests (input → display/history tokens) as acceptance.
- Resist UI/a11y/i18n rabbit holes until engine P0 is green.
- Practice MIT attribution on a large Microsoft OSS repo.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; read architecture docs; locate `CalcManager`.
2. Create a **new MIT-attributed** repo; cite microsoft/calculator.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; **engine before UI**.
4. Golden-test expressions for standard mode; add a **small** scientific subset only if Wave time remains.
5. Graphing, full WinUI chrome, and localization packs are out of weekend P0.

## Suggested verification

- Golden corpus: expression strings → expected results vs upstream engine / known values.
- Differential tests: same inputs against built oracle binary or extracted engine when practical.
- Optional thin UI that only binds to engine APIs after Slice D.
- README lists deferred UI/a11y/i18n/graphing explicitly.

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add yours on new files.
- Do not strip Microsoft copyright from ported engine files.
- Record oracle commit and which modes (standard / scientific subset) are in P0.

## Links

- Upstream: https://github.com/microsoft/calculator
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
