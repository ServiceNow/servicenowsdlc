---
title: "ReleaseOps: assess and release"
slide: "22 — ReleaseOps"
---

# ReleaseOps: assess and release

[← Back to slide 22 in the deck](https://servicenow.github.io/servicenowsdlc/#22) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/05-releaseops.ja.md)

## Objective

Take the app you published in Exercise 04 through the ReleaseOps quality-control pipeline — create a Deployment Request, watch the assessment run, and get it to Ready for Deploy. **You will not push to production in this exercise** — that part is a live demo right after, not something you do yourselves.

## Estimated Time

30 minutes

## Prerequisites

- Pre-release published to the Application Repository (Exercise 04)
- Access to a test instance separate from your dev instance

## Exercise Steps

1. On your dev instance, promote your Update Set into a new Deployment Request.
2. Mark the Deployment Request "Ready to Assess."
3. Watch the assessment playbook run automatically: Instance Scan (on your dev instance) → Move to Test → Run ATF.
4. If ATF fails, pick one of the three reconciliation options and understand what each actually does:
   - **Retest** — rerun the assessment as-is.
   - **Need Code Change** — invalidates the assessment; the Deployment Request returns to Draft and needs a new payload before it can be re-assessed.
   - **Sign Off** — manual approval to proceed despite the failure.
5. Once the Deployment Request reaches "Ready for Deploy," **stop there.** That's the finish line for this exercise — you do not need to create a Release record or push anything to production yourselves.
6. Actually releasing to production (On Demand vs. Scheduled) is covered right after this as a live demo — see the next slide. Watch, don't replicate it on your own instance.

## Success Criteria

- [ ] Deployment Request created and moved through Instance Scan, Move to Test, and Run ATF
- [ ] You can explain the difference between Retest, Need Code Change, and Sign Off
- [ ] Deployment Request reached "Ready for Deploy" — this is where the exercise ends, no release/push-to-prod required

## Learning Points

- A Deployment Request is the payload container the assessment runs against; a Release is the separate record that actually moves a cleared Deployment Request to production.
- "Need Code Change" isn't a soft warning — it invalidates the whole assessment and sends you back to Draft, so it's worth understanding before you hit it.
- Rollback only works when the app was installed from the Application Repository, and only within a fixed time window — another reason the App Repo, not a manual install, is the supported path to production.

## Bonus Challenge

- Deliberately fail an ATF test before assessment and walk the "Need Code Change" path — rebuild the payload and re-assess
- Compare On Demand vs. Scheduled release and discuss when each fits your team's cadence
