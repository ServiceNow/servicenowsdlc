---
title: "Source control: ship it with Git"
slide: "22 — Source control exercise"
---

# Source control: ship it with Git

[← Back to slide 22 in the deck](https://servicenow.github.io/servicenowsdlc/#22) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/05-source-control.ja.md)

## Objective

Walk the full Git-based collaboration flow — push, PR, review, merge, publish, pull — and see firsthand why update sets can't replace it.

## Estimated Time

30 minutes

## Prerequisites

- Feature branch with committed work from Exercise 03

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

## Success Criteria

- [ ] Merged PR with visible review history

## Learning Points

- Review and conflict resolution are collaboration primitives Git provides natively — update sets have no equivalent, only last-write-wins.
- The App Repo, not the update set, is what makes an install on a second instance reproducible and auditable — publishing your merged app there is exactly what Exercise 06 starts with.

## Bonus Challenge

- Intentionally create a merge conflict with your review partner (both edit the same field) and resolve it line-by-line
- Discuss: how would that same conflict have played out with two update sets instead?
