---
title: "Off-instance development: AI Skills for Fluent"
slide: "21 — Off-instance development"
---

# Off-instance development: AI Skills for Fluent

[← Back to slide 21 in the deck](https://servicenow.github.io/servicenowsdlc/#21) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/04-off-instance-development.ja.md)

## Objective

Use the ServiceNow SDK's AI Skills plugin with a coding agent (Claude Code, Cursor, Windsurf, etc.) to change existing behavior in your maintenance app off-platform — not just add something new — and confirm the same `now-sdk build`/`now-sdk install` loop, and the same broken-test auto-heal behavior, works outside Studio.

## Estimated Time

15–20 minutes

## Prerequisites

- Sandbox and Fluent project from the Build + test exercise
- A coding agent installed locally (Claude Code, Cursor, Windsurf, etc.)
- ServiceNow SDK AI Skills plugin installed for your coding agent — follow the setup instructions in the SDK README (the [Claude Code section](https://github.com/ServiceNow/sdk#claude-code) covers the plugin install steps that apply to any coding agent, not just Claude Code; the README also has agent-specific sections if you're using Cursor, Windsurf, etc.)

## Exercise Steps

1. Open a terminal at the root of your Fluent project (the same one from the Build + test exercise) and start a session with your coding agent.
2. Prompt it to change existing behavior, not add something new:
   ```
   Change default status to be draft on new maintenance request.
   ```
3. Confirm the agent finds the current default ("New") on the Maintenance Request business rule, updates it to "Draft," and updates the rule's comment to match.
4. This is a deliberate breaking change: it invalidates any existing ATF test that asserts the default status is "New" (the positive-case test from Exercise 03, and possibly an ACL/state-enforcement test that checks it too). Ask your coding agent to check for and fix any ATF tests that now fail because of this change:
   ```
   Does this change break any existing ATF tests? If so, fix them.
   ```
   Confirm it updates the failing test's assertion and comment to expect "Draft" instead of "New" — the same auto-heal behavior Test Agent does on-instance, just off-platform.
5. Build and install:
   ```
   now-sdk build && now-sdk install
   ```
6. Open your sandbox, confirm new Maintenance Requests default to "Draft," and re-run the affected ATF test(s) in Studio to confirm they pass again.

## Success Criteria

- [ ] AI Skills plugin installed for your coding agent
- [ ] New Maintenance Requests default to "Draft" status
- [ ] The ATF test(s) that asserted the old "New" default are updated and passing again
- [ ] `now-sdk build && now-sdk install` completed without errors

## Learning Points

- The AI Skills plugin gives your coding agent the same grounded, up-to-date Fluent/SDK knowledge Build Agent has on-instance — the workflow doesn't change when you move off-platform, only the tool does.
- `now-sdk build`/`now-sdk install` are the same commands whether Build Agent or a coding agent generated the code — the SDK doesn't care who (or what) wrote the source.
- The auto-heal loop isn't a Build Agent/Test Agent-only trick — a coding agent with the same grounded knowledge can detect a test it just broke and fix it, off-platform, the same way.

## Bonus Challenge

- Extend the schema too — add a new table and a column to the existing one:
  ```
  Create a new ServiceNow table called "Todo List" that can be used to
  group records from the existing Maintenance Request table. Also, add
  a "Due Date" date/time column to the existing Maintenance Request
  table.
  ```
  Then ask it to generate demo data so you can see the change in action:
  ```
  Add some demo Todo Lists and Todo records for them so I can see them
  in action.
  ```
- Ask your coding agent what else it would add to this app
- Prompt it to add a dashboard that visualizes the lists and their todos
