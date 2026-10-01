# PRD — AFNetworking → Swift URLSession Async Facade

**Pack:** `13-afnetworking` · **Upstream:** [AFNetworking/AFNetworking](https://github.com/AFNetworking/AFNetworking) · **License:** MIT

---

## 1. Synopsis / problem statement

Port a **curated subset** of AFNetworking’s session/serializer concepts to a Swift facade over `URLSession` using async/await (and protocols for serializers). Students invent **no** product UI—the deliverable is a library API + tests. Success is measured by subset parity and clear documentation of unsupported AF* surfaces—not by cloning every category and manager.

## 2. Goals & non-goals

**Goals**

- Swift package (or framework) with session-like client: configure base URL/headers, perform data tasks.
- Request serialization subset: URL-encoded and JSON body encoding for common cases.
- Response serialization subset: raw `Data`, JSON (`Codable` or `Any`), string.
- Async API (`async throws`) as primary; optional completion-handler wrappers if useful for pedagogy.
- Error mapping for HTTP status / transport failures documented vs oracle spirit.
- Curated symbol table: AF* → Swift equivalent | unsupported.

**Non-goals**

- Full AFNetworking API surface / drop-in ObjC compatibility.
- UIKit image download categories, UIActivityIndicator bindings, or UI helpers.
- Multipart upload matrix, comprehensive security-policy UI, or every reachability edge case (P2).
- Custom URL protocols, SOCKS deep configuration, or server-trust pinning lab beyond a stub hook (P2).
- Shipping an iOS sample app as P0 (optional P2).

## 3. Target architecture

**Modular boundaries**

| Module | Responsibility |
|--------|----------------|
| `HTTPClient` | Session configuration, send request, cancellation |
| `RequestSerializer` | Build `URLRequest` (JSON / form / raw) |
| `ResponseSerializer` | Decode `Data` → typed result |
| `HTTPError` | Status + transport error model |
| `URLSessionTransport` | Thin adapter over `URLSession` (injectable for tests) |
| `Tests/Fixtures` | URLProtocol stubs or local mock server |

**SOLID mapping (prose)**

- **S:** Serializers do not own session lifetime; client does not parse JSON inline.
- **O:** New response serializers extend without rewriting `HTTPClient`.
- **L:** Transport port allows `URLSession` ↔ mock without changing callers.
- **I:** Callers depend on narrow client APIs—not a god “AFHTTPSessionManager clone.”
- **D:** Client depends on `Transport` and serializer protocols; Foundation networking stays behind the adapter.

## 4. EPICs

1. **API inventory & package shell** — symbol cut list, SPM/Xcode package, empty client.
2. **Transport & GET** — async GET data + status errors.
3. **Serializers** — JSON/form request + JSON/string response.
4. **POST + headers + cancel** — write paths and cancellation.
5. **Parity docs & tests** — fixture suite, unsupported list, weekend close-out.

## 5. User stories (by epic)

### Epic 1 — API inventory & package shell

- As a **student**, I want a curated AF* → Swift symbol table before coding, so that scope is finite.
- As a **reviewer**, I want a buildable Swift package with README, so that I can run tests.

### Epic 2 — Transport & GET

- As a **developer**, I want `get(url) async throws → Data`, so that the happy path works.
- As a **developer**, I want non-2xx responses mapped to typed errors, so that callers can branch.

### Epic 3 — Serializers

- As a **developer**, I want JSON request encoding, so that POST bodies match common AF usage.
- As a **developer**, I want JSON response decoding to `Codable` or dictionary, so that apps can consume APIs.

### Epic 4 — POST + headers + cancel

- As a **developer**, I want default and per-request headers, so that auth tokens work.
- As a **developer**, I want task cancellation, so that view dismissal patterns are teachable.

### Epic 5 — Parity docs & tests

- As a **student**, I want fixture-based tests green for the subset, so that “done” is objective.
- As a **reviewer**, I want an explicit unsupported AF* list, so that the facade is honest.

## 6. Features (mapped to stories)

| Feature | Stories | Priority |
|---------|---------|----------|
| F1 Symbol cut list + package scaffold | E1 | P0 |
| F2 Async GET + error mapping | E2 | P0 |
| F3 JSON/form request serializers | E3 | P0 |
| F4 JSON/string response serializers | E3 | P0 |
| F5 POST + headers | E4 | P0 |
| F6 Cancellation | E4 | P0 |
| F7 Fixture/unit tests | E5 | P0 |
| F8 Unsupported API doc | E5 | P0 |
| F9 Image categories / UI helpers | — | P2 |
| F10 Multipart + pinning lab | — | P2 |
| F11 Sample iOS app | — | P2 |

## 7. Slices (vertical, runnable)

1. **Slice A:** Package builds; cut list committed; client stub compiles.
2. **Slice B:** GET + status error tests green (mocked transport).
3. **Slice C:** JSON GET decode + form/JSON encode paths.
4. **Slice D:** POST + headers + cancel tests green.
5. **Slice E:** Parity docs; unsupported list; weekend retrospective.

## 8. Waves (time-boxed)

| Wave | Focus | Exit criteria |
|------|-------|---------------|
| **Wave 1** | Inventory + shell | Slice A (**gate:** subset frozen) |
| **Wave 2** | GET transport | Slice B |
| **Wave 3** | Serializers | Slice C |
| **Wave 4** | Write path + cancel | Slice D |
| **Wave 5** | Docs / parity | Slice E |

**Wave scope rule:** Do not expand the symbol subset mid-weekend without shrinking another P0. UIKit categories stay closed.

## 9. Acceptance criteria / parity checklist outline

- [ ] Curated symbol table committed; unsupported AF* symbols listed.
- [ ] Async GET/POST JSON paths tested with mocked transport or local server.
- [ ] Header merge behavior documented and tested for P0 cases.
- [ ] Cancellation prevents completion / throws `CancellationError` (document chosen behavior).
- [ ] No UIKit image category code in P0.
- [ ] README: how to build/test, oracle commit, subset rationale.
- [ ] MIT attribution on new files; upstream notices preserved where code was consulted/ported.

## 10. Risks & license notes

| Risk | Mitigation |
|------|------------|
| “Compat” scope explosion | Freeze subset in Wave 1; reject new symbols |
| URLSession subtlety bikeshed | Prefer simple defaults; document deviations from AF |
| Testing without device | Mock `URLProtocol` / inject transport |
| Claiming drop-in AF replacement | Explicitly disclaim in README |

**License:** MIT upstream — retain notices; new Swift code may remain MIT. Do not strip AFNetworking copyright from any copied/translated files.
