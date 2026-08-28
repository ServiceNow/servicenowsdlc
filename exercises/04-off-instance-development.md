---
title: "Off-instance development: AI Skills for Fluent"
slide: "21 — Off-instance development"
---

# Off-instance development: AI Skills for Fluent

[← Back to slide 21 in the deck](https://servicenow.github.io/servicenowsdlc/#21) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/04-off-instance-development.ja.md)

## Objective

Use the ServiceNow SDK's AI Skills plugin with a coding agent (Claude Code, Cursor, Windsurf, etc.) to extend your maintenance app off-platform, and confirm the same `now-sdk build`/`now-sdk install` loop works outside Studio.

## Estimated Time

15–20 minutes

## Prerequisites

- Sandbox and Fluent project from the Build + test exercise
- A coding agent installed locally (Claude Code, Cursor, Windsurf, etc.)
- ServiceNow SDK AI Skills plugin installed for your coding agent — follow the setup instructions in the SDK README (the [Claude Code section](https://github.com/ServiceNow/sdk#claude-code) covers the plugin install steps that apply to any coding agent, not just Claude Code; the README also has agent-specific sections if you're using Cursor, Windsurf, etc.)

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
5. Prompt it to make a business-logic change too, not just a data-model one:
   ```
   Change default status to be draft on new maintenance request.
   ```
6. Build and install:
   ```
   now-sdk build && now-sdk install
   ```
7. Open your sandbox and confirm the new table, column, demo data, and default-status change are there.

## Success Criteria

- [ ] AI Skills plugin installed for your coding agent
- [ ] New Todo List table and Due Date column exist as Fluent source
- [ ] Demo data generated and visible in your sandbox
- [ ] New Maintenance Requests default to "Draft" status
- [ ] `now-sdk build && now-sdk install` completed without errors

## Learning Points

- The AI Skills plugin gives your coding agent the same grounded, up-to-date Fluent/SDK knowledge Build Agent has on-instance — the workflow doesn't change when you move off-platform, only the tool does.
- `now-sdk build`/`now-sdk install` are the same commands whether Build Agent or a coding agent generated the code — the SDK doesn't care who (or what) wrote the source.

## Bonus Challenge

- Ask your coding agent what else it would add to this app
- Prompt it to add a dashboard that visualizes the lists and their todos
