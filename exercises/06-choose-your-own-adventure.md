---
title: "Choose your own adventure"
slide: "24 — Choose your own adventure (exercise)"
---

# Choose your own adventure

[← Back to slide 24 in the deck](https://apatti-now.github.io/servicenowsdlc/#24)

## Objective

Go one level deeper on whichever part of the stack is most relevant to your team — pick one track and spend the remaining time there.

## Estimated Time

30 minutes

## Prerequisites

- Maintenance app built, tested, source-controlled, and pushed through ReleaseOps (Exercises 01–05)

## Exercise Steps

Here are a bunch of ideas to keep exploring — choose anything below, or come up with your own adventure entirely. We're here to help with questions.

### Track: Fluent deep dive

- Add a metadata type the maintenance app didn't use yet:
  ```
  Add a scheduled job that runs nightly and auto-closes Maintenance
  Requests that have been in "Resolved" state for more than 7 days.
  ```
  ```
  Add a Scripted REST API that lets an external system create a
  Maintenance Request via POST.
  ```
- Transform an existing UI Builder page into Fluent source and see what comes out:
  ```
  now-sdk transform --sys-id <ui_builder_page_sys_id>
  ```

### Track: Testing deep dive

- Deliberately break a test and watch Test Agent triage it:
  ```
  Comment out the routing/assignment step in the Maintenance Request
  flow, then run the ATF test and triage the failure.
  ```
- Make a UI-facing change and let Test Agent add coverage for it:
  ```
  Add a "Priority" choice field (Low/Medium/High) to the Maintenance
  Request form, visible only to Technicians.
  ```
  When asked if you want ATF tests written for this change, select "Yes, proceed." Then open the generated test and look at the UI step it added for the new field.
- Remove a requirement and watch Test Agent clean up after itself:
  ```
  Remove the "Priority" field and its form logic from the Maintenance
  Request app.
  ```
  Watch Test Agent recognize the ATF test covering that field is no longer relevant and remove it — coverage stays in sync with the app in both directions, not just when you add things.

### Track: CI/CD APIs

- Script a Git-triggered pipeline using the CI/CD REST APIs (`app_repo/publish`, `app_repo/install`, `testsuite/run`, `app_repo/rollback`) instead of the ReleaseOps UI flow from Exercise 05.
- Add an approval gate so the pipeline pauses for manual sign-off before it calls `app_repo/install` against a production-bound target.

## Success Criteria

- [ ] Your table went hands-on with at least one track beyond what the core exercises covered
- [ ] You can point to something concrete (a command, a config, a passing/failing test) from that track

## Learning Points

- The core exercises cover the golden path end-to-end; each track here is where that golden path gets deeper in practice.

## Bonus Challenge

- Combine two tracks — e.g., wire the CI/CD approval gate from the CI/CD track so it only calls `app_repo/install` if the ATF tests from the Testing track pass via `testsuite/run`
