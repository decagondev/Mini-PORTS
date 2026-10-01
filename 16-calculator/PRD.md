# PRD — Windows Calculator CalcManager Slice Port

**Pack:** `16-calculator` · **Upstream:** [microsoft/calculator](https://github.com/microsoft/calculator) · **License:** MIT

---

## 1. Synopsis / problem statement

Port or re-host the **CalcManager** calculation engine (standard mode + optional small scientific subset) with golden expression tests, optionally binding a **thin** WinUI 3 or cross-platform UI afterward. Students treat upstream architecture docs and engine behavior as the contract. Full Calculator UI, graphing, accessibility, and localization are **non-goals for the weekend**—engine parity first.

## 2. Goals & non-goals

**Goals**

- Runnable CalcManager-equivalent API: submit expressions/key sequences → results.
- Golden tests for **standard** mode operations (add/sub/mul/div, decimals, chaining, clear/CE, ±, percent as present in oracle P0 set).
- Document whether you **wrap/port C++ engine** vs **reimplement** in another language—and keep differential tests either way.
- Optional: minimal UI that calls the engine (not a full Windows Calculator clone).
- Explicit deferred list: graphing, programmer mode, full scientific surface, a11y, i18n.

**Non-goals (crystal clear)**

- Full WinUI/UWP visual parity, animations, and acrylic chrome.
- Graphing calculator mode.
- Complete scientific / programmer / date-calculator surfaces (beyond a tiny agreed subset).
- Full accessibility + localization packs for all languages.
- Store packaging / installer polish as P0.
- Feature invention (CAS, units, crypto) beyond Calculator’s engine.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `engine/calc` | Expression/key evaluation (ported or wrapped CalcManager) |
| `engine/modes` | Standard (+ optional scientific subset) feature flags |
| `app/api` | Stable façade: `input(token)`, `display()`, `history()` |
| `infra/oracle` | Test harness comparing to upstream fixtures |
| `ui/shell` | **Optional** thin UI binding to `app/api` only |

**SOLID mapping (prose)**

- **S:** UI does not implement arithmetic; engine does not render XAML.
- **O:** New modes extend behind feature flags without rewriting standard tests.
- **L:** Engine port vs FFI wrapper both satisfy the same façade for tests.
- **I:** UI depends on narrow display/input APIs—not the entire UWP app model.
- **D:** Tests depend on the façade; upstream C++ details stay behind an adapter if wrapped.

## 4. EPICs

1. **Architecture & harness** — read docs; fixture format; empty façade.
2. **Standard ops** — basic arithmetic + clear behaviors.
3. **Decimals & chaining** — edge cases from golden corpus.
4. **Optional scientific subset or thin UI** — only if standard green.
5. **Close-out** — deferred matrix, README, weekend retrospective.

## 5. User stories (by epic)

### Epic 1 — Architecture & harness

- As a **student**, I want an architecture note citing upstream docs, so that engine boundaries are clear.
- As a **student**, I want a test harness that runs expression fixtures, so that parity is objective from day one.

### Epic 2 — Standard ops

- As a **user** (via API), I want + − × ÷ to match oracle results, so that the core calculator works.
- As a **user**, I want C / CE / backspace behaviors per P0 fixtures, so that editing inputs matches spirit.

### Epic 3 — Decimals & chaining

- As a **user**, I want decimal and chained operations to match golden expected values, so that edge cases are covered.
- As a **developer**, I want failing fixtures listed, so that known gaps are honest.

### Epic 4 — Optional scientific subset or thin UI

- As a **student**, I want either a **small** scientific subset **or** a thin UI—not both if time is tight—so that scope stays controlled.
- As a **reviewer**, I want UI optional and engine-mandatory, so that grading favors CalcManager parity.

### Epic 5 — Close-out

- As a **reviewer**, I want graphing/a11y/i18n listed deferred, so that the slice is credible.
- As a **student**, I want MIT attribution + oracle commit recorded, so that provenance is clear.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Architecture note + harness | E1 | P0 |
| F2 Standard arithmetic façade | E2 | P0 |
| F3 Clear / CE / editing keys (P0 set) | E2 | P0 |
| F4 Decimal + chaining fixtures | E3 | P0 |
| F5 Golden corpus CI/script | E3 | P0 |
| F6 Scientific subset (sin/cos/tan/log small set) | E4 | P1 |
| F7 Thin UI shell | E4 | P1 |
| F8 Graphing mode | — | **Out** |
| F9 Full a11y / i18n packs | — | **Out** / P2 |
| F10 Programmer / date modes | — | **Out** / P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Harness runs; one hard-coded `1+1=2` fixture green against façade stub/engine.
2. **Slice B:** Standard arithmetic P0 corpus green.
3. **Slice C:** Decimals/chaining/clear fixtures green; gaps listed.
4. **Slice D (optional):** Scientific subset **or** thin UI bound to façade.
5. **Slice E:** Deferred matrix published; no graphing.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Docs + harness | Slice A |
| **Wave 2** | Standard ops | Slice B |
| **Wave 3** | Edge fixtures | Slice C (**gate:** engine P0 green) |
| **Wave 4** | Optional UI or sci subset | Slice D or explicit skip |
| **Wave 5** | Close-out | Slice E — **no graphing/a11y pack** |

**Wave scope rule:** Do **not** start WinUI chrome, graphing, or localization until Slice C is green. If time is short, **skip Wave 4** entirely—engine fixtures beat UI.

## 9. Acceptance criteria / parity checklist outline

- [ ] Architecture note references upstream engine/UI separation.
- [ ] Standard-mode golden corpus passes for agreed P0 operations.
- [ ] Façade API documented (`input` / `display` or equivalent).
- [ ] Wrap vs reimplement decision documented.
- [ ] Deferred: graphing, full scientific/programmer, a11y, i18n.
- [ ] README: how to run tests; oracle commit; mode scope.
- [ ] MIT attribution; Microsoft notices preserved on ported files.
- [ ] Optional UI (if present) cannot bypass engine façade.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| UI/a11y time sink | Wave rule: engine gate before UI |
| Scientific surface sprawl | Cap P1 subset; else skip |
| FFI vs rewrite bikeshed | Decide in Wave 1; keep façade stable |
| Fixture drift | Pin oracle commit; vendor expected JSON |

**License:** MIT upstream — retain notices; new code may remain MIT. Do not remove Microsoft copyright headers from ported engine sources.
