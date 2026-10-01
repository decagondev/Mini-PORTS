# PRD — microblog Flask → FastAPI Port

**Pack:** `10-microblog` · **Upstream:** [miguelgrinberg/microblog](https://github.com/miguelgrinberg/microblog) · **License:** MIT

---

## 1. Synopsis / problem statement

Re-implement Miguel Grinberg’s microblog (auth, posts, followers, profiles) on **FastAPI** with **SQLAlchemy 2** and **Pydantic**, treating the Flask app + Mega-Tutorial feature order as the product contract. Students ship a weekend vertical slice with behavioral parity on core social flows—not a full rewrite of every extension, email task, or optional deployment chapter.

## 2. Goals & non-goals

**Goals**

- User registration/login/logout (session **or** JWT—pick one, document it).
- Create text posts; view personal and followed timelines.
- Follow / unfollow; basic profile view/edit (username/bio subset).
- SQLAlchemy 2 models + Alembic (or equiv.) migrations; SQLite by default.
- OpenAPI for JSON APIs; Jinja/HTMX **or** simple SPA for UI (pick one).

**Non-goals**

- Full email stack, Elasticsearch full-text, RQ/Celery parity on day one (P2 unless already trivial).
- Flask-Admin / every deployment target (Heroku/Docker orthodoxy).
- Feature invention (stories, reactions, DMs, GraphQL).
- Pixel-perfect Bootstrap clone (behavioral parity wins).

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/user` | User entity, password hashing ports |
| `domain/post` | Post entity, author relationship |
| `domain/follow` | Follow graph operations |
| `app/api` | FastAPI routers (auth, users, posts, feed) |
| `app/deps` | DB session, current user dependencies |
| `app/schemas` | Pydantic request/response models |
| `infra/db` | SQLAlchemy 2 engine/session/Alembic |
| `infra/auth` | Session cookie or JWT issuer/verifier |
| `ui/` | Jinja/HTMX templates **or** SPA consuming OpenAPI |

**SOLID mapping (prose)**

- **S:** Routers do not hash passwords inline; auth service owns credentials.
- **O:** New feed filters extend feed services without rewriting post CRUD.
- **L:** Auth backend (session vs JWT) sits behind a `CurrentUser` dependency other routers share.
- **I:** Feed endpoints depend on narrow query services—not a god `MicroblogService`.
- **D:** Domain/use-cases depend on abstract repositories; SQLAlchemy stays in `infra`.

## 4. EPICs

1. **Bootstrap & schema** — FastAPI app, DB, migrations, empty OpenAPI.
2. **Auth** — register/login/logout + current-user dependency.
3. **Posts** — create/list own posts.
4. **Social graph** — follow/unfollow + followed feed.
5. **Profiles & parity** — profile view/edit, fixtures, weekend close-out.

## 5. User stories (by epic)

### Epic 1 — Bootstrap & schema

- As a **student**, I want a runnable FastAPI app with migrations, so that I can iterate with a real DB.
- As a **reviewer**, I want README run instructions + stack choices (session vs JWT, UI mode), so that I can grade fairly.

### Epic 2 — Auth

- As a **user**, I want to register with username/email/password, so that I can obtain an account.
- As a **user**, I want to log in and out, so that my session is controlled.
- As a **developer**, I want a `get_current_user` dependency, so that routes stay declarative.

### Epic 3 — Posts

- As a **user**, I want to create a text post, so that I can publish updates.
- As a **user**, I want to see my posts on my profile/timeline, so that history is visible.
- As a **user**, I want empty states to be sane, so that new accounts are usable.

### Epic 4 — Social graph

- As a **user**, I want to follow another user, so that their posts appear in my feed.
- As a **user**, I want to unfollow, so that I can revise subscriptions.
- As a **user**, I want a home feed of followed users’ posts (plus/minus self per oracle policy), so that the product matches microblog.

### Epic 5 — Profiles & parity

- As a **user**, I want to view and lightly edit a profile, so that identity is more than a username.
- As a **student**, I want HTTP fixtures replaying login→post→follow, so that “done” is objective.
- As a **reviewer**, I want P2 deferrals listed (email, search, async jobs), so that scope is honest.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 FastAPI + SQLAlchemy 2 scaffold | E1 | P0 |
| F2 Alembic migrations (users/posts/followers) | E1 | P0 |
| F3 Register / login / logout | E2 | P0 |
| F4 Password hashing (bcrypt/argon2) | E2 | P0 |
| F5 Create + list posts | E3 | P0 |
| F6 Follow / unfollow | E4 | P0 |
| F7 Followed feed | E4 | P0 |
| F8 Profile view/edit subset | E5 | P0 |
| F9 HTTP fixture replay | E5 | P0 |
| F10 OpenAPI export | E1 | P0 |
| F11 Email confirmation / reset | — | P2 |
| F12 Full-text search | — | P2 |
| F13 Background jobs | — | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** App boots; `/health`; migrations apply; OpenAPI empty shell.
2. **Slice B:** Register/login/logout roundtrip (API and/or UI).
3. **Slice C:** Authenticated post create + list.
4. **Slice D:** Follow graph + home feed.
5. **Slice E:** Profile edit + fixture suite green; P2 backlog published.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Shell/bootstrap | Slice A |
| **Wave 2** | Auth | Slice B |
| **Wave 3** | Posts | Slice C |
| **Wave 4** | Social feed | Slice D |
| **Wave 5** | Profiles / parity | Slice E; weekend retrospective |

## 9. Acceptance criteria / parity checklist outline

- [ ] Register → login → create post → follow → see post in follower feed (scripted).
- [ ] Logout prevents authenticated actions.
- [ ] Passwords stored hashed; no plaintext in DB.
- [ ] Migrations reproducible from empty DB.
- [ ] OpenAPI lists auth/posts/feed/profile routes.
- [ ] README documents session vs JWT choice and UI mode.
- [ ] MIT attribution; oracle commit + P0 chapter mapping recorded.
- [ ] P2 items (email/search/jobs) explicitly deferred—not silently missing.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Session vs JWT bikeshedding | Decide in Wave 1; don’t dual-implement |
| Tutorial scope sprawl | Cap P0 to auth/posts/follow/profile; list P2 |
| Sync Flask habits in async FastAPI | Prefer sync SQLAlchemy 2 session pattern first; async optional |
| UI pixel chasing | API+minimal templates acceptable for P0 |

**License:** MIT upstream — retain notices; new code may remain MIT. Do not paste large copyrighted tutorial/book text; link to public tutorial resources instead.
