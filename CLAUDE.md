# AI-Led SDLC Enablement — Workshop Deck

This repo holds a single-page slide deck (`index.html`) plus companion exercise write-ups (`exercises/`) for a full-day customer workshop: **AI-Led SDLC Enablement for Customers**. Originally scaffolded via the `brown-bag-setup` Claude Code skill, then rewritten to match a live workshop agenda and grounded in the ServiceNow SDK SDLC guide (`servicenow.github.io/sdk/guides/sdlc-guide`).

## Mirror repo (retired as of 2026-08-24 — do not mirror going forward)

This deck used to be mirrored by hand from a primary internal repo so it was also reachable from a personal GitHub account:

| Repo | Remote | Live deck |
|---|---|---|
| Primary (`sdlccontent`) — **no longer updated** | `code.devsnc.com/ashwin-patti/sdlccontent` (Gitea) | https://code.devsnc.com/pages/ashwin-patti/sdlccontent/ |
| **This repo** (`servicenowsdlc`) — **sole target going forward** | `github.com/ServiceNow/servicenowsdlc` | https://servicenow.github.io/servicenowsdlc/ |

**Decision: stop mirroring.** `servicenowsdlc` is now the only repo that gets edited, committed, and pushed. `sdlccontent` is left as-is (stale) and should not be touched or kept in sync — do not apply changes there, and do not ask whether a change should be "repeated in the primary repo." Everything below that references keeping the two repos in sync (the old dual-push editing workflow, "repeat in the primary repo" instructions, etc.) is superseded by this decision.

> Gotcha (historical, only relevant if `sdlccontent` is ever revisited): in a single tool-call session, `cd` does not persist across separate shell invocations — always `cd /path && git ...` in one command per repo rather than relying on a prior `cd`.

Pushes to **this** repo (`github.com/ServiceNow/servicenowsdlc`) go over SSH. The repo-local `user.name`/`user.email` here are set to the `ShelbyCohen` GitHub identity (`Shelby Cohen` / `7768053+ShelbyCohen@users.noreply.github.com`), not the `shelby.cohen@servicenow.com` devsnc identity that's the machine's global git default — don't let a global-config change silently flip commits back to the wrong identity. Separately, this network has intermittently blocked either port 22 or port 443 to `github.com` (never both at once so far) — if a push/pull fails with a connection timeout or reset, check `~/.ssh/config`'s `Host github.com` block and flip between direct port 22 and the `ssh.github.com:443` fallback; it's a network issue, not a credentials issue, every time this has come up.

## Slide deck (`index.html`)

Single self-contained HTML file — no build step, no dependencies beyond two Google Fonts. Structure:

- One `<div class="slide" data-index="N">` per slide, inside `<div class="deck">`. `data-index` is cosmetic/documentation only — actual slide order and count come from DOM order via `document.querySelectorAll('.slide:not(.hidden-slide)')`.
- Add `class="hidden-slide"` to a slide to skip it without deleting it.
- Deep-link any slide with `#N` (1-based) — the hash tracks the current slide as you navigate, so any slide has a shareable URL.
- Theme is "midnight" (dark blue/green), defined in `assets/themes.mjs` inside the `brown-bag-setup` skill (`~/.claude/skills/brown-bag-setup`, on the machine that maintains the primary repo). Regenerate via `node $SKILL_DIR/scripts/generate.mjs --theme <name>` — **this overwrites all hand-edited slide content**, so only run it before customizing, or manually copy the new `:root` variable block afterward.

### Current slide map (39 in the DOM; 34 navigable — 5 are hidden via `hidden-slide`, kept but skipped)

