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

### Current slide map (30 in the DOM; 26 navigable — 4 are hidden via `hidden-slide`, kept but skipped)

1. Title
2. Legal (safe harbor / forward-looking-statements notice)
3. Agenda (single flat list, no AM/PM split, no clock times, first item is a "Welcome" framing line — not a separate slide)
4–7. Foundations — "app" definition, **prescribed stack**, SDK + Fluent, Git as source of truth (stack moved up to precede the SDK+Fluent/Git detail slides — overview before drill-down)
Break — **hidden**
8. Build-ready requirements (vague vs. build-ready example)
9. Activity: rewrite the story → links to `exercises/01-build-ready-requirements.md`
10. Why sandboxes (concept: shared instance + agentic-speed change = last-write-wins; sandbox contains the blast radius, Git reconciles it; feature branch lives inside the sandbox)
11. Setup time
12. Build Agent (concept: what it is — conversational, platform-native, full-loop build+test)
13. Build Agent in ServiceNow Studio (screenshot cropped from `Build_agent_workshop.pdf` p.8 → `assets/images/build-agent-studio.png`)
14. Build Agent + ATF (concept: Test Agent generates ATF tests as you build)
15. Build + test the maintenance app (exercise) → links to `exercises/02-build-maintenance-app.md`
Q&A — **hidden**
16. Lunch
17. Re-anchor (Build and Test both shown as done at this point; no "this morning"/"this afternoon" framing)
Q&A — **hidden**
18. Off-instance development: AI Skills for Fluent (exercise) → links to `exercises/03-off-instance-development.md`
19. Source control (concept: Git vs. update sets table)
20. Source control exercise: ship it with Git → links to `exercises/04-source-control.md`
Break — **hidden**
21. ReleaseOps (concept: names Deployment Request vs. Release vs. Assessment as distinct objects; the two release paths — promote-Update-Set vs. Git-triggered CI/CD)
22. ReleaseOps (exercise) → links to `exercises/05-releaseops.md`
Q&A — **hidden**
23. Push to production (demo)
24. Choose your own adventure (menu: Fluent/Testing/CI-CD topic cards — still a TODO to finalize per audience)
25. Choose your own adventure (exercise) → links to `exercises/06-choose-your-own-adventure.md`
26. Wrap

Global sizing pass: base typography (h1/h2/p/lead/quote/list items), card/flow-box padding, and every per-slide `max-width` inline value were bumped up deck-wide so slides use more of the screen — there was a lot of unused blank space at the previous sizes.

Automated testing is no longer its own exercise — its ATF steps were folded into `exercises/02-build-maintenance-app.md` (retitled "Build + test the maintenance app"), and slide 14 above is what's left of its old concept content, now framed as a lead-in to the build exercise rather than its own exercise card. Slide-label times (e.g. "9:20–9:55") were removed deck-wide in favor of no exact times. ReleaseOps previously had a "LAB" pointer with no backing exercise file — `exercises/05-releaseops.md` now fills that gap.

Slides 12–13 (Build Agent / Build Agent in Studio) were added because jumping straight from Setup into the ATF-focused "Build Agent + ATF" slide had no general context for what Build Agent even is — sourced from a second reference deck, `~/Downloads/Build_agent_workshop.pdf` (not part of this repo). That PDF's roadmap/availability/pricing content was deliberately left out as out of scope for this workshop.

Exercise files are numbered 01–06 in day order, not creation order — `exercises/03-off-instance-development.md` (AI Skills for Fluent development, sourced from an internal ServiceNow bootcamp exercise, content fully inlined rather than linked) pushed source-control/releaseops/choose-your-own-adventure from 03/04/05 to 04/05/06. The Source Control slide previously linked out to an internal bootcamp page as a second "Bootcamp Exercise 04" link — that reference has been removed; the bootcamp's actual exercise 4 turned out to be the AI Skills content, not source control, hence the rename above rather than merging it into source control.

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
