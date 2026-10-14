# Continuous Release Loop

This file governs the automated, iterative release cycle for this codebase. Follow this loop sequentially, preparing and validating one release candidate at a time.

---

## The Master Loop Control

1. **DISCOVER:** Identify the release target, including version, branch, milestone, merged changes, open blockers, deployment environment, and release owner. Check `TODO.md` for open entries in **Bugs to fix** that affect the release scope and for answered **Needs Input** entries about this release. If there are no unreleased changes, stop the loop.
2. **PLAN:** Define the release scope, acceptance criteria, rollback path, verification commands, communication needs, and deployment order.
3. **PREPARE:** Update version metadata, changelogs, release notes, documentation, migrations, and configuration required for the release. Record any decision only the owner can make in **Needs Input** following the **TODO Logging Rules** below, and keep preparing everything else.
4. **RECURRENT CHECKS:** Immediately pass the release candidate through the **Recurrent Release Checklist** below.
5. **PUBLISH:** Create the release artifact, tag, package, deployment, or announcement only after all gates pass and no **Needs Input** entry for this release is unanswered. If a gate fails, do not publish: log the failure in **Bugs to fix**, record the go/no-go decision in **Needs Input**, and stop for human review.
6. **DONE & REPEAT:** Run Phase 7 of the checklist. If it finds a regression, execute the Rollback Plan, log the regression in **Bugs to fix**, and stop for human review; otherwise restart the loop for the next release target.

---

## TODO Logging Rules

- **Bugs to fix:** Every failing check, regression, broken build, packaging or migration problem, or defect found while preparing or validating the release goes in **Bugs to fix** with the version, the environment, the failing command or check, and its output. Skip issues already listed. Do not patch application code from the release loop; the coding loop fixes it.
- **Needs Input:** Decisions only the owner can make go in **Needs Input** with the precise question and the release they block: version number or bump level, scope changes, wording of user-facing notes, accepting a known issue, missing credentials or approvals, release timing, and go/no-go after a failed gate. Set aside only that decision and finish every other preparation step; publishing waits until it is answered.
- **Answers:** When an entry in **Needs Input** has an answer, apply it to the release on the next loop, then remove the entry.

---

## Recurrent Release Checklist

*Apply Phases 1-6 to the release candidate BEFORE publishing it, and Phase 7 after publishing.*

### Phase 1: Scope & Readiness

- [ ] **Release Scope:** Confirm included changes match the milestone, ticket list, or release plan.
- [ ] **Blockers:** Check for open critical bugs, failing checks, unresolved review threads, or incomplete release tasks.
- [ ] **Compatibility:** Verify breaking changes are intentional, documented, and versioned appropriately.
- [ ] **Ownership:** Confirm an owner is assigned for release execution, validation, and rollback decisions.

### Phase 2: Versioning & Changelog

- [ ] **Version Bump:** Update package, app, API, schema, or infrastructure versions according to project policy.
- [ ] **Changelog:** Summarize user-facing changes, fixes, breaking changes, migrations, and known issues.
- [ ] **Release Notes:** Prepare concise release notes for the intended audience.
- [ ] **Tags:** Confirm release tags follow the repository's naming convention.

### Phase 3: Quality Gates

- [ ] **Test Suites:** Run required unit, integration, end-to-end, and regression tests.
- [ ] **Static Checks:** Run linting, formatting, type checks, builds, and generated asset checks.
- [ ] **Security Checks:** Run dependency audit and any required security scans.
- [ ] **Manual Verification:** Complete release-specific smoke tests and critical path validation.

### Phase 4: Data, Migrations & Configuration

- [ ] **Migrations:** Verify database or data migrations are reversible, ordered, tested, and documented.
- [ ] **Configuration:** Confirm environment variables, feature flags, secrets, and service settings are ready.
- [ ] **Backups:** Ensure backup or restore expectations are clear for stateful systems.
- [ ] **Data Compatibility:** Check old and new application versions can safely operate during rollout if required.

### Phase 5: Packaging & Deployment

- [ ] **Build Artifact:** Confirm artifacts are reproducible, signed or checksummed where applicable, and stored in the expected location.
- [ ] **Deployment Plan:** Document deployment commands, order, target environments, and expected duration.
- [ ] **Rollback Plan:** Define rollback commands, data considerations, and decision criteria.
- [ ] **Access:** Confirm required credentials, permissions, and approval gates are available.

### Phase 6: Communication & Documentation

- [ ] **Documentation:** Update setup guides, API docs, migration notes, screenshots, or user guides affected by the release.
- [ ] **Stakeholder Notice:** Prepare internal or external communication for changes, downtime, or action required.
- [ ] **Known Issues:** Document limitations, deferred fixes, and monitoring areas.
- [ ] **Support Readiness:** Ensure support, operations, or maintainers know how to recognize and respond to release issues.

### Phase 7: Post-Release Validation

- [ ] **Smoke Test:** Verify the deployed release works in the target environment.
- [ ] **Monitoring:** Watch logs, metrics, errors, alerts, and user reports for regressions.
- [ ] **Release Record:** Save links to tags, artifacts, deployment runs, release notes, and verification evidence.
- [ ] **Follow-Up:** Create tasks in `TODO.md` for cleanup, deferred work, incidents, or improvements found during release, with defects in **Bugs to fix**.
