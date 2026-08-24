---
title: "Source control: ship it with Git"
slide: "20 — Source control exercise"
---

# Source control: ship it with Git

[← Back to slide 20 in the deck](https://apatti-now.github.io/servicenowsdlc/#20)

## Objective

Walk the full Git-based collaboration flow — push, PR, review, merge, publish, pull — and see firsthand why update sets can't replace it.

## Estimated Time

30 minutes

## Prerequisites

- Feature branch with committed work from Exercise 02

## Exercise Steps

1. **Off instance (GitHub):** commit and push your feature branch.
   ```
   git add .
   git commit -m "Add maintenance request app"
   git push -u origin feature/maintenance-app
   ```
   Don't want to type these yourself? Ask Claude Code (or your coding agent) to commit and push the branch for you.
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
- The pre-release you just published is exactly what ReleaseOps picks up next — Exercise 05 starts by promoting an Update Set built from that same Application Repository artifact into a Deployment Request.

## Bonus Challenge

- Intentionally create a merge conflict with your review partner (both edit the same field) and resolve it line-by-line
- Discuss: how would that same conflict have played out with two update sets instead?
