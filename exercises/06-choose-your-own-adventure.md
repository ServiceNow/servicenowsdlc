---
title: "Choose your own adventure"
slide: "25 — Choose your own adventure (exercise)"
---

# Choose your own adventure

[← Back to slide 25 in the deck](https://apatti-now.github.io/servicenowsdlc/#25)

## Objective

Go one level deeper on whichever part of the stack is most relevant to your team — pick one track and spend the remaining time there.

## Estimated Time

30 minutes

## Prerequisites

- Maintenance app built, tested, source-controlled, and pushed through ReleaseOps (Exercises 01–05)

## Exercise Steps

1. As a table, pick one track below. Each has a couple of concrete things to try — you don't need to do all of them, just go deep on what's interesting to your team.
2. Be ready to show the group what you found.

### Track: Fluent deep dive

Try one or both:

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

Try one or both:

- Deliberately break a test and watch Test Agent triage it:
  ```
  Comment out the routing/assignment step in the Maintenance Request
  flow, then run the ATF test and triage the failure.
  ```
- Explore running the same test browserless, outside Studio:
  ```
  Show me how to run this app's ATF tests from Cloud Runner instead of
  Studio, and what's different about the triage loop there.
  ```

### Track: CI/CD APIs

Try one or both:

- Script a Git-triggered pipeline using the CI/CD REST APIs (`app_repo/publish`, `app_repo/install`, `testsuite/run`, `app_repo/rollback`) instead of the ReleaseOps UI flow from Exercise 05.
- Add an approval gate so the pipeline pauses for manual sign-off before it calls `app_repo/install` against a production-bound target.

## Success Criteria

- [ ] Your table went hands-on with at least one track beyond what the core exercises covered
- [ ] You can point to something concrete (a command, a config, a passing/failing test) from that track

## Learning Points

- The core exercises cover the golden path end-to-end; each track here is where that golden path gets deeper in practice.

## Bonus Challenge

- Combine two tracks — e.g., wire the CI/CD approval gate from the CI/CD track so it only calls `app_repo/install` if the ATF tests from the Testing track pass via `testsuite/run`
