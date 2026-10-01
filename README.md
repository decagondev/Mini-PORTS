# Port Projects — Student Pack Bundle

Planning packs for **fork-and-port** modernization exercises: take a real public upstream repo as the **spec + golden oracle**, then produce a clean port to a suggested modern stack.

This bundle does **not** ship ported source code. Each project folder contains a **README** (how to run the exercise) and a **PRD** (epics → user stories → features → slices → waves, with SOLID / modular architecture notes).

**Difficulty order:** see [RANKING.md](./RANKING.md).

**Upstream shortlist source:** [gist (decagondev)](https://gist.github.com/decagondev/6a1c5ba541c7692979bc9d24e27b0411) — afternoon / weekend port candidates verified ~2026-10-01 (Europe/London).

---

## What’s in each pack

| File | Purpose |
|------|---------|
| `README.md` | Upstream URL, license, from→to stack, synopsis, effort band, learning outcomes, fork-and-port usage, verification hints, attribution |
| `PRD.md` | Synopsis, goals/non-goals, modular architecture + SOLID mapping, EPICs, user stories, features, slices, waves, acceptance/parity, risks & license |

**Conventions**

- Prefer **new repo + attribution** over drive-by rewrite PRs unless maintainers invite them.
- Keep the upstream checkout **read-only** as the oracle (differential tests, fixtures, Cypress, `nc` sessions, etc.).
- **No invented product features** — parity with the old app / written spec only.
- **Weekend UX apps** (Boostnote, Notepadqq, Calculator) are scoped as **slices**; full feature matrices are explicit non-goals.
- **GPL** projects (Boostnote, Notepadqq): read PRD §10 license checklist before any redistribution.

---

## Pack index (easiest → hardest)

| # | Folder | Upstream | From → to (suggestion) | Effort |
|---|--------|----------|------------------------|--------|
| 1 | [`01-todomvc`](./01-todomvc/) | [tastejs/todomvc](https://github.com/tastejs/todomvc) | jQuery / legacy → React / Vue / Svelte / Solid | Afternoon |
| 2 | [`02-2048`](./02-2048/) | [gabrielecirulli/2048](https://github.com/gabrielecirulli/2048) | Vanilla JS → React / Vue / Svelte or Flutter | Afternoon |
| 3 | [`05-jsmn`](./05-jsmn/) | [zserge/jsmn](https://github.com/zserge/jsmn) | C → Rust / Go / Zig | Afternoon |
| 4 | [`06-inih`](./06-inih/) | [benhoyt/inih](https://github.com/benhoyt/inih) | C → Rust / Go / modern C++ | Afternoon |
| 5 | [`04-sds`](./04-sds/) | [antirez/sds](https://github.com/antirez/sds) | C → C++ or Rust | Afternoon |
| 6 | [`03-linenoise`](./03-linenoise/) | [antirez/linenoise](https://github.com/antirez/linenoise) | C → C++17+ or Rust | Afternoon |
| 7 | [`07-electron-quick-start`](./07-electron-quick-start/) | [electron/electron-quick-start](https://github.com/electron/electron-quick-start) | Electron → Tauri 2 + Vite | Afternoon |
| 8 | [`08-smallchat`](./08-smallchat/) | [antirez/smallchat](https://github.com/antirez/smallchat) | C `select` chat → Go / Rust / modern C++ | Afternoon |
| 9 | [`09-cjson`](./09-cjson/) | [davegamble/cJSON](https://github.com/davegamble/cJSON) | ANSI C → Rust `serde_json` or C++ RAII | Afternoon–Weekend |
| 10 | [`10-microblog`](./10-microblog/) | [miguelgrinberg/microblog](https://github.com/miguelgrinberg/microblog) | Flask → FastAPI + SQLAlchemy 2 / Pydantic | Weekend |
| 11 | [`11-pico`](./11-pico/) | [picocms/Pico](https://github.com/picocms/Pico) | Legacy PHP CMS → Astro / Eleventy / modern PHP | Weekend |
| 12 | [`12-flasky`](./12-flasky/) | [miguelgrinberg/flasky](https://github.com/miguelgrinberg/flasky) | Flask → FastAPI or Django Ninja (MVP) | Weekend |
| 13 | [`13-afnetworking`](./13-afnetworking/) | [AFNetworking/AFNetworking](https://github.com/AFNetworking/AFNetworking) | ObjC → Swift `URLSession` async facade (subset) | Weekend |
| 14 | [`14-boostnote`](./14-boostnote/) | [BoostIO/Boostnote](https://github.com/BoostIO/Boostnote) | Electron → Tauri + React/Svelte (**editor + local FS slice**) | Weekend · GPL-3.0 |
| 15 | [`15-notepadqq`](./15-notepadqq/) | [notepadqq/notepadqq](https://github.com/notepadqq/notepadqq) | Qt/C++ → WinUI / Flutter / Tauri+Monaco (**document-model slice**) | Weekend · GPL-3.0 |
| 16 | [`16-calculator`](./16-calculator/) | [microsoft/calculator](https://github.com/microsoft/calculator) | UWP → WinUI 3 or engine port (**CalcManager slice**) | Weekend |

Folder numbers (`01`…`16`) follow the original shortlist table order; **learning order** follows [RANKING.md](./RANKING.md) (jsmn/inih before linenoise, electron-quick-start before smallchat, etc.).

---

## Suggested use

1. Read [RANKING.md](./RANKING.md) and pick an Afternoon pack first.
2. Open that folder’s `README.md` + `PRD.md`; implement **Wave 1 → last wave**, P0 stories only.
3. Keep upstream as oracle; close acceptance via differential tests / fixtures named in the PRD.
4. For GPL or large UX apps, stay inside the documented **slice** and license checklist.

## License of *this* documentation

These planning docs are original teaching materials. They do **not** relicense upstream code. Always obey each upstream SPDX / file-header terms when students publish a port.
