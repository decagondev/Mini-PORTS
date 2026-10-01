# PRD — Pico Flat-File CMS Modernization Port

**Pack:** `11-pico` · **Upstream:** [picocms/Pico](https://github.com/picocms/Pico) · **License:** MIT

---

## 1. Synopsis / problem statement

Re-implement Pico’s flat-file CMS core—Markdown pages with YAML front matter, folder-based routing, and a theme/template layer—on **Astro**, **Eleventy**, Node, **or** modern PHP 8.x. Students treat a frozen `content/` tree + oracle HTML as the product contract and ship a weekend vertical slice with URL/content parity. Plugin hooks and full theme-compat are explicitly out of Wave scope unless already trivial.

## 2. Goals & non-goals

**Goals**

- Load Markdown + front matter from a content directory; emit HTML pages.
- Preserve folder → URL routing spirit (index pages, nested paths).
- Apply a single theme/template (Twig-like concepts → chosen engine components/layouts).
- Golden HTML (or text) snapshot suite for a frozen content corpus.
- Document chosen stack (Astro vs Eleventy vs PHP 8.x) in README.

**Non-goals**

- Full Pico plugin API / event-hook parity (P2 phase).
- Drop-in compatibility with arbitrary third-party Pico themes.
- Admin UI, user accounts, comments, search engines, or DB backends.
- Inventing CMS features (Wysiwyg, media library, multilingual matrix) beyond oracle P0.
- Pixel-perfect CSS clone of the default theme (behavioral/content parity wins).

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/page` | Page entity: path, slug, front matter, body AST/HTML |
| `domain/nav` | Optional menu/order derived from meta or folder order |
| `app/content` | Scan content tree; parse Markdown + YAML |
| `app/router` | Path → page resolution; 404 policy |
| `app/render` | Theme/layout apply; asset URLs |
| `infra/fs` | Read-only content + theme asset loaders |
| `infra/md` | Markdown engine adapter (unified/markdown-it/league/commonmark, etc.) |
| `ui/theme` | Layouts/partials for the **one** chosen theme |

**SOLID mapping (prose)**

- **S:** Markdown parser does not know about HTTP; router does not parse YAML.
- **O:** New front-matter keys extend page model/mappings without rewriting the scanner.
- **L:** Content source port can swap disk ↔ in-memory fixtures for tests.
- **I:** Theme depends on a narrow `PageViewModel`, not the full CMS god-object.
- **D:** Domain/page logic depends on abstractions; filesystem and MD engines stay in `infra`.

## 4. EPICs

1. **Bootstrap & content load** — toolchain, content scanner, empty layout.
2. **Routing & pages** — URL resolution, index pages, nested paths, 404.
3. **Front matter & meta** — title/description/date/template fields → output.
4. **Theme render** — one theme applied end-to-end; asset wiring.
5. **Parity hardening** — golden HTML suite, edge cases, weekend close-out.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & content load

- As a **student**, I want a runnable site that reads a frozen `content/` tree, so that I can iterate without inventing pages.
- As a **reviewer**, I want README stating stack choice and oracle commit, so that I can grade fairly.

### Epic 2 — Routing & pages

- As a **visitor**, I want `/` and nested Markdown paths to resolve to pages, so that the site matches Pico URL spirit.
- As a **visitor**, I want missing URLs to 404 sanely, so that broken links are obvious.
- As a **developer**, I want index.md / folder conventions documented, so that fixtures stay stable.

### Epic 3 — Front matter & meta

- As an **author**, I want YAML front matter to set title and description, so that `<title>` / meta match oracle intent.
- As an **author**, I want dates/order fields (if in P0 fixtures) honored, so that listing pages stay consistent.
- As a **developer**, I want unknown front-matter keys ignored or passed through per documented policy, so that ports don’t crash.

### Epic 4 — Theme render

- As a **visitor**, I want pages wrapped in a consistent layout, so that the site feels like a CMS theme.
- As a **student**, I want one theme only for P0, so that weekend scope stays honest.

### Epic 5 — Parity hardening

- As a **student**, I want HTML snapshots per golden URL, so that “done” is objective.
- As a **reviewer**, I want plugins/theme-compat listed as P2, so that non-goals are visible.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Scaffold + content scanner | E1 | P0 |
| F2 Markdown → HTML body | E1 | P0 |
| F3 Folder/URL routing + 404 | E2 | P0 |
| F4 Front-matter title/description/date subset | E3 | P0 |
| F5 Single theme/layout | E4 | P0 |
| F6 Golden HTML snapshot suite | E5 | P0 |
| F7 Plugin event API | — | P2 |
| F8 Multi-theme drop-in compat | — | P2 |
| F9 Search / tags taxonomy matrix | — | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** App/site boots; one hardcoded page from disk Markdown.
2. **Slice B:** Full content tree scan; all pages routable; 404 works.
3. **Slice C:** Front matter drives title/meta; nested URLs match fixtures.
4. **Slice D:** Chosen theme wraps all pages; assets load.
5. **Slice E:** Snapshot suite green vs oracle; P2 backlog published.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell + one page | Slice A |
| **Wave 2** | Tree routing | Slice B |
| **Wave 3** | Front matter | Slice C |
| **Wave 4** | Theme | Slice D |
| **Wave 5** | Parity / close-out | Slice E; weekend retrospective |

**Wave scope rule:** Do **not** open plugin API or multi-theme work until Slice E is green (or explicitly re-scoped by instructor).

## 9. Acceptance criteria / parity checklist outline

- [ ] Frozen content corpus renders every golden URL with matching body text / agreed HTML subset.
- [ ] Front-matter title (and P0 meta fields) appear in output.
- [ ] Nested paths and index conventions match documented oracle policy.
- [ ] 404 for unknown paths.
- [ ] Single theme applied consistently.
- [ ] README documents stack, how to run, oracle commit, content tree path.
- [ ] MIT attribution present; no silent feature invention.
- [ ] Plugins / theme-compat explicitly deferred as P2.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Plugin/theme rabbit hole | Hard non-goal; Wave scope rule above |
| Markdown dialect drift | Pin one engine; note dialect vs oracle Pico |
| HTML whitespace noise in snapshots | Compare normalized DOM text or agreed selectors |
| SSG vs “dynamic CMS” confusion | P0 is content→HTML parity; live PHP rewrite is optional path |

**License:** MIT upstream — retain notices; new code may remain MIT. Do not remove Pico copyright from reused assets.
