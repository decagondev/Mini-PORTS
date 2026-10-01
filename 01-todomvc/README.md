# 01 — TodoMVC Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/tastejs/todomvc |
| **License** | MIT |
| **From → to** | jQuery / legacy JS example → React 19 / Vue 3 / Svelte 5 / Solid (pick one) |
| **Effort band** | **Afternoon** (~2–4 h) |
| **~Stars** | ~29k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

TodoMVC is the canonical “same todo app, many frameworks” teaching suite. It ships an explicit **`app-spec.md`** and Cypress coverage so behavioral parity is not a matter of taste. Starting from a legacy example (e.g. `examples/jquery`) and re-implementing in a modern framework is an ideal first port: small UI, clear persistence (`localStorage`), and well-defined routing (hash).

**Why it ports well:** Identical product across examples; written spec + test suite as golden oracle; no networking or native platform surface.

## Learning outcomes

- Read a written product spec and refuse feature invention.
- Separate DOM/state from framework components (extract model → view bindings).
- Use differential / E2E tests (Cypress or equivalent) as the acceptance gate.
- Practice fork-and-port attribution under MIT.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle** (do not rewrite in-place).
2. Create a **new repo** for the port; copy LICENSE / NOTICE; cite tastejs/todomvc.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Feed `app-spec.md` to your AI session verbatim; forbid inventing features.
5. Prefer starting from `examples/jquery` (or another *legacy* example)—do not copy a maintained modern example wholesale.

## Suggested verification

- Run upstream Cypress / `npm run test:all` patterns against your framework id where applicable.
- Manual parity checklist from `app-spec.md` (add, edit, complete, clear completed, filters, persistence, routing).
- Optional: screenshot diff of empty / populated / filtered states (behavioral parity > pixel-perfect CSS).

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add your own copyright on *new* files.
- Do not strip upstream headers from any copied assets (CSS template, etc.).
- Document the chosen from-example path in your README so reviewers know you did not lift a modern official example.

## Links

- Upstream: https://github.com/tastejs/todomvc
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
