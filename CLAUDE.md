# AI-Led SDLC Enablement — Workshop Deck

This repo holds a single-page slide deck (`index.html`) plus companion exercise write-ups (`exercises/`) for a full-day customer workshop: **AI-Led SDLC Enablement for Customers**. Originally scaffolded via the `brown-bag-setup` Claude Code skill, then rewritten to match a live workshop agenda and grounded in the ServiceNow SDK SDLC guide (`servicenow.github.io/sdk/guides/sdlc-guide`).

## Mirror repo

This deck is mirrored from a primary internal repo so it's also reachable from a personal GitHub account:

| Repo | Remote | Live deck |
|---|---|---|
| Primary (`sdlccontent`) | `code.devsnc.com/ashwin-patti/sdlccontent` (Gitea) | https://code.devsnc.com/pages/ashwin-patti/sdlccontent/ |
| **This repo** (`servicenowsdlc`) | `github.com/apatti-now/servicenowsdlc` | https://apatti-now.github.io/servicenowsdlc/ |

**These are two independent repos kept in sync by hand — not git remotes of each other.** Any change to `index.html`, `README.md`, or `exercises/` should be applied to both working trees and pushed to both remotes. The only intentional difference between the two copies is the deck/exercise link URLs — each points at its own Pages host.

> Gotcha: in a single tool-call session, `cd` does not persist across separate shell invocations — always `cd /path && git ...` in one command per repo rather than relying on a prior `cd`. Pushes to this repo require the `apatti-now` GitHub account to be the active `gh auth` identity (not `apatti`), since `apatti` has no write access here.

## Slide deck (`index.html`)

Single self-contained HTML file — no build step, no dependencies beyond two Google Fonts. Structure:

- One `<div class="slide" data-index="N">` per slide, inside `<div class="deck">`. `data-index` is cosmetic/documentation only — actual slide order and count come from DOM order via `document.querySelectorAll('.slide:not(.hidden-slide)')`.
- Add `class="hidden-slide"` to a slide to skip it without deleting it.
- Deep-link any slide with `#N` (1-based) — the hash tracks the current slide as you navigate, so any slide has a shareable URL.
- Theme is "midnight" (dark blue/green), defined in `assets/themes.mjs` inside the `brown-bag-setup` skill (`~/.claude/skills/brown-bag-setup`, on the machine that maintains the primary repo). Regenerate via `node $SKILL_DIR/scripts/generate.mjs --theme <name>` — **this overwrites all hand-edited slide content**, so only run it before customizing, or manually copy the new `:root` variable block afterward.

### Current slide map (25 slides)

1. Title
2. Agenda (AM/PM table)
3–6. Foundations — "app" definition, SDK + Fluent, Git as source of truth, prescribed stack
7. Break
8. Build-ready requirements (vague vs. build-ready example)
9. Activity: rewrite the story → links to `exercises/01-build-ready-requirements.md`
10. Setup time
11. Build the maintenance app (exercise) → links to `exercises/02-build-maintenance-app.md`
12. Q&A
13. Lunch
14. Re-anchor
15. Automated testing (exercise) → links to `exercises/03-automated-testing.md`
16. Q&A
17. Off-instance development
18. Source control (concept: Git vs. update sets table)
19. Source control exercise: ship it with Git → links to `exercises/04-source-control.md`
20. Break
21. ReleaseOps (quality-control model + lab)
22. Q&A
23. Push to production (demo)
24. Choose your own adventure (placeholder — topics are a TODO, finalize per audience before running)
25. Wrap

## Exercises (`exercises/`)

`TEMPLATE.md` defines the section order every exercise follows:

```
Title, Objective, Estimated Time, Prerequisites, Exercise Steps, Success Criteria, Learning Points, Bonus Challenge
```

Frontmatter (`title`, `slide`) is metadata only, kept as **plain text** — not a Markdown link. GitHub/Gitea render frontmatter as a raw key/value table and don't parse Markdown syntax inside a cell; a `[text](url)` value there shows literally with only the bare URL auto-linked, which looks broken. The actual clickable link to the deck lives in the body instead.

Cross-linking convention:
- **Slide → exercise:** a small `📄 Exercise steps →` line at the bottom of the relevant slide's content, pointing at that exercise file's blob view on this repo's own Git host.
- **Exercise → slide:** a `← Back to slide N in the deck` line right under the exercise's `# Title`, pointing at this repo's own Pages URL with a `#N` deep link.

To add a new exercise: copy `TEMPLATE.md`, number it next in sequence, fill in all eight sections, then add the two cross-links once you know which slide it belongs to — and repeat both the exercise file and the slide-side link in the primary repo.

## Editing workflow

1. Edit `index.html` / `exercises/*.md` in **both** `sdlccontent` and `servicenowsdlc` working trees — they're independent copies, not synced automatically.
2. Commit and push each repo separately.
3. Gitea Pages / GitHub Pages both serve straight from `main`, no CI, no build step — changes go live roughly 30–60 seconds after push.
