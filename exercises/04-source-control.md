---
title: "Source control: ship it with Git"
slide: "19 — Source control exercise"
---

# Source control: ship it with Git

## Objective

Walk the full Git-based collaboration flow — push, PR, review, merge, publish, pull — and see firsthand why update sets can't replace it.

## Estimated Time

30 minutes (2:30–3:00)

## Prerequisites

- Feature branch with committed work from Exercise 02/03

## Exercise Steps

1. **Off instance (GitHub):** commit and push your feature branch.
2. Open a pull request. Pair with another team and have them review it — leave at least one comment.
3. Resolve the feedback and merge to `main`.
4. From your base dev instance, publish a pre-release of your app to the Application Repository.
5. **On instance:** open Studio/IDE (SNS) on a second instance and pull the merged changes using git.
6. Confirm the pulled app reflects the merged change.

> **Prescriptive point:** Git does branching, review, and line-by-line conflict resolution. Update sets cannot.

## Success Criteria

- [ ] Merged PR with visible review history (at least one comment addressed)
- [ ] Pre-release published to the Application Repository
- [ ] Changes pulled and visible on a second instance

## Learning Points

- Review and conflict resolution are collaboration primitives Git provides natively — update sets have no equivalent, only last-write-wins.
- The App Repo, not the update set, is what makes an install on a second instance reproducible and auditable.

## Bonus Challenge

- Intentionally create a merge conflict with your review partner (both edit the same field) and resolve it line-by-line
- Discuss: how would that same conflict have played out with two update sets instead?

Full walkthrough reference: [Exercise 04 — Bootcamp content](https://code.devsnc.com/pages/dev/bootcamp-content/modules/dev-environment/exercises/exercise-04.html)
