# PRD — flasky Flask Book App → FastAPI / Django Ninja Port

**Pack:** `12-flasky` · **Upstream:** [miguelgrinberg/flasky](https://github.com/miguelgrinberg/flasky) · **License:** MIT

---

## 1. Synopsis / problem statement

Re-implement the **MVP core** of Miguel Grinberg’s flasky social app on **FastAPI** or **Django Ninja**, treating the Flask blueprints and user-visible social flows as the product contract. flasky is larger than microblog; students must **ruthlessly scope** the weekend to auth, profiles, posts, and follow feed—admin, mail, and peripheral APIs are deferred unless already trivial.

## 2. Goals & non-goals

**Goals**

- User registration / login / logout (document auth mechanism).
- Profile view + light edit (username/name/location/about subset matching oracle P0).
- Create text posts; personal timeline + followed feed.
- Follow / unfollow graph.
- Migrations + SQLite (or equivalent) schema spirit; OpenAPI or Django schema for APIs.
- Written **MVP cut list**: every non-ported blueprint marked P2 with rationale.

**Non-goals (ruthless — default P2)**

- Full admin/roles/permission matrix parity on day one.
- Email confirmation, mail queue, or Celery/RQ parity.
- Complete REST/API extras beyond what P0 UI/flows need.
- Feature invention (stories, reactions, DMs, GraphQL, realtime).
- Pixel-perfect Bootstrap clone; dual FastAPI **and** Django Ninja submissions.
- Porting every extension “because it exists.”

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `domain/user` | User entity, password hashing ports, profile fields |
| `domain/post` | Post entity, author relationship |
| `domain/follow` | Follow graph operations |
| `app/api` or `app/ninja` | Routers for auth, users, posts, feed |
| `app/deps` | DB session / request user dependencies |
| `app/schemas` | Pydantic (or DRF/Ninja schemas) request/response |
| `infra/db` | ORM + migrations |
| `infra/auth` | Session, JWT, or Django auth backend—**one** choice |
| `ui/` | Minimal templates/HTMX **or** SPA consuming API |

**Blueprint → module map (student fills from oracle)**

| Upstream blueprint / area | Weekend disposition |
|---------------------------|---------------------|
| Auth | **P0** |
| Main / posts / feed | **P0** |
| Profiles / user pages | **P0** |
| Admin / roles extras | **P2** |
| API extras / mail / misc | **P2** |

**SOLID mapping (prose)**

- **S:** Routers do not hash passwords; auth service owns credentials.
- **O:** New feed filters extend feed services without rewriting post CRUD.
- **L:** Auth mechanism sits behind a shared `CurrentUser` dependency.
- **I:** Narrow query services—no god `FlaskyService`.
- **D:** Domain depends on repository ports; ORM stays in `infra`.

## 4. EPICs

1. **Inventory & bootstrap** — blueprint cut list, app shell, migrations, empty schema docs.
2. **Auth** — register/login/logout + current-user dependency.
3. **Profiles** — view/edit subset.
4. **Posts & social** — create/list, follow/unfollow, followed feed.
5. **Parity & MVP honesty** — fixtures, P2 backlog published, weekend close-out.

## 5. User stories (by epic)

### Epic 1 — Inventory & bootstrap

- As a **student**, I want a completed P0/P2 blueprint table before coding, so that scope cannot silently expand.
- As a **student**, I want a runnable API/app with migrations, so that I can iterate with a real DB.
- As a **reviewer**, I want README stating FastAPI vs Django Ninja and auth choice, so that grading is fair.

### Epic 2 — Auth

- As a **user**, I want to register and log in/out, so that accounts work.
- As a **developer**, I want a declarative current-user dependency, so that routes stay clean.

### Epic 3 — Profiles

- As a **user**, I want to view a profile page, so that identity is visible.
- As a **user**, I want to edit a small profile field subset, so that the MVP feels complete.

### Epic 4 — Posts & social

- As a **user**, I want to create a text post and see it on my timeline, so that publishing works.
- As a **user**, I want to follow/unfollow and see a followed feed, so that social graph works.

### Epic 5 — Parity & MVP honesty

- As a **student**, I want HTTP fixtures for login→profile→post→follow, so that “done” is objective.
- As a **reviewer**, I want an explicit deferred list (admin/mail/API), so that ruthlessness is auditable.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Blueprint inventory + cut list | E1 | P0 |
| F2 App scaffold + migrations | E1 | P0 |
| F3 Register / login / logout | E2 | P0 |
| F4 Password hashing | E2 | P0 |
| F5 Profile view/edit subset | E3 | P0 |
| F6 Create + list posts | E4 | P0 |
| F7 Follow / unfollow + feed | E4 | P0 |
| F8 HTTP fixture replay | E5 | P0 |
| F9 Schema/OpenAPI export | E1 | P0 |
| F10 Admin / roles full matrix | — | P2 |
| F11 Email / async jobs | — | P2 |
| F12 Extra REST surfaces | — | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Cut list merged; app boots; migrations apply; health/schema empty shell.
2. **Slice B:** Register/login/logout roundtrip.
3. **Slice C:** Profile view/edit subset.
4. **Slice D:** Posts + follow graph + feed.
5. **Slice E:** Fixture suite green; P2 backlog published; no admin rabbit hole.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Inventory + shell | Slice A (**gate:** cut list reviewed) |
| **Wave 2** | Auth | Slice B |
| **Wave 3** | Profiles | Slice C |
| **Wave 4** | Posts + social | Slice D |
| **Wave 5** | Parity / honesty | Slice E; weekend retrospective |

**Wave scope rule:** Admin, mail, and non-P0 APIs stay closed until Slice E is green. If time slips, **cut UI chrome**—not the fixture suite.

## 9. Acceptance criteria / parity checklist outline

- [ ] Written P0/P2 blueprint table committed in repo.
- [ ] Register → login → edit profile → create post → follow → see feed (scripted).
- [ ] Logout prevents authenticated actions.
- [ ] Passwords hashed; migrations reproducible.
- [ ] Schema/OpenAPI (or equiv.) lists P0 routes only—or clearly marks stubs.
- [ ] README documents stack + auth choice + deferred list.
- [ ] MIT attribution; oracle commit recorded.
- [ ] No silent “almost admin”—deferred items are named.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| Scope sprawl vs microblog | Cut list gate in Wave 1; ruthless non-goals |
| Dual-stack indecision | Pick FastAPI **or** Django Ninja in Wave 1 |
| Book/extension chasing | P2 by default; fixtures > features |
| Auth bikeshedding | One mechanism; document; don’t dual-implement |

**License:** MIT upstream — retain notices; new code may remain MIT. Do not paste large copyrighted book text; link to public resources instead.
