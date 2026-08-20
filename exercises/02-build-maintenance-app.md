---
title: "Build the maintenance app"
slide: "11 — Build"
time: "10:30–11:30"
type: "exercise"
---

# Build the maintenance app

**Time:** 10:30–11:30 (60 min)
**Type:** Exercise

## Objective

Use Build Agent (and ATF) to go from a natural-language prompt to a working maintenance-request app, using the build-ready requirements your team wrote earlier.

## Prerequisites

- Sandbox allocated and feature branch created (Setup, 10:15–10:30)
- Build-ready requirements from the previous activity (entities, roles/access, Given-When-Then)

## Instructions

1. Open Build Agent in Studio (or your IDE if working off-instance).
2. Prompt Build Agent with your build-ready requirements — name the entities, roles/access, and the Given-When-Then scenario directly in the prompt.
3. Review what Build Agent creates. If you're curious what's happening under the hood, ask it to show you the Fluent (`.now.ts`) code it generated — this is optional and aimed at architects.
4. Install the app to your sandbox:
   ```
   now-sdk install --sandbox
   ```
5. Manually walk through the golden path in your sandbox and confirm it matches your Given-When-Then scenario.
6. Ask Build Agent to generate an ATF test for that golden-path scenario, then run it.

## Success criteria

- [ ] The entities from your requirements exist as tables in your sandbox
- [ ] Roles/access match what your team specified
- [ ] The golden path works end-to-end and matches your Given-When-Then scenario
- [ ] At least one ATF test passes

## Stretch goals (optional)

- Add Technician assignment/routing logic
- Add a second ATF test covering an edge case or rejection path
- Add role-based access restrictions and verify them as a non-privileged user

## Facilitator notes (optional)

- Table floaters throughout — this is the longest block of the day, circulate continuously rather than waiting to be flagged down.
- If a team gets stuck on the prompt, suggest they paste their build-ready requirements from Exercise 01 verbatim rather than re-summarizing.
- The "show the Fluent code" moment is a good place for a 5-minute architect sidebar without slowing down the rest of the room.
