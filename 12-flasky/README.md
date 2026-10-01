# 12 — flasky Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/miguelgrinberg/flasky |
| **License** | MIT |
| **From → to** | Flask + extensions (book app) → **FastAPI** or **Django Ninja** (pick one); optional HTMX UI |
| **Effort band** | **Weekend** (~8–16 h) — **ruthless MVP scope** |
| **~Stars** | ~8.7k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

flasky is Miguel Grinberg’s larger Flask social application from *Flask Web Development*: blueprints, auth, profiles, posts, followers, roles/admin-shaped surface, and more extensions than microblog. Same author patterns as [`10-microblog`](../10-microblog/), but the weekend only succeeds with a **ruthless MVP**—auth + profiles + posts + follow feed—before admin/API extras.

**Why it ports well:** Familiar domain; blueprint map → routers is teachable; MIT; microblog pack is a warm-up. **Why it’s harder:** More blueprints/extensions invite scope sprawl.

## Learning outcomes

- Architecture-map Flask blueprints/extensions → FastAPI routers or Django Ninja APIs.
- Ruthlessly cut P0 vs P2 (admin, mail, API extras) before coding.
- Preserve core social flows via HTTP record/replay fixtures.
- Document session vs JWT (or Django auth) tradeoffs deliberately.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; inventory blueprints → P0/P2 table **first**.
2. Create a **new MIT-attributed** repo; cite miguelgrinberg/flasky.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Port **auth + profiles** before admin/API extras; do not dual-implement frameworks.
5. Optionally reuse lessons from the microblog pack, but treat flasky’s oracle as source of truth for this pack.

## Suggested verification

- Replay HTTP fixtures: register/login → edit profile → create post → follow → feed.
- Compare DB rows or JSON resources for the same scripted journey.
- Manual P0 checklist; OpenAPI (or Django schema) snapshot for route surface.
- Explicit written list of deferred admin/mail/API features (must not be “accidentally missing”).

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add yours on new files.
- Book prose is copyrighted—**link** to the book/resources; do not paste large book text into the repo.
- Record oracle commit hash and the P0 blueprint subset you honor.

## Links

- Upstream: https://github.com/miguelgrinberg/flasky
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
- Related pack: [10-microblog](../10-microblog/)
