---
title: "Automated testing with Test Agent"
slide: "15 — Automated testing"
---

# Automated testing with Test Agent

[← Back to slide 15 in the deck](https://apatti-now.github.io/servicenowsdlc/#15)

## Objective

Practice the principle that tests live in app scope, authored in the dev loop — not bolted on afterward — by generating, running, and triaging ATF tests with Test Agent.

## Estimated Time

30 minutes (1:15–1:45)

## Prerequisites

- Maintenance app built (Exercise 02) and installed to your sandbox

## Exercise Steps

1. Open Test Agent inside Build Agent / Studio.
2. Describe a test scenario in natural language (e.g. "a Requester submits a request for active equipment and it lands in New state").
3. Let Test Agent generate an ATF test from that description.
4. Run the test suite.
5. If a test fails, use Test Agent to triage the failure and apply a fix, then rerun until it passes.

> **Honest constraint:** Test Agent execution and triage run from the Studio surface. ATF Cloud Runner enables browserless execution, but doesn't carry that same triage loop yet.

## Success Criteria

- [ ] At least two ATF tests passing (happy path + one edge case)
- [ ] One deliberately introduced failure triaged and fixed using Test Agent

## Learning Points

- A failing test is a starting point for triage, not a dead end — Test Agent closes the loop from failure to fix without leaving the Studio surface.
- Cloud Runner buys browserless execution, not the triage experience — know which tool solves which problem.

## Bonus Challenge

- Add a regression test for an adjacent scenario (e.g. Technician updating status)
- Try running your suite via ATF Cloud Runner and compare the experience to the Studio surface
