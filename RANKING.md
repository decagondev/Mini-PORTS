# Port Projects — Difficulty Ranking

Ordered **easiest → hardest** for the 16 shortlisted modernization exercises. Effort bands and star counts follow the upstream shortlist (verified ~2026-10-01 Europe/London); stars will drift.

**Method:** Prefer clear written/behavioral specs, tiny LOC cores, and few platform edge cases first. Scaffold-only shells rank easier than networking or rich parsers. Weekend UX apps are ranked by *slice* difficulty (not full rewrite).

| Rank | Slug / project | Effort | ~Stars | 1-line rationale |
|------|----------------|--------|--------|------------------|
| 1 | **todomvc** (`tastejs/todomvc`) | Afternoon | ~29k | Explicit `app-spec.md` + Cypress oracle; UI is small and identical across frameworks. |
| 2 | **2048** (`gabrielecirulli/2048`) | Afternoon | ~13.4k | Pure deterministic game core; tiny LOC; snapshot/replay tests are straightforward. |
| 3 | **jsmn** (`zserge/jsmn`) | Afternoon | ~4.2k | Single-header ~471 LOC tokenizer; token dumps diff cleanly with no DOM allocations. |
| 4 | **inih** (`benhoyt/inih`) | Afternoon | ~3k | ~500 LOC callback SAX; fixture-driven parity; optional flags are explicit feature toggles. |
| 5 | **sds** (`antirez/sds`) | Afternoon | ~5.6k | Famous small API + README tutorial; harder than jsmn/inih due to binary-safe lengths and alloc headers. |
| 6 | **linenoise** (`antirez/linenoise`) | Afternoon | ~4.4k | Bounded ~2.5k LOC surface, but terminal escapes / UTF-8 / PTY edge cases raise the bar. |
| 7 | **electron-quick-start** (`electron/electron-quick-start`) | Afternoon | ~11.4k | Almost no product logic—main/preload → Tauri commands—but IPC security model differs. *(Refined: before smallchat; scaffold beats sockets.)* |
| 8 | **smallchat** (`antirez/smallchat`) | Afternoon | ~7.5k | ~700 LOC teaching chat; `select` fan-out is small but concurrency + buffering choices matter. |
| 9 | **cJSON** (`davegamble/cJSON`) | Afternoon–Weekend | ~13k | ~3.5k LOC; rich escape/depth/number/malloc-failure edge cases push past a pure afternoon. |
| 10 | **microblog** (`miguelgrinberg/microblog`) | Weekend | ~4.8k | Familiar auth/posts/followers domain; tutorial chapters = ordered spec; sessions vs JWT tradeoffs. |
| 11 | **Pico** (`picocms/Pico`) | Weekend | ~3.9k | Flat-file Markdown golden corpus helps, but theme/plugin hook parity expands scope. |
| 12 | **flasky** (`miguelgrinberg/flasky`) | Weekend | ~8.7k | Same patterns as microblog but more blueprints/extensions—needs ruthless MVP scoping. |
| 13 | **AFNetworking** (`AFNetworking/AFNetworking`) | Weekend | ~33.4k | Concepts map cleanly (session/serializers), yet “done” is a curated Swift facade, not a UI app. |
| 14 | **Boostnote** *(slice)* (`BoostIO/Boostnote`) | Weekend | ~16.9k | Clear notes UX, but full matrix >> weekend; GPL-3.0; slice editor + local FS only. |
| 15 | **notepadqq** *(slice)* (`notepadqq/notepadqq`) | Weekend | ~2.3k | Familiar editor model, encoding/search/multi-doc state + GPL make a hard weekend slice. |
| 16 | **calculator** *(CalcManager slice)* (`microsoft/calculator`) | Weekend | ~31.1k | Engine is separable and well-documented, but expression/scientific parity + a11y/i18n outgrow a week if UI is included. |

## Refinement vs raw shortlist table order

- **electron-quick-start** placed above **smallchat**: scaffold mapping is cognitively lighter than socket fan-out.
- **cJSON** after the tiny C utilities and shells: edge-case surface is larger than linenoise/sds/jsmn/inih.
- Weekend ranks follow domain size + license/platform friction (GPL, ObjC→Swift facade, Qt/UWP).

## Suggested 5-day / ~4 h-day week

Use the six afternoon packs in this repo as the curriculum core. Keep upstream checkouts read-only as oracles.

