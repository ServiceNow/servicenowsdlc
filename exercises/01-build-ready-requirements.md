---
title: "Build-ready requirements: rewrite the vague story"
slide: "9 — Activity"
time: "10:10–10:15"
type: "activity"
---

# Build-ready requirements: rewrite the vague story

**Time:** 10:10–10:15 (5 min)
**Type:** Activity

## Objective

Practice turning a vague user story into a build-ready one — with entities, roles/access, and Given-When-Then acceptance criteria — before writing any code.

## Prerequisites

- None. Pen/paper or a shared doc is enough.

## Instructions

1. As a team, start from this vague story:

   > "As a facilities user, I want to request maintenance on equipment so it gets fixed."

2. In 5 minutes, rewrite it as build-ready by identifying:
   - **Entities** — what tables/records does this touch?
   - **Roles & access** — who can create, view, and act on each entity?
   - **Given–When–Then** — at least one concrete acceptance scenario.
3. Write down your build-ready version.
4. Be ready to present it back to the group — we'll compare a few teams' answers before moving on.

## Success criteria

- [ ] At least two entities identified (e.g. Maintenance Request, Equipment)
- [ ] At least two roles defined, each with what they can create/view/update
- [ ] One complete Given-When-Then scenario for the primary (happy) path

## Stretch goals (optional)

- Add a second scenario for a rejection or edge case (e.g. equipment already under maintenance)
- Specify a routing rule for how requests get assigned to a Technician

## Facilitator notes (optional)

- Give the customer the full 5 minutes before revealing the reference answer — resist rescuing early.
- Goal is completing the *core* requirements first; extensions are optional if a team finishes early.
- Reference build-ready version lives on slide 8 (Build-ready requirements) for comparison after the activity.
