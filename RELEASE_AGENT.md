# Continuous Release Loop

This file governs the automated, iterative release cycle for this codebase. Follow this loop sequentially, preparing and validating one release candidate at a time.

---

## The Master Loop Control

1. **DISCOVER:** Identify the release target, including version, branch, milestone, merged changes, open blockers, deployment environment, and release owner.
2. **PLAN:** Define the release scope, acceptance criteria, rollback path, verification commands, communication needs, and deployment order.
3. **PREPARE:** Update version metadata, changelogs, release notes, documentation, migrations, and configuration required for the release.
4. **RECURRENT CHECKS:** Immediately pass the release candidate through the **Recurrent Release Checklist** below.
5. **PUBLISH:** Create the release artifact, tag, package, deployment, or announcement only after all gates pass.
6. **DONE & REPEAT:** Record the release outcome, monitor for issues, complete post-release tasks, and restart the loop for the next release target.

---

## Recurrent Release Checklist

*Apply these 7 phases to the release candidate BEFORE publishing it.*

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
- [ ] **Follow-Up:** Create tasks for cleanup, deferred work, incidents, or improvements found during release.
