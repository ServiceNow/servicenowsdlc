---
title: "Automated testing with Test Agent"
slide: "15 — Automated testing"
time: "1:15–1:45"
type: "exercise"
---

# Automated testing with Test Agent

**Time:** 1:15–1:45 (30 min)
**Type:** Exercise

## Objective

Practice the principle that tests live in app scope, authored in the dev loop — not bolted on afterward — by generating, running, and triaging ATF tests with Test Agent.

## Prerequisites

- Maintenance app built (Exercise 02) and installed to your sandbox

## Instructions

1. Open Test Agent inside Build Agent / Studio.
2. Describe a test scenario in natural language (e.g. "a Requester submits a request for active equipment and it lands in New state").
3. Let Test Agent generate an ATF test from that description.
4. Run the test suite.
5. If a test fails, use Test Agent to triage the failure and apply a fix, then rerun until it passes.

> **Honest constraint:** Test Agent execution and triage run from the Studio surface. ATF Cloud Runner enables browserless execution, but doesn't carry that same triage loop yet.

## Success criteria

- [ ] At least two ATF tests passing (happy path + one edge case)
- [ ] One deliberately introduced failure triaged and fixed using Test Agent

## Stretch goals (optional)

- Add a regression test for an adjacent scenario (e.g. Technician updating status)
- Try running your suite via ATF Cloud Runner and compare the experience to the Studio surface

## Facilitator notes (optional)

- Encourage teams to break something on purpose (e.g. remove a required field) so they get real triage practice rather than only writing passing tests.
- If a team's app from Exercise 02 has gaps, this is a good moment to patch it — the tests will surface those gaps naturally.
