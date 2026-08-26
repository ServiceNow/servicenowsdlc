---
title: "Off-instance development: AI Skills for Fluent"
slide: "19 — Off-instance development"
---

# Off-instance development: AI Skills for Fluent

[← Back to slide 19 in the deck](https://servicenow.github.io/servicenowsdlc/#19) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/04-off-instance-development.ja.md)

## Objective

Use the ServiceNow SDK's AI Skills plugin with a coding agent (Claude Code, Cursor, Windsurf, etc.) to extend your maintenance app off-platform, and confirm the same `now-sdk build`/`now-sdk install` loop works outside Studio.

## Estimated Time

15–20 minutes

## Prerequisites

- Sandbox and Fluent project from the Build + test exercise
- A coding agent installed locally (Claude Code, Cursor, Windsurf, etc.)
- ServiceNow SDK AI Skills plugin installed for your coding agent — follow the setup instructions in the SDK README ([Claude Code instructions](https://github.com/ServiceNow/sdk#claude-code); the README also covers other agents)

## Exercise Steps

1. Open a terminal at the root of your Fluent project (the same one from the Build + test exercise) and start a session with your coding agent.
2. Prompt it to extend your app:
   ```
   Create a new ServiceNow table called "Todo List" that can be used to
   group records from the existing Maintenance Request table. Also, add
   a "Due Date" date/time column to the existing Maintenance Request
   table.
   ```
3. Confirm the agent created a new `.now.ts` file for the table (likely under `src/fluent/`) and updated the existing table's code to add the column.
4. Ask it to generate demo data so you can see the change in action:
   ```
   Add some demo Todo Lists and Todo records for them so I can see them
   in action.
   ```
5. Build and install:
   ```
   now-sdk build && now-sdk install
   ```
6. Open your sandbox and confirm the new table, column, and demo data are there.

## Success Criteria

- [ ] AI Skills plugin installed for your coding agent
- [ ] New Todo List table and Due Date column exist as Fluent source
- [ ] Demo data generated and visible in your sandbox
- [ ] `now-sdk build && now-sdk install` completed without errors

## Learning Points

- The AI Skills plugin gives your coding agent the same grounded, up-to-date Fluent/SDK knowledge Build Agent has on-instance — the workflow doesn't change when you move off-platform, only the tool does.
- `now-sdk build`/`now-sdk install` are the same commands whether Build Agent or a coding agent generated the code — the SDK doesn't care who (or what) wrote the source.

## Bonus Challenge

- Ask your coding agent what else it would add to this app
- Prompt it to add a dashboard that visualizes the lists and their todos