1. Title
2. Legal (safe harbor / forward-looking-statements notice)
3. Agenda (single flat list, no AM/PM split, no clock times, first item is a "Welcome" framing line — not a separate slide)
4–7. Foundations — "app" definition, **prescribed stack**, SDK + Fluent, Git as source of truth (stack moved up to precede the SDK+Fluent/Git detail slides — overview before drill-down). Slide 6 (SDK + Fluent) does **not** carry an exercise link — that moved to slide 8, see below.
8. **Get source control ready** — a short numbered-step activity (initialize a Fluent app with nowSDK → create/push a git repo and connect the instance with a personal access token → build and install) that links to `exercises/02-creating-a-fluent-app.md`, where the actual repo-creation instructions live. Positioned right after "Git is the authoritative source of truth" (slide 7) — moved here from its original spot (right before the sandbox-setup slide) because it belongs with the Foundations block conceptually, not with hands-on-sandbox setup. Content also changed: originally a checklist + `git config` terminal snippet that didn't actually instruct anyone to create a repo; now it's an activity slide (matching the "Activity" pattern) that sends attendees straight to the exercise.
Break — **hidden**
9. Build-ready requirements (vague vs. build-ready example)
10. Activity: rewrite the story → links to `exercises/01-build-ready-requirements.md`
11. Why sandboxes (concept: shared instance + agentic-speed change = last-write-wins; sandbox contains the blast radius, Git reconciles it; feature branch lives inside the sandbox)
12. Setup time ("Get your sandbox ready")
13. Build Agent (concept: what it is — conversational, platform-native, full-loop build+test)
14. Build Agent in ServiceNow Studio (screenshot cropped from `Build_agent_workshop.pdf` p.8 → `assets/images/build-agent-studio.png`)
15. Build Agent + ATF (concept: Test Agent generates ATF tests as you build)
16. Build + test the maintenance app (exercise) → links to `exercises/03-build-maintenance-app.md`. Opens Build Agent from **the IDE**, not Studio (fixes the IDE-vs-Studio inconsistency noted in the ReleaseOps deep dive's roadmap slide — this exercise now matches the rest of the day's IDE-first framing). App name must be attendee-unique: `Maintenance_<YourName>`.
Q&A — **hidden**
17. Lunch
18. Re-anchor (Build and Test both shown as done at this point; no "this morning"/"this afternoon" framing)
Q&A — **hidden**
19. Off-instance development: AI Skills for Fluent (exercise) → links to `exercises/04-off-instance-development.md`
20. Source control exercise: ship it with Git → links to `exercises/05-source-control.md`. **The "Where Git goes further" concept slide (Git vs. Update Sets table) that used to precede this was removed** — it was redundant with slide 7's Git-vs-Update-Sets comparison, and slide 8 now covers the hands-on git setup earlier anyway.
Break — **hidden**
21. **ReleaseOps: Two objects, one pipeline** (concept: names Deployment Request vs. Release vs. Assessment as distinct objects; the two release paths — promote-Update-Set vs. Git-triggered CI/CD) — title now explicitly says "ReleaseOps:" (was just "Two objects, one pipeline," which wasn't clear out of context even with the `.slide-label` already reading "ReleaseOps"). Carries an `.exercise-link` chip to `releaseops-deep-dive.html` (see "Companion decks" below)
22. ReleaseOps (exercise) — the lab (create a Deployment Request, watch the assessment run, handle Retest/Need Code Change/Sign Off) — **no longer links to `exercises/06-releaseops.md` directly**; that exercise is now reached via the Choose Your Own Adventure "ReleaseOps" track instead (see slide 24)
Q&A — **hidden**
23. Push to production (demo)
24. Choose your own adventure (exercise) — menu cards merged with the exercise-link slide into one; no more "as a table" / "show the group" framing, just "explore what interests you, we're here for questions" → links to `exercises/07-choose-your-own-adventure.md`. 6 track cards: Fluent deep dive, Testing deep dive, CI/CD APIs, MCP integrations, Custom rules, **ReleaseOps** (added when ReleaseOps stopped being a mandatory exercise — see slide 22). Prerequisite line accordingly reads "Exercises 01–05."
25. Wrap
26. Thank You (closing slide — links to the public SDLC guide, https://servicenow.github.io/sdk/guides/sdlc-guide)

**27–34: Bonus slides**, appended after Thank You, time/interest permitting — not part of the core day. `SPEAKER_NOTES.md` used to track which of these were verified vs. placeholder but was deliberately removed from this repo (kept only in the separate `sdlc-internal` repo instead — see below), so the flags live here now:
27. Bonus: Build Agent — unified SNS & Spec Mode — **placeholder**, unverified
28. Bonus: What's coming in the September release — **placeholder**, unverified — overlaps with 33 below, worth merging or differentiating
29. Bonus: ATF in Spec Mode — **placeholder**, unverified
30. Bonus: Generating ATF tests on existing legacy apps — **placeholder**, unverified
31. Bonus: Build Agent + MCP — **real, grounded content** (from the public SDK docs: Connect Hub + AI Control Tower admin setup, then "Enable MCP servers" in Build Agent Settings)
32. Bonus: Custom rules — changing language — **placeholder**, unverified, and unlike the others we don't even have a firm shape for what "custom rule" means here yet
33. Bonus: Sneak peek — what's next — **placeholder**, unverified — overlaps with 28
34. Bonus (demo only): Warpspeed demo — **real**, links to `https://saplingapp.com/` (Warpspeed is a workflow-automation platform — equipment requests, time-off approvals, invoicing, access permissions); framed as a live demo like the Push to Production slide, not a placeholder

The 5 hidden slides, for completeness (they don't get a number above): Break (after slide 8), Q&A (after slide 16), Q&A (after slide 18), Break (after slide 20), Q&A (after slide 22).

**A note on these slide numbers:** they come from actually walking the DOM in order and counting non-hidden `.slide` divs — not from the (frequently stale) `<!-- SLIDE N: ... -->` authoring comments inside `index.html`/`index.ja.html`. Those comments are pure documentation with no functional role, and have drifted out of sync with real nav position more than once now (each inserted/removed/moved slide shifts everything after it, and downstream comments don't always get renumbered in the same pass). If you need the *true* current slide number for something — a deep-link, a frontmatter `slide:` field — count `.slide:not(.hidden-slide)` divs in DOM order yourself rather than trusting a comment. This has already happened twice: once when "Get source control ready" was first inserted (slide count 34→35), and again when it was relocated and the old "Source control" concept slide was removed in the same pass (35→34, net).

Global sizing pass: base typography (h1/h2/p/lead/quote/list items), card/flow-box padding, and every per-slide `max-width` inline value were bumped up deck-wide so slides use more of the screen — there was a lot of unused blank space at the previous sizes.

### Companion decks

`releaseops-deep-dive.html` (repo root, alongside `index.html`) — a second, separate single-file deck reusing the exact same engine/CSS as `index.html` (`.slide`/`.deck`/nav/keyboard/deep-link JS all copied verbatim), for material that's too deep for the core-day pace. Linked from slide 21's `.exercise-link` chip ("Deep dive: full architecture + walkthrough →"), and links back via the same chip pattern (plus a second chip to the ReleaseOps exercise) — both now point at `#21`, following the slide renumbering above. 26 slides, architecture-first: the 6 architecture concept slides (from source control to Deployment Request, the two flows, freeze vs. release date, why a two-instance dev→test topology doesn't work, the assessment playbook stage-by-stage with each stage's sad path, pause points as the #1 adoption blocker) come first, unchanged in position; then the 8-step walkthrough (build & install from the IDE → Studio publish → Update Set → Release record → Deployment Request → activate → Ready to Assess → the Workflow Studio playbook diagram per stage → setting up and resolving a manual pause point) — described **in words** (`ul.clean` step lists), not screenshots, per a deliberate decision to keep the deck text-first and put the click-by-click screenshots in the exercise instead (see below). The one exception: the four Workflow Studio playbook-stage diagrams (Instance Scan, Move to Test, Run ATF, Ready for Deploy) keep their screenshots, since those diagrams aren't something you can describe in a bullet and aren't part of the hands-on exercise either — they land at **slides 18–21**. After that: the roadmap-gaps slide ("What ReleaseOps can't do yet") is deliberately **not last** — a "Deployed to prod" wrap-up slide sits between it and the closing slide, so the deck doesn't end on a list of limitations. Slide 2's title reads "From source control to Deployment Request" (was "Why Update Sets, not source control") and no longer carries the "built roughly three years ago... known architectural constraint, not a design goal" line — that framing read as negative about Update Sets; the slide is now just the flow diagram, no editorializing. Adds one new CSS block not in `index.html` — `.shots`/`.shot`/`.shot-single` (now used only by the 4 stage-diagram slides) and `.quote.tone-orange` — everything else is copy-pasted from `index.html`'s `<style>` block, so the two files' shared CSS should be kept in sync by hand if the base theme changes.

**Screenshots live with the exercise, not the deck.** All 33 screenshots from the live walkthrough recorded 2026-08-26 with Robert are in `exercises/images/releaseops/` and embedded in a new "Appendix: Full Walkthrough (Screenshots)" section at the end of `exercises/06-releaseops.md` — that's the click-by-click version (steps 2–8, plus the 4 stage diagrams again for completeness). `assets/images/releaseops-deep-dive/` keeps only the 5 files the deck's slides 18–21 actually reference (the 4 stage diagrams, one of which — Move to Test — uses 2 images). Don't add screenshots back into `releaseops-deep-dive.html` beyond those 4 stages without re-checking this decision; the whole point was to keep the deck word-first and push pixel-level detail into the exercise.

**Now has a Japanese mirror.** `releaseops-deep-dive.ja.html` mirrors `releaseops-deep-dive.html` slide-for-slide, following the same terminology/code-stays-English conventions as the rest of the repo (see "Japanese localization" below) — treat it exactly like `index.ja.html` from here on: any content change to the English deck gets applied there too, in the same pass. Both files carry the standard `.lang-switch` pill. The new SDK exercise (`exercises/02-creating-a-fluent-app.md`) does **not** have a `.ja.md` mirror yet — that one's still pending, unlike the deep dive.

Speaker notes for this material (if produced) belong in the separate `sdlc-internal` repo alongside `SPEAKER_NOTES.md` — see "Related local repos" below — not inlined into this HTML file.

Automated testing is no longer its own exercise — its ATF steps were folded into `exercises/03-build-maintenance-app.md` (retitled "Build + test the maintenance app"), and slide 15 above is what's left of its old concept content, now framed as a lead-in to the build exercise rather than its own exercise card. Slide-label times (e.g. "9:20–9:55") were removed deck-wide in favor of no exact times. `exercises/06-releaseops.md` still exists and is fully valid content — it's just no longer directly linked from slide 22; it's reached via the CYOA ReleaseOps track (slide 24) instead.

Slides 13–14 (Build Agent / Build Agent in Studio) were added because jumping straight from Setup into the ATF-focused "Build Agent + ATF" slide had no general context for what Build Agent even is — sourced from a second reference deck, `~/Downloads/Build_agent_workshop.pdf` (not part of this repo). That PDF's roadmap/availability/pricing content was deliberately left out as out of scope for this workshop.

**Exercise files are numbered 01–07 in day order, not creation order.** History, oldest to newest:
- `exercises/03-off-instance-development.md` (AI Skills for Fluent development, sourced from an internal ServiceNow bootcamp exercise, content fully inlined rather than linked) originally pushed source-control/releaseops/choose-your-own-adventure from 03/04/05 to 04/05/06. The Source Control slide previously linked out to an internal bootcamp page as a second "Bootcamp Exercise 04" link — that reference has been removed; the bootcamp's actual exercise 4 turned out to be the AI Skills content, not source control, hence the rename above rather than merging it into source control.
- `exercises/02-creating-a-fluent-app.md` (initialize a Fluent app with nowSDK, then connect it to git) was inserted right after Exercise 01, pushing what was 02–06 to 03–07. This exercise originally had Step 3 (Define a table) and Step 4 (Define a business rule) — both were removed per feedback (they duplicated the main build exercise and complicated the app-scope-collision story); what's left is Step 1 (init), Step 2 (git setup), Step 3 (build/install), Step 4 (verify). The removed table/BR content now lives in the Bonus Challenge as an optional extension instead. Step 2's git setup intentionally doesn't spell out the ServiceNow-instance-side click path in detail — it points forward to Exercise 05 (source control) for that, since that's where the full push/PR/review/pull flow actually gets exercised hands-on.
- `exercises/05-source-control.md` no longer has the "Prescriptive point" callout ("Git does branching, review, and line-by-line conflict resolution. Update sets cannot.") — cut for the same "don't editorialize against Update Sets" reason as the deep dive's slide 2. Success Criteria's first bullet also dropped the "(at least one comment addressed)" parenthetical.
- Every exercise's frontmatter `slide:` field and "← Back to slide N" link, and every cross-exercise reference by number (in both languages), were renumbered in step with each slide-map change above — see the note at the end of the slide map about not trusting stale in-file comments for slide numbers.

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

## Related local repos & files (outside this repo, all under `~/sdlc-workshop/`)

- **`servicenowsdlc/`** — the original `apatti-now/servicenowsdlc` clone. This is the retired mirror described at the top of this file — don't edit it, don't sync it, don't ask whether a change belongs there too.
- **`sdlc-internal/`** — a separate private repo (`git@code.devsnc.com:shelby-cohen/sdlc-internal.git`) holding `SPEAKER_NOTES.md` and `facilitator-qa-reference.md`. These used to also live inside this repo but were deliberately removed from here — this is the customer-facing repo, and both files contain internal-only material (presenter notes, real customer Q&A from Slack threads, known-limitation flags) that shouldn't be public. Commit identity there should be the devsnc identity (`shelby.cohen@servicenow.com`, git's global default), not `ShelbyCohen` — opposite of this repo.
- **`facilitator-qa-reference.md`** at the top level of `~/sdlc-workshop/` (not inside any repo) — the working copy of the Q&A reference; `sdlc-internal/facilitator-qa-reference.md` is a synced copy of it, not the other way around. Copy forward into `sdlc-internal` after editing, then commit/push there.
- **`workshop-plan-presentation.html`** — a completely separate, standalone deck (same slide engine/CSS architecture as this repo's `index.html`, but a distinct deep-violet/gold palette) for an internal "here's our plan for these workshops" presentation — not the customer workshop content, not part of any repo, and not something to keep in sync with this repo's slides.
