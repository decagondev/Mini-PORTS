# 13 — AFNetworking Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/AFNetworking/AFNetworking |
| **License** | MIT |
| **From → to** | Objective-C networking stack → **Swift** `URLSession` + **async/await** facade (**curated API subset**) |
| **Effort band** | **Weekend** (~8–16 h) |
| **~Stars** | ~33.4k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

AFNetworking is the historic ObjC HTTP networking library: sessions, request/response serializers, and convenience managers. This pack is **not** a UI app—“done” means a **Swift compatibility facade** over `URLSession` that covers a **curated subset** of session + serializer concepts (GET/POST JSON, headers, error mapping). Ignore UIKit image categories and sprawling reachability UI helpers in v1.

**Why it ports well:** Concepts map cleanly (session, serializers, managers); MIT; unit tests / header surface define the oracle. **Hard part:** Defining the subset so the weekend ends.

## Learning outcomes

- Map ObjC headers/blocks → Swift protocols, closures, and `async`/`await`.
- Design a narrow facade over `URLSession` without re-implementing the entire AF* surface.
- Port or rewrite characterization tests for the curated subset.
- Practice API inventory → P0/P2 cut before coding.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; inventory public headers → curated subset table.
2. Create a **new MIT-attributed** Swift package (or Xcode framework); cite AFNetworking/AFNetworking.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Generate Swift protocol stubs from ObjC headers for the **subset only**; ignore UIKit image categories in v1.
5. Prefer SPM-friendly layout; keep upstream tests as oracle where license/structure allows.

## Suggested verification

- Unit tests for: JSON GET/POST, custom headers, status-code error mapping, cancellation.
- Side-by-side characterization: same request fixtures → compare status/body/error classification vs oracle behavior for the subset.
- Public Swift API doc comments listing **supported** vs **explicitly unsupported** AF* symbols.
- No UIKit/AppKit demo required for P0 (optional smoke app is P2).

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add yours on new files.
- Do not claim drop-in binary/ObjC compatibility unless you actually provide it (this pack does **not** require that).
- Record oracle commit hash and the curated symbol list in README/`PARITY.md`.

## Links

- Upstream: https://github.com/AFNetworking/AFNetworking
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
