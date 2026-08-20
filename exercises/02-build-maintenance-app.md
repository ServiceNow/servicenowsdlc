---
title: "Build + test the maintenance app"
slide: "11 — Build"
---

# Build + test the maintenance app

[← Back to slide 11 in the deck](https://apatti-now.github.io/servicenowsdlc/#11)

## Objective

Use Build Agent (and ATF) to go from a natural-language prompt to a working maintenance-request app, using the build-ready requirements your team wrote earlier.

## Estimated Time

60 minutes

## Prerequisites

- Sandbox allocated and feature branch created
- Build-ready requirements from the previous activity (entities, roles/access, Given-When-Then)
- "Sync ATF tests with app" turned on in Build Agent settings (General tab) — this is what generates ATF tests as you build and keeps them synced as the app changes

![Build Agent settings with "Sync ATF tests with app" toggled on](images/ba-settings-atf.png)

## Exercise Steps

1. Open Build Agent in Studio, we are covering off-instance later.
2. Prompt Build Agent with your build-ready requirements — name the entities, roles/access, and the Given-When-Then scenario directly in the prompt.
3. Review what Build Agent creates. If you're curious what's happening under the hood, ask it to show you the Fluent (`.now.ts`) code it generated — this is optional and aimed at architects.
4. When asked if you want ATF tests written for your change, select "Yes, proceed" — your changes will get test coverage added.
5. Install the app to your sandbox:
   ```
   now-sdk install --sandbox
   ```
6. Manually walk through the golden path in your sandbox and confirm it matches your Given-When-Then scenario.
7. Ask Build Agent to generate an ATF test for an edge case, then run it.

## Success Criteria

- [ ] The entities from your requirements exist as tables in your sandbox
- [ ] Roles/access match what your team specified
- [ ] The golden path works end-to-end and matches your Given-When-Then scenario
- [ ] At least one ATF test passes

## Learning Points

- The SDK and Fluent turn a natural-language prompt into typed, diagnosable source — not a black box of clicks you can't inspect or diff.
- Writing an ATF test in the same session you build the feature keeps tests in the dev loop instead of a follow-up task nobody gets back to.

## Bonus Challenge

- Add Technician assignment/routing logic
- Add more ATF tests to cover edge cases or rejection paths
- Add role-based access restrictions and verify them as a non-privileged user
