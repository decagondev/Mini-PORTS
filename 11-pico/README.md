# 11 — Pico Port Pack

| Field | Value |
|-------|-------|
| **Upstream** | https://github.com/picocms/Pico |
| **License** | MIT |
| **From → to** | Legacy PHP flat-file CMS → **Astro** / **Eleventy** / Node static pipeline **or** modern PHP 8.x rewrite (pick one) |
| **Effort band** | **Weekend** (~8–16 h) |
| **~Stars** | ~3.9k (approx.; drifts) |
| **PRD** | [PRD.md](./PRD.md) |

## Synopsis

Pico is a flat-file PHP CMS: Markdown content on disk, YAML front matter, Twig-like themes, and optional plugins. The content tree is the product oracle—freeze a sample `content/` corpus and snapshot HTML per URL. Port to a modern static/SSG stack (Astro/Eleventy) or a clean PHP 8.x core; treat plugin/theme hook parity as a **later phase**, not the weekend MVP.

**Why it ports well:** Files are golden fixtures; URL → rendered page mapping is deterministic; MIT; clear “CMS without a DB” teaching story.

## Learning outcomes

- Map flat-file Markdown + front matter → SSG content collections or a modern PHP content loader.
- Separate content parsing, routing, and theme rendering (theme plugins deferred).
- Build HTML snapshot / golden-URL tests from a frozen content tree.
- Practice fork-and-port attribution under MIT without inventing CMS features.

## How to use this pack

1. **Fork / clone** upstream read-only as the **oracle**; freeze a sample `content/` (+ theme assets you will honor).
2. Create a **new MIT-attributed** repo; cite picocms/Pico.
3. Read [PRD.md](./PRD.md); implement Wave 1 → last wave; only open P0 stories.
4. Capture oracle HTML for each golden URL **before** rewriting themes.
5. Defer plugin API and third-party theme drop-in compatibility to P2.

## Suggested verification

- Freeze `content/` fixtures; compare rendered HTML (or DOM text) per URL vs oracle Pico.
- Front-matter meta (title, description, date, template) round-trips into page output.
- 404 / missing page and nested folder URL behavior match oracle policy.
- Manual P0 checklist in the PRD; optional visual diff (behavioral parity > pixel-perfect CSS).

## Attribution / license notes

- Upstream is **MIT**. Keep copyright notices; add yours on new files.
- Do not strip Pico headers from reused theme/CSS assets you copy.
- Record oracle commit hash and which content tree / theme you treat as P0.

## Links

- Upstream: https://github.com/picocms/Pico
- Planning doc: [PRD.md](./PRD.md)
- Ranking context: [../RANKING.md](../RANKING.md)
