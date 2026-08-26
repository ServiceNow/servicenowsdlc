---
title: "ReleaseOps: assess and release"
slide: "23 — ReleaseOps"
---

# ReleaseOps: assess and release

[← Back to slide 23 in the deck](https://servicenow.github.io/servicenowsdlc/#23) | [日本語版](https://github.com/ServiceNow/servicenowsdlc/blob/main/exercises/06-releaseops.ja.md)

## Objective

Take the app you published in Exercise 05 through the ReleaseOps quality-control pipeline — create a Deployment Request, watch the assessment run, and get it to Ready for Deploy. **You will not push to production in this exercise** — that part is a live demo right after, not something you do yourselves.

## Estimated Time

30 minutes

## Prerequisites

- Pre-release published to the Application Repository (Exercise 05)
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

## Appendix: Full Walkthrough (Screenshots)

Reference material, not something every attendee needs to replicate step-by-step — this is the same walkthrough covered conceptually in the [ReleaseOps Deep Dive](https://servicenow.github.io/servicenowsdlc/releaseops-deep-dive.html), and it also covers what happens in the "Push to production" demo right after this exercise (creating and activating a Release, and watching the release flow fire). Useful if you're doing the CYOA ReleaseOps track, prepping to present this material yourself, or just want to see every click.

### 1. Build and install the app from the IDE

Build the app in your IDE — same Fluent SDK workflow as the earlier build exercise — then `now-sdk build` followed by `now-sdk install` to get it onto your dev instance. Studio can't see an app that isn't installed yet, so this has to come first. No screenshot for this step; it precedes where the recording below starts.

### 2. Publish the app to the App Repository

![Explorer panel in ServiceNow Studio with the app open](images/releaseops/01-open-app-in-studio.png)
*Open the app in ServiceNow Studio via the Explorer panel.*

![Publish to app repository dialog with a new version number](images/releaseops/02-click-app-details-and-publish.png)
*App Details → Publish. Bump the version — Studio auto-increments it for you.*

![Publish successful confirmation](images/releaseops/03-publish-successful.png)
*Publish successful — the app is now visible to every other instance in your company.*

### 3. Find the Update Set publish created

![Searching for Local Update Sets on the dev instance](images/releaseops/04-navigate-local-update-sets.png)
*Search Local Update Sets on the dev instance.*

![List view showing the update set the publish action created](images/releaseops/05-view-published-update-set.png)
*The publish action created this update set automatically — "Maintenance install version 1.0.1…".*

![Opened update set record, state Complete](images/releaseops/06-open-update-set.png)
*Open it — this is the payload your Deployment Request will actually move.*

> **Gotcha:** every scoped app also keeps a default "[app] in progress" Update Set open alongside this one — that's standard behavior, not an error. Use the specific maintenance/install version set, not the default.

### 4. Create the Release record

![Navigating to the Releases module under ReleaseOps](images/releaseops/07-navigate-to-releases.png)
*On the destination instance: search Releases (ReleaseOps app).*

![Blank new Release record form](images/releaseops/08-create-new-release-record.png)
*New Release record — blank, state Draft.*

![Picking the destination environment for the release](images/releaseops/09-select-destination-environment.png)
*Destination environment: the instance ReleaseOps should ultimately deploy to (here, prod).*

![Choosing between Sample On Demand Pipeline and Sample Release Pipeline](images/releaseops/10-select-sample-release-pipeline.png)
*Pipeline: Sample Release Pipeline (waits for the release date) vs. Sample On Demand Pipeline (pushes as soon as the DR is ready).*

![Freeze date and release date fields set an hour apart](images/releaseops/11-select-freeze-and-release-dates.png)
*Freeze date and release date — set close together here for a live demo; normally days or weeks apart.*

![Saved release record](images/releaseops/12-final-release-record.png)
*Saved. Assignment group set to App Engine Admins; state stays Draft until activated.*

### 5. Promote the Update Set → create a Deployment Request

![Promote Update Set button on the update set record](images/releaseops/13-promote-update-set.png)
*Back on dev: open the update set, click Promote Update Set.*

![Deploy an Update Set record producer opened on the destination instance](images/releaseops/14-deploy-update-set-record-producer.png)
*Opens a record producer on the destination instance — "Deploy an Update Set."*

![Record producer form with create new deployment request checked](images/releaseops/15-fill-deployment-request-info.png)
*Check "Create new deployment request," link the Release you just made, optionally restrict which ATF suites run.*

### 6. Activate the Release

![Notice that the assigned release needs to be active before the deployment request can be set to ready to assess](images/releaseops/16-release-needs-to-be-active.png)
*The Deployment Request won't move to Ready to Assess until its Release is Active.*

![Release record with the Activate Release button](images/releaseops/17-navigate-activate-release.png)
*Back on the Release record: Activate Release.*

![Release record showing State Active](images/releaseops/18-release-now-active.png)
*State: Active. ReleaseOps has scheduled the freeze-date job behind the scenes.*

### 7. Ready to Assess → watch it move

![Deployment Request record with the Ready to Assess button](images/releaseops/19-dr-ready-to-assess.png)
*On the Deployment Request: click Ready to Assess — this is what actually triggers the assessment playbook.*

![Deployment Request state flips to Assessing](images/releaseops/20-status-now-assessing.png)
*State flips to Assessing automatically — no separate approval step.*

![Activity log narrating instance scan completion and the move to the test instance](images/releaseops/21-automatic-record-movement.png)
*The activity log narrates it live: instance scan completed with 0 findings, then "Started moving updates to…"*

#### Inside Workflow Studio: find the playbook

![Search results for Studio, highlighting Workflow Studio](images/releaseops/22-navigate-workflow-studio.png)
*Search "Studio" → Workflow Studio.*

![Playbooks list filtered to the ReleaseOps application](images/releaseops/23-find-playbook.png)
*Filter Playbooks by the ReleaseOps application — four ship out of the box, including Deployment Request Assessment.*

![Workflow Studio canvas showing the Run Instance Scan stage](images/releaseops/24-canvas-overview.png)
*Open it. Four numbered stages, left to right: Run Instance Scan → Move To Test → Run ATF Tests → Ready for Deploy.*

#### The four stages, close up

![Workflow Studio diagram of the Run Instance Scan stage](images/releaseops/25-stage1-instance-scan.png)
*Stage 1 — Instance Scan: a static policy check on the dev instance. Happy path continues to Move to Test; Failures or Errors branches to a Reconciling task instead.*

![Workflow Studio diagram of the Move To Test stage](images/releaseops/26-stage2-move-to-test.png)
![Integrate Deployment Request activity properties, with Destination Instance Label set to Test](images/releaseops/27-stage2-config-panel.png)
*Stage 2 — Move to Test: "Test" is a label, not a hard-coded instance — the destination is a configurable input, here literally named "Test" but just as easily Staging or UAT.*

![Workflow Studio diagram of the Run ATF Tests stage](images/releaseops/28-stage3-run-atf.png)
*Stage 3 — Run ATF Tests: two independent sad paths, Failures or Errors and Low Code Coverage — either routes to a Reconciling task instead of blocking silently.*

![Workflow Studio diagram of the Ready for Deploy stage](images/releaseops/29-stage4-ready-for-deploy.png)
*Stage 4 — Ready for Deploy: the finish line for the assessment flow. Getting to prod from here is the release flow's job, on its own schedule.*

### 8. Setting up a manual pause point

![Deployment Request Tasks related list, empty](images/releaseops/30-dr-tasks-tab-empty.png)
*Deployment Request → Deployment Request Tasks. Empty until someone adds one.*

![New Deployment Request Task form with the Type dropdown open](images/releaseops/31-create-task-type-options.png)
*New task — Type can be Runbook, Test issue, Scan issue, or Preview issue.*

![Deployment Request Task detail wired to a playbook stage and activity](images/releaseops/32-task-detail.png)
*This task is wired to Playbook stage "Ready for Deploy," waiting on activity "Before Ready for Deployment" — the playbook won't proceed past that point until the task closes.*

![Deployment Request Task with the Resolve button](images/releaseops/33-resolve-task.png)
*Resolve clears it, and the playbook picks back up — same mechanism whether the sign-off is "ready to deploy?" or a post-deploy smoke test.*
