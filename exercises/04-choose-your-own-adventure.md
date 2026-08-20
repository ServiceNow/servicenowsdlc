---
title: "Choose your own adventure"
slide: "20 — Choose your own adventure (exercise)"
---

# Choose your own adventure

[← Back to slide 20 in the deck](https://apatti-now.github.io/servicenowsdlc/#20)

## Objective

Go one level deeper on whichever part of the stack is most relevant to your team — pick one track and spend the remaining time there.

## Estimated Time

30 minutes

## Prerequisites

- Maintenance app built, tested, source-controlled, and pushed through ReleaseOps (Exercises 01–03)

## Exercise Steps

1. As a table, pick one track: Fluent deep dive, Testing deep dive, or CI/CD APIs.
2. **Fluent deep dive** — explore additional metadata types beyond what the maintenance app used, and try transforming/syncing a UI-builder-created artifact into Fluent source.
3. **Testing deep dive** — go past the golden-path test: try Test Agent triage on a deliberately broken test, then explore Cloud Runner and what a regression suite looks like.
4. **CI/CD APIs** — script a Git-triggered pipeline using `now-sdk cicd`, and add an approval gate.
5. Be ready to show the group what you found.

## Success Criteria

- [ ] Your table went hands-on with at least one track beyond what the core exercises covered
- [ ] You can point to something concrete (a command, a config, a passing/failing test) from that track

## Learning Points

- The core exercises cover the golden path end-to-end; each track here is where that golden path gets deeper in practice.

## Bonus Challenge

- Combine two tracks — e.g., add a CI/CD approval gate that only promotes if the Testing deep-dive regression suite passes