| Day | Focus (~4 h) | Project pack | Outcome by EOD |
|-----|--------------|--------------|----------------|
| **Day 1** | Spec-driven UI port | [`01-todomvc`](./01-todomvc/) | Shell + add/complete/filter P0s green vs Cypress / app-spec |
| **Day 2** | Pure domain + UI shell | [`02-2048`](./02-2048/) | `move(grid, dir)` parity + playable SPA (animations optional) |
| **Day 3** | Tiny C → safe lang (tokenizer) | [`05-jsmn`](./05-jsmn/) | Token-dump differential tests on a golden JSON corpus |
| **Day 4** | Callback parser + string API | [`06-inih`](./06-inih/) AM · [`04-sds`](./04-sds/) PM | INI fixtures green; SDS ops table + null-byte fuzz smoke |
| **Day 5** | Terminal I/O (stretch) | [`03-linenoise`](./03-linenoise/) | History/completion demo + byte-level demo parity on happy path |

**If the cohort is stronger / has leftover hours:** insert **electron-quick-start** (Tauri shell) between Day 2 and Day 3, or swap Day 5 for **smallchat** scripted `nc` sessions.

**Weekend follow-on:** [`10-microblog`](./10-microblog/) → [`11-pico`](./11-pico/) or [`12-flasky`](./12-flasky/) → one of [`13-afnetworking`](./13-afnetworking/) / [`14-boostnote`](./14-boostnote/) (GPL slice) / [`16-calculator`](./16-calculator/) (CalcManager slice).

## Pack index (batch 1)

| Folder | Upstream | From → to (suggestion) |
|--------|----------|------------------------|
| [`01-todomvc`](./01-todomvc/) | [tastejs/todomvc](https://github.com/tastejs/todomvc) | jQuery/legacy → React / Vue / Svelte / Solid |
| [`02-2048`](./02-2048/) | [gabrielecirulli/2048](https://github.com/gabrielecirulli/2048) | Vanilla JS → React / Vue / Svelte or Flutter |
| [`03-linenoise`](./03-linenoise/) | [antirez/linenoise](https://github.com/antirez/linenoise) | C → C++17+ or Rust |
| [`04-sds`](./04-sds/) | [antirez/sds](https://github.com/antirez/sds) | C → C++ or Rust |
| [`05-jsmn`](./05-jsmn/) | [zserge/jsmn](https://github.com/zserge/jsmn) | C → Rust / Go / Zig |
| [`06-inih`](./06-inih/) | [benhoyt/inih](https://github.com/benhoyt/inih) | C → Rust / Go / modern C++ |

## Pack index (batch 2)

| Folder | Upstream | From → to (suggestion) |
|--------|----------|------------------------|
| [`07-electron-quick-start`](./07-electron-quick-start/) | [electron/electron-quick-start](https://github.com/electron/electron-quick-start) | Electron main/preload → Tauri 2 + Vite UI |
| [`08-smallchat`](./08-smallchat/) | [antirez/smallchat](https://github.com/antirez/smallchat) | C `select` chat → Go / Rust / modern C++ |
| [`09-cjson`](./09-cjson/) | [DaveGamble/cJSON](https://github.com/DaveGamble/cJSON) | ANSI C JSON DOM → Rust serde_json or C++ RAII |
| [`10-microblog`](./10-microblog/) | [miguelgrinberg/microblog](https://github.com/miguelgrinberg/microblog) | Flask → FastAPI + SQLAlchemy 2 / Pydantic |

## Pack index (batch 3 / complete)

| Folder | Upstream | From → to (suggestion) |
|--------|----------|------------------------|
| [`11-pico`](./11-pico/) | [picocms/Pico](https://github.com/picocms/Pico) | Legacy PHP flat-file CMS → Astro / Eleventy / Node or modern PHP |
| [`12-flasky`](./12-flasky/) | [miguelgrinberg/flasky](https://github.com/miguelgrinberg/flasky) | Flask book app → FastAPI or Django Ninja (ruthless MVP) |
| [`13-afnetworking`](./13-afnetworking/) | [AFNetworking/AFNetworking](https://github.com/AFNetworking/AFNetworking) | ObjC networking → Swift URLSession async facade (API subset) |
| [`14-boostnote`](./14-boostnote/) | [BoostIO/Boostnote](https://github.com/BoostIO/Boostnote) | Electron notes → Tauri + React/Svelte (**SLICE** editor + local FS; GPL-3.0) |
| [`15-notepadqq`](./15-notepadqq/) | [notepadqq/notepadqq](https://github.com/notepadqq/notepadqq) | Qt C++ editor → WinUI / Flutter / Tauri+Monaco (**SLICE** document model; GPL-3.0) |
| [`16-calculator`](./16-calculator/) | [microsoft/calculator](https://github.com/microsoft/calculator) | UWP C++/C# → WinUI 3 or engine port (**SLICE** CalcManager first; MIT) |

**All packs complete.** All 16 shortlisted port-project folders (`01`–`16`) now ship `README.md` + `PRD.md` planning packs. Source shortlist: [gist](https://gist.github.com/decagondev/6a1c5ba541c7692979bc9d24e27b0411).
