---
title: "Source control: ship it with Git"
slide: "19 — Source control exercise"
time: "2:30–3:00"
type: "exercise"
---

# Source control: ship it with Git

**Time:** 2:30–3:00 (30 min)
**Type:** Exercise

## Objective

Walk the full Git-based collaboration flow — push, PR, review, merge, publish, pull — and see firsthand why update sets can't replace it.

## Prerequisites

- Feature branch with committed work from Exercise 02/03

## Instructions

1. **Off instance (GitHub):** commit and push your feature branch.
2. Open a pull request. Pair with another team and have them review it — leave at least one comment.
3. Resolve the feedback and merge to `main`.
4. From your base dev instance, publish a pre-release of your app to the Application Repository.
5. **On instance:** open Studio/IDE (SNS) on a second instance and pull the merged changes using git.
6. Confirm the pulled app reflects the merged change.

> **Prescriptive point:** Git does branching, review, and line-by-line conflict resolution. Update sets cannot.

## Success criteria

- [ ] Merged PR with visible review history (at least one comment addressed)
- [ ] Pre-release published to the Application Repository
- [ ] Changes pulled and visible on a second instance

## Stretch goals (optional)

- Intentionally create a merge conflict with your review partner (both edit the same field) and resolve it line-by-line
- Discuss: how would that same conflict have played out with two update sets instead?

## Facilitator notes (optional)

- Full walkthrough reference: [Exercise 04 — Bootcamp content](https://code.devsnc.com/pages/dev/bootcamp-content/modules/dev-environment/exercises/exercise-04.html)
- Pairing teams for PR review adds a few minutes but is worth it — the review step is the point, not a formality.
- If a team has no real conflicts to resolve, the stretch goal is the fastest way to make the Git-vs-update-sets contrast concrete rather than abstract.
