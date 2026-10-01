# PRD — smallchat Teaching Chat Port

**Pack:** `08-smallchat` · **Upstream:** [antirez/smallchat](https://github.com/antirez/smallchat) · **License:** BSD-style file headers (no root LICENSE)

---

## 1. Synopsis / problem statement

Port antirez’s tiny C chat (server + client, `select`-based) to **Go**, **Rust**, or modern **C++**, preserving nickname + fan-out behavior so scripted multi-client sessions match the oracle. Students must confront intentional simplicity (buffering, line handling) and either match it or document a deliberate upgrade. Licensing requires careful NOTICE practice because there is no root LICENSE file.

## 2. Goals & non-goals

**Goals**

- Working server and client (or server + `nc` client if client is deferred to P1—default is **both** for P0).
- Multi-client broadcast with nicknames matching oracle transcripts on the happy path.
- Clear written policy: preserve smallchat buffering quirks **or** upgrade with tests proving safety.
- Differential session scripts (`expect` / Python / shell) as acceptance gate.

**Non-goals**

- TLS, accounts, rooms, history persistence, or web UI.
- Redis-level protocol compatibility or RESP.
- Perfect Unicode/terminal editing (not linenoise).
- Inventing a root LICENSE SPDX for upstream-derived code.

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `net/listen` | Accept loop / listener bind |
| `net/conn` | Per-connection read/write, line framing |
| `chat/session` | Nickname state, join/leave bookkeeping |
| `chat/fanout` | Broadcast to peers (exclude sender if oracle does) |
| `client/ui` | Minimal stdin ↔ socket client |
| `tests/sessions` | Recorded multi-client transcripts |

**SOLID mapping (prose)**

- **S:** Fan-out logic does not own TCP accept; framing does not own nickname policy.
- **O:** Adding a `/me` command later should extend a command dispatcher without rewriting the acceptor.
- **L:** Connection wrappers should be mockable so fan-out unit tests need no real sockets.
- **I:** Peers expose a narrow `send_line` / `id` surface to the broadcaster—not full socket APIs.
- **D:** Chat domain depends on abstract connection ports; OS socket details stay at the edges.

## 4. EPICs

1. **Bootstrap & policy** — stack choice, NOTICE/headers, buffering policy doc.
2. **Server accept & line IO** — listen, read lines, handle disconnects.
3. **Nicknames & fan-out** — chat semantics parity.
4. **Client port** — stdin client matching oracle UX.
5. **Session CI** — differential scripts + docs.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & policy

- As a **student**, I want a BUFFERING.md decision (preserve vs upgrade), so that reviewers know the contract.
- As a **reviewer**, I want NOTICE + header attribution correct, so that license practice is safe.

### Epic 2 — Server accept & line IO

- As a **user**, I want the server to accept multiple TCP clients, so that a chat room exists.
- As a **developer**, I want clean disconnect handling, so that one dropped client does not kill the server.

### Epic 3 — Nicknames & fan-out

- As a **user**, I want to set a nickname, so that messages are attributable.
- As a **user**, I want my messages delivered to other clients, so that the room works.
- As a **user**, I want join/leave (or equivalent oracle messages) if upstream shows them, so that parity holds.

### Epic 4 — Client port

- As a **user**, I want a small CLI client that talks to the server, so that I do not rely only on `nc`.
- As a **developer**, I want the client to share framing helpers with the server where sensible, so that bugs do not diverge.

### Epic 5 — Session CI

- As a **reviewer**, I want a scripted 3-client transcript diff, so that grading is objective.
- As a **student**, I want intentional deltas listed, so that upgrades are not mistaken for regressions.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Project scaffold + NOTICE | E1 | P0 |
| F2 BUFFERING.md policy | E1 | P0 |
| F3 TCP listen + accept | E2 | P0 |
| F4 Line-oriented read/write | E2 | P0 |
| F5 Nickname command/flow | E3 | P0 |
| F6 Broadcast fan-out | E3 | P0 |
| F7 Disconnect resilience | E2 | P0 |
| F8 CLI client | E4 | P0 |
| F9 Differential session scripts | E5 | P0 |
| F10 Rooms / TLS | — | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Server accepts one client; echoes or logs lines.
2. **Slice B:** Two clients; broadcast works without nicknames.
3. **Slice C:** Nicknames + oracle-matching message format.
4. **Slice D:** Client binary + disconnect cases.
5. **Slice E:** Session script gate in CI / `make test-sessions`.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Server binds; policy + NOTICE committed |
| **Wave 2** | Multi-client IO | Slice B demo |
| **Wave 3** | Nick + format parity | Slice C vs oracle transcript |
| **Wave 4** | Client + disconnects | Slice D |
| **Wave 5** | Polish / edge cases | Session CI green; deltas documented |

## 9. Acceptance criteria / parity checklist outline

- [ ] ≥3 concurrent clients exchange messages with nicknames.
- [ ] Happy-path transcript matches oracle **or** listed upgrades with before/after fixtures.
- [ ] Server survives abrupt client disconnect.
- [ ] Client (or documented `nc` procedure) can complete the demo.
- [ ] NOTICE / file headers preserve antirez BSD-style terms; no fake root LICENSE for upstream code.
- [ ] Oracle commit hash recorded in README.
- [ ] No TLS/auth/rooms feature creep.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Silent buffering “fixes” break parity | Require BUFFERING.md + dual fixtures if upgraded |
| Racey fan-out in Go/Rust | Mutex or channel design review; stress script |
| Invented SPDX on upstream code | Copy headers; NOTICE only; refuse false MIT claim |
| Afternoon overrun on client polish | Server + `nc` scripts can gate P0 if README says so—prefer full client |

**License:** BSD-style **file headers**, **no root LICENSE**. Copy terms verbatim; do not invent SPDX for derived files. Cite antirez/smallchat. Pure new code may declare a compatible license with clear separation in NOTICE.
