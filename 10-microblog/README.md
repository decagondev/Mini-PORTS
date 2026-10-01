# 10 — microblog Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/miguelgrinberg/microblog |
| **License** | MIT |
| **From → to** | Flask mega-tutorial app → **FastAPI** + **SQLAlchemy 2** / **Pydantic** (Jinja or HTMX/React SPA) |
| **Effort band** | **Weekend** (~8–16 h) |
| **~Stars** | ~4.8k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

microblog is Miguel Grinberg’s Flask Mega-Tutorial application: auth, profiles, posts, followers, and DB-backed flows. Tutorial chapters act as an **ordered product spec**. The port replaces Flask blueprints/sessions/Flask-Login idioms with FastAPI routers, dependency injection, SQLAlchemy 2, and Pydantic models—keeping SQLite (or equivalent) schema spirit and replaying HTTP fixtures (login → post → follow).

**Why it ports well:** Familiar domain; chapter order = natural waves; MIT; clear “social microblog” MVP without inventing features.

## Learning outcomes

- Map Flask blueprints / `g` / session login → FastAPI `APIRouter` + dependencies.
- Replace WTForms-style validation with Pydantic models.
- Preserve schema & user-visible flows via HTTP record/replay fixtures.
- Choose **session cookies vs JWT** deliberately and document the security tradeoff.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; skim Mega-Tutorial chapter order as the backlog.
2. Create a **new MIT-attributed** repo; cite miguelgrinberg/microblog.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Build OpenAPI from routes early; keep SQLite schema close to upstream spirit.
5. Capture HTTP fixtures from the Flask app (login → post → follow) before rewriting auth.

## Suggested verification

- Replay HTTP fixtures (curl/httpx/`schemathesis` smoke) against port: register/login, create post, follow/unfollow, timeline.
- Compare DB rows or JSON resources for the same scripted user journey.
- Manual checklist from tutorial features marked P0 in the PRD.
- Optional: OpenAPI snapshot diff for route surface.

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add yours on new files.
- Tutorial prose is explanatory—don’t paste large copyrighted book text into the repo; link chapters instead.
- Record oracle commit hash and which tutorial feature chapters you treat as P0.

## Links

- Upstream: https://github.com/miguelgrinberg/microblog
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
