# AI-Led SDLC Enablement — Workshop Deck

This repo holds a single-page slide deck (`index.html`) plus companion exercise write-ups (`exercises/`) for a full-day customer workshop: **AI-Led SDLC Enablement for Customers**. Originally scaffolded via the `brown-bag-setup` Claude Code skill, then rewritten to match a live workshop agenda and grounded in the ServiceNow SDK SDLC guide (`servicenow.github.io/sdk/guides/sdlc-guide`).

## Mirror repo (retired as of 2026-08-24 — do not mirror going forward)

This deck used to be mirrored by hand from a primary internal repo so it was also reachable from a personal GitHub account:

| Repo | Remote | Live deck |
|---|---|---|
| Primary (`sdlccontent`) — **no longer updated** | `code.devsnc.com/ashwin-patti/sdlccontent` (Gitea) | https://code.devsnc.com/pages/ashwin-patti/sdlccontent/ |
| **This repo** (`servicenowsdlc`) — **sole target going forward** | `github.com/ServiceNow/servicenowsdlc` | https://servicenow.github.io/servicenowsdlc/ |

**Decision: stop mirroring.** `servicenowsdlc` is now the only repo that gets edited, committed, and pushed. `sdlccontent` is left as-is (stale) and should not be touched or kept in sync — do not apply changes there, and do not ask whether a change should be "repeated in the primary repo." Everything below that references keeping the two repos in sync (the old dual-push editing workflow, "repeat in the primary repo" instructions, etc.) is superseded by this decision.

> Gotcha (historical, only relevant if `sdlccontent` is ever revisited): in a single tool-call session, `cd` does not persist across separate shell invocations — always `cd /path && git ...` in one command per repo rather than relying on a prior `cd`. Pushes to `servicenowsdlc` require the `apatti-now` GitHub account to be the active `gh auth` identity (not `apatti`), since `apatti` has no write access here.

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
24. Choose your own adventure (exercise) — menu cards (Fluent/Testing/CI-CD) merged with the exercise-link slide into one; no more "as a table" / "show the group" framing, just "explore what interests you, we're here for questions" → links to `exercises/06-choose-your-own-adventure.md`
25. Wrap
26. Thank You (closing slide — links to the public SDLC guide, https://servicenow.github.io/sdk/guides/sdlc-guide)

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
- **Slide → exercise:** a `.exercise-link` pill (see `index.html` CSS, `/* ─── Exercise link chip ─── */`) wrapped in an `.exercise-links` container at the bottom of the relevant slide's content, reading "Open the exercise →" (no emoji — deliberately dropped in favor of the pill's own background/border signaling it's clickable), pointing at that exercise file's blob view on this repo's own Git host.
- **Exercise → slide:** a `← Back to slide N in the deck` line right under the exercise's `# Title`, pointing at this repo's own Pages URL with a `#N` deep link.

To add a new exercise: copy `TEMPLATE.md`, number it next in sequence, fill in all eight sections, then add the two cross-links once you know which slide it belongs to. (No more repeating this in a primary repo — see "Mirror repo" above.)

## Japanese localization (`index.ja.html`, `exercises/*.ja.md`)

Parallel Japanese files exist alongside every English file: `index.ja.html` mirrors `index.html`, and each `exercises/NN-*.ja.md` mirrors its English counterpart. This is for the Japan delivery of this workshop.

- **Always keep them in sync.** Any content change to `index.html` or `exercises/*.md` (new/edited/removed slide, new/edited exercise, changed cross-link, changed image, etc.) must be applied to the matching `.ja.html` / `.ja.md` file in the same pass — don't leave the Japanese version stale. If a change is purely structural/internal (e.g. an HTML authoring comment, a CSS tweak with no visible text) and has no user-facing text to translate, still check whether it affects the JA file (e.g. a shared CSS rule) and apply it there too.
- **Terminology convention** — ServiceNow-specific product/feature nouns stay in English inside otherwise-Japanese text: `Build Agent`, `Test Agent`, `ATF`, `Fluent`, `SDK`, `ReleaseOps`, `Deployment Request`, `Release`, `Update Set`, `App Repo`/`Application Repository`, `Instance Scan`/`Move to Test`/`Run ATF`/`Ready for Deploy`/`Retest`/`Need Code Change`/`Sign Off`/`Draft`/`Ready to Assess` (workflow/state labels), `Connect Hub`, `AI Control Tower`, `CI/CD`, `MCP`, and third-party proper nouns (`GitHub`, `VS Code`, `Claude Code`, `Cursor`, `Codex`, `Figma`, `Miro`, `React`/`Svelte`/`Vue`/`Preact`, `TypeScript`/`JavaScript`). Generic software-engineering vocabulary gets katakana-ized instead, since it's an already-lexicalized loanword in Japanese tech writing (`sandbox`→サンドボックス, `branch`→ブランチ, `merge`→マージ, `pull request`→プルリクエスト, `instance`→インスタンス, `repository`→リポジトリ). Coined pedagogical terms specific to this deck (e.g. "build-ready") stay in English with な/の attached, matching common JP engineering-blog style.
- **Code/commands/example prompts** (terminal blocks, Fluent `.now.ts` snippets, literal Build Agent prompts in the exercises) stay in English — only the surrounding prose/instructions around them get translated. Table/field/role names used as example metadata (`Maintenance Request`, `Equipment`, `Requester`, `Technician`, `New`) also stay in English so they match what the app actually generates.
- **Legal/safe-harbor slide is a special case.** Slide 2 in `index.ja.html` intentionally holds placeholder text, not a translation — it's SEC-boilerplate forward-looking-statements language (Form 10-K/10-Q references) and needs a legal/IR-approved Japanese version, not a freehand one. Leave the placeholder (and the HTML comment preserving the English source) in place until that approved text is provided; don't translate it yourself.
- **Cross-links:** every deck (both languages) carries a small fixed `.lang-switch` pill (top-right) linking to the other language's `index.html`/`index.ja.html`. Every exercise file carries a `| [日本語版](...)` or `| [English version](...)` link next to its "back to slide" line, pointing at the other language's file via this repo's GitHub blob view (same convention as the `.exercise-link` chip, not the raw Pages path).
- `sdlccontent` (see "Mirror repo" above) does **not** get a Japanese version — it's retired, so only `servicenowsdlc`'s JA files matter.

## Editing workflow

1. Edit `index.html` / `exercises/*.md` in this (`servicenowsdlc`) working tree only — and apply the same content change to `index.ja.html` / the matching `exercises/*.ja.md` (see "Japanese localization" above).
2. Commit and push.
3. GitHub Pages serves straight from `main`, no CI, no build step — changes go live roughly 30–60 seconds after push.
