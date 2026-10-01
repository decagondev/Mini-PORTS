# PRD — TodoMVC Modern Framework Port

**Pack:** `01-todomvc` · **Upstream:** [tastejs/todomvc](https://github.com/tastejs/todomvc) · **License:** MIT

---

## 1. Synopsis / problem statement

Students re-implement the TodoMVC application in one modern UI framework (React, Vue, Svelte, or Solid), treating the official **app-spec** and legacy example behavior as the product contract. The problem is not “build a todo app from imagination”—it is **parity under a written spec** with localStorage persistence and hash routing, without inventing features.

## 2. Goals & non-goals

**Goals**

- Behavioral parity with TodoMVC `app-spec.md` (P0 checklist).
- Clean component/module boundaries suitable for the chosen framework.
- Persistence and routing matching the canonical examples.
- Runnable demo + automated or scripted verification in an afternoon.

**Non-goals**

- Pixel-perfect CSS clone of a specific example theme (behavioral parity wins).
- Backend, auth, sync, or multi-user features.
- Copying a maintained modern official example as the submission.
- Framework bake-offs inside one repo (pick **one** target stack).

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/todo` | Todo entity, ids, completed flag; pure reducers (add/edit/toggle/destroy/clearCompleted) |
| `domain/filter` | Active / completed / all filter predicates |
| `app/store` | Application state + use-cases; no DOM |
| `infra/storage` | `localStorage` adapter (serialize/hydrate) |
| `infra/router` | Hash-route ↔ filter binding |
| `ui/*` | Framework components (header, list, item, footer); I/O only |

**SOLID mapping (prose)**

- **S:** Domain reducers do not know about React/Vue; storage does not know about filters UI.
- **O:** New filter modes (if ever) extend filter module without rewriting list rendering.
- **L:** Storage port can be swapped (memory ↔ localStorage) behind the same interface for tests.
- **I:** UI depends on narrow store hooks/selectors, not a god-object.
- **D:** Framework UI depends on domain abstractions; domain does not import framework or `window` except via injected ports.

## 4. EPICs

1. **Bootstrap & shell** — toolchain, empty app shell, TodoMVC CSS/base markup hooks.
2. **Core CRUD** — add, list, toggle, edit, destroy.
3. **Filters & bulk actions** — routing filters, clear completed, items-left count.
4. **Persistence** — hydrate/persist localStorage without losing parity semantics.
5. **Parity hardening** — edge cases, empty states, Cypress/spec checklist close-out.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & shell

- As a **student**, I want a runnable empty shell with the standard TodoMVC layout hooks, so that I can iterate visually from hour one.
- As a **reviewer**, I want a clear README stating from-example and target framework, so that I can judge originality.

### Epic 2 — Core CRUD

- As a **user**, I want to add a todo from the input and see it in the list, so that I can capture tasks.
- As a **user**, I want to mark a todo complete/incomplete, so that I can track progress.
- As a **user**, I want to edit a todo’s title inline, so that I can fix mistakes.
- As a **user**, I want to delete a todo, so that I can remove noise.

### Epic 3 — Filters & bulk actions

- As a **user**, I want All / Active / Completed filters via hash routes, so that I can focus the list.
- As a **user**, I want “clear completed,” so that I can reset finished work.
- As a **user**, I want an accurate items-left count, so that I know remaining work.

### Epic 4 — Persistence

- As a **user**, I want todos to survive refresh, so that the app is useful.
- As a **developer**, I want storage behind a port, so that tests can run without flaky disk coupling.

### Epic 5 — Parity hardening

- As a **student**, I want a checkboxed parity list from `app-spec.md`, so that “done” is objective.
- As a **user**, I want correct empty-state footer/input behaviors, so that the UI matches the spec.

## 6. Features (mapped to stories)

| Feature | Stories | Notes |
|---------|---------|-------|
| F1 Scaffold + base styles | E1 | Vite/equivalent; don’t invent design system |
| F2 Add todo | E2 | Trim; reject empty |
| F3 Toggle one / toggle all (if spec requires) | E2–E3 | Follow app-spec exactly |
| F4 Inline edit + commit/cancel | E2 | Spec keybindings |
| F5 Destroy item | E2 | |
| F6 Hash filters | E3 | `#/`, `#/active`, `#/completed` |
| F7 Clear completed + count | E3 | |
| F8 localStorage round-trip | E4 | Key/name per canonical example policy |
| F9 Spec checklist + tests | E5 | Cypress or mirrored cases |

## 7. Slices (vertical, runnable)

1. **Slice A:** Empty shell renders; input visible; no persistence.
2. **Slice B:** Add + list + toggle + delete in memory.
3. **Slice C:** Edit + footer count + clear completed.
4. **Slice D:** Hash filters wired end-to-end.
5. **Slice E:** Persistence + full P0 checklist green.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | `npm run dev` (or equiv.) shows shell |
| **Wave 2** | In-memory CRUD | Slice B demo |
| **Wave 3** | Filters + footer actions | Slice C–D |
| **Wave 4** | Persistence | Refresh keeps data |
| **Wave 5** | Polish / edge cases | P0 parity checklist complete; known P2s listed |

## 9. Acceptance criteria / parity checklist outline

- [ ] Matches `app-spec.md` P0 behaviors (add/edit/complete/destroy/filter/clear/persist/route).
- [ ] No extra features beyond spec.
- [ ] Empty list / all-complete / mixed states behave per spec.
- [ ] Hash navigation updates filter and URL.
- [ ] localStorage hydrate does not duplicate or drop items.
- [ ] README documents framework, how to run, and upstream attribution.
- [ ] Automated tests **or** recorded Cypress/manual script attached.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Accidental copy of modern official example | Start from jquery/legacy; disclose source example |
| CSS rabbit hole | Cap time; accept behavioral parity |
| Spec drift vs chosen Cypress version | Pin to upstream app-spec text as source of truth |
| localStorage key mismatch | Copy key convention from chosen legacy example |

**License:** MIT upstream — retain notices; new code may remain MIT. Do not remove tastejs copyright from reused assets.
