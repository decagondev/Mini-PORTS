# 08 — smallchat Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/antirez/smallchat |
| **License** | **BSD-style file headers** (no root `LICENSE` file—see notes) |
| **From → to** | Tiny C `select` chat (server + client) → **Go** / **Rust** (`tokio`/`mio`) / modern **C++** |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~7.5k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

smallchat is antirez’s teaching chat (~700 LOC): a **server** and **client** built on `select`, nicknames, and simple fan-out. Intentional simplicity (including buffering assumptions) is part of the lesson. Porting means preserving the protocol/behavior enough for scripted `nc` / expect sessions to look the same—or deliberately documenting upgrades (better buffering, clearer framing).

**Why it ports well:** Tiny file set (`smallchat-server.c`, `smallchat-client.c`, `chatlib.*`); networking fan-out is learnable in an afternoon; differential `nc` scripts make an objective oracle.

## Learning outcomes

- Replace `select` loops with idiomatic concurrency (goroutines, async tasks, or asio) without changing the user-visible chat contract.
- Decide **preserve vs upgrade** for kernel buffering / partial reads—and write that decision down.
- Build record/replay chat transcripts as golden tests.
- Handle awkward licensing: copy file-header terms verbatim; do not invent a root SPDX.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**.
2. Create a **new repo**; copy copyright notices from C file headers into `NOTICE` / per-file headers—**do not invent** a root LICENSE that upstream never published.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Capture oracle transcripts (`nc` join → nick → message fan-out) before changing buffering.
5. Pick **one** target stack (Go *or* Rust *or* modern C++); forbid polyglot submissions.

## Suggested verification

- Scripted sessions: connect N clients; set nicknames; broadcast; assert identical visible lines vs oracle (modulo intentional upgrades noted in README).
- Disconnect / partial-line cases: document behavior match or deliberate fix.
- Optional: fuzz truncated writes; ensure server does not crash on client drop.

## Attribution / license notes

- Upstream carries **BSD-style copyright in file headers** and **no root LICENSE**. Treat headers as the license text; keep them on any copied/ported files derived from upstream.
- Do **not** claim “MIT” or assign a new SPDX to upstream code. Your *purely new* files may use a clear license if compatible—still keep NOTICE for antirez copyrighted material.
- Cite https://github.com/antirez/smallchat and record the oracle commit hash.

## Links

- Upstream: https://github.com/antirez/smallchat
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
