# Continuous Infrastructure & Deployment Execution Loop

This file governs the automated, iterative delivery cycle for infrastructure, platform engineering, DevOps, and deployment changes in this repository. Follow the loop sequentially and process one task at a time.

The repository is the source of truth. Prefer declarative, idempotent, reviewable automation. Never make an undocumented production-only change.

---

## Operating Principles

- **Declarative first:** Prefer Kubernetes YAML, Helm/Kustomize, Terraform/OpenTofu, and Ansible over imperative commands.
- **One canonical implementation:** Do not maintain equivalent logic independently in several formats. Kubernetes resources define workload state; Ansible defines host and cluster provisioning; shell scripts are thin wrappers for bootstrap, validation, or break-glass recovery.
- **Parity where required:** If a supported operation must work through both Ansible and shell, both paths must consume the same variables/templates and produce the same result. Test both paths.
- **Idempotency:** Re-running automation must converge safely without duplicating resources, resetting data, rotating secrets unexpectedly, or causing avoidable downtime.
- **Secure by default:** Deny by default, grant least privilege, pin trusted artifacts, keep secrets outside Git, and expose only required ports and services.
- **Progressive delivery:** Validate locally, then in an ephemeral/test environment, then staging, and finally production with explicit approval when required.
- **Reversibility:** Every risky change must have a documented, tested rollback or recovery procedure before deployment.
- **No silent drift:** Detect and reconcile divergence between Git and live infrastructure. Emergency changes must be back-ported to Git immediately.

---

## Master Loop Control

1. **DISCOVER:** Scan `TODO.md` and identify the simplest, lowest-risk infrastructure task that is not implemented and has no unresolved dependency. Do not select production-destructive work merely because it is short.
2. **ASSESS:** Classify the change by environment, blast radius, data risk, downtime risk, security impact, external dependencies, and required approval. Record assumptions and blockers.
3. **PLAN:** Write a concise implementation plan containing:
   - acceptance criteria and affected environments;
   - files, manifests, roles, scripts, pipelines, and documentation to modify;
   - compatibility, migration, backup, rollout, rollback, and recovery steps;
   - validation commands, automated tests, observability signals, and success/failure thresholds;
   - secrets, RBAC, networking, data, and supply-chain implications.
4. **IMPLEMENT:** Make the smallest complete change using existing modules, roles, charts, overlays, and conventions. Keep environment-specific values outside shared base definitions.
5. **VALIDATE:** Run every applicable phase in the **Infrastructure Recurrent Tasks Checklist** below. Record skipped checks with a concrete reason; never report an unavailable check as passed.
6. **REVIEW DIFF & PLAN:** Inspect the complete Git diff and generated deployment plan/rendered manifests. Reject unexpected deletion, replacement, privilege expansion, public exposure, mutable image tags, secret material, or unrelated changes.
7. **DEPLOY SAFELY:** Apply through the approved CI/CD or GitOps path. Use canary, rolling, or staged rollout where possible. Production deployment requires the repository's normal approval and change-control policy.
8. **VERIFY & OBSERVE:** Confirm readiness, health, logs, metrics, alerts, events, SLOs, and user-facing behavior for the defined observation window. Verify that backups and restore paths remain valid.
9. **ROLL BACK ON FAILURE:** Stop the rollout when thresholds fail. Execute the documented rollback; preserve logs and evidence; open a follow-up task for root cause. Do not continue forward merely to complete the loop.
10. **COMMIT:** Create one clean Git commit for the completed task with a short imperative message and no co-author trailer. Never commit secrets, generated credentials, kubeconfigs, state files, or unreviewed artifacts.
11. **DONE & REPEAT:** Mark the task complete in `TODO.md` only after acceptance criteria, checks, deployment verification, documentation, and rollback readiness are satisfied. Then restart at Step 1.

---

## Infrastructure Recurrent Tasks Checklist

Apply all relevant phases to the current task before marking it complete.

### Phase 1: Scope, Risk & Change Safety

- [ ] **Environment Scope:** Identify every affected cluster, account, region, namespace, node group, host, network, and dependent service.
- [ ] **Risk Classification:** Record blast radius, downtime, security, cost, state/data, and vendor/API risks.
- [ ] **Change Window:** Confirm maintenance-window and approval requirements for disruptive or production changes.
- [ ] **Preconditions:** Verify required capacity, quotas, versions, credentials, certificates, DNS, storage, backups, and external dependencies.
- [ ] **Backup & Restore:** Create or verify a recent backup/snapshot for stateful changes and confirm the restore procedure and retention.
- [ ] **Rollback:** Define precise rollback triggers, commands or Git revision, ordering, ownership, and expected recovery time.

### Phase 2: Infrastructure as Code Architecture

- [ ] **Source of Truth:** Ensure the desired state is represented in version-controlled IaC; no undocumented manual configuration is required.
- [ ] **Separation of Concerns:** Keep reusable bases/modules/roles separate from environment-specific inventories, overlays, and values.
- [ ] **Reuse:** Search existing roles, modules, charts, templates, tasks, and utilities before adding new logic.
- [ ] **Idempotency:** Confirm repeated execution converges without unintended restart, recreation, credential rotation, or data loss.
- [ ] **Dependency Ordering:** Encode readiness and dependencies without fragile fixed sleeps.
- [ ] **Maintainability:** Split oversized files and roles into cohesive units; use meaningful names, comments for non-obvious constraints, and a documented variable contract.
- [ ] **Version Pinning:** Pin providers, modules, collections, charts, actions, container images, and downloaded tools to reviewed versions or digests. Define a deliberate update process instead of silently using `latest`.

### Phase 3: Kubernetes, Helm & YAML Quality

- [ ] **Schema Validation:** Parse YAML and validate rendered resources against the target Kubernetes API versions and CRD schemas.
- [ ] **Render First:** Render Helm/Kustomize output and inspect the final manifests, not only templates or values.
- [ ] **API Compatibility:** Detect deprecated/removed APIs and verify compatibility with the oldest and newest supported cluster versions.
- [ ] **Workload Identity:** Use dedicated ServiceAccounts; disable token automount unless required; bind the minimum RBAC verbs and resources.
- [ ] **Pod Security:** Set non-root execution, immutable UID/GID where practical, read-only root filesystem, dropped Linux capabilities, `seccompProfile: RuntimeDefault`, and `allowPrivilegeEscalation: false`. Justify every exception.
- [ ] **Resource Safety:** Define realistic requests and limits, probes, termination grace periods, disruption budgets, and topology/anti-affinity rules appropriate to the workload.
- [ ] **Network Isolation:** Apply default-deny NetworkPolicies and explicitly allow required ingress and egress. Avoid public `LoadBalancer`, `NodePort`, host networking, and wildcard ingress unless justified.
- [ ] **Configuration & Secrets:** Keep non-secret configuration in ConfigMaps/values and secrets in an approved external secret manager. Do not store plaintext, base64-only secrets, private keys, or kubeconfigs in Git.
- [ ] **Image Security:** Use trusted registries, immutable digests for production, a non-root image, minimal base layers, and verified signatures/SBOMs where supported.
- [ ] **Stateful Safety:** Validate StorageClass, access mode, reclaim policy, capacity, replica placement, backup, restore, expansion, and upgrade behavior.
- [ ] **Availability:** Verify rolling-update strategy, readiness gates, PodDisruptionBudgets, replica count, failure-domain distribution, and graceful shutdown.
- [ ] **Policy Gates:** Run repository-standard checks such as `kubeconform`/`kubeval`, `helm lint`, `kustomize build`, `kubectl --dry-run=server`, `kube-linter`, `conftest`, or Kyverno policy tests as applicable.

### Phase 4: Ansible & Shell Automation Parity

- [ ] **Canonical Ownership:** Kubernetes YAML owns workloads; Ansible owns repeatable node/cluster configuration; shell is limited to bootstrap, local validation, and recovery unless the task explicitly requires otherwise.
- [ ] **Ansible Quality:** Use fully qualified collection names, handlers, modules instead of raw commands, protected sensitive output, tags, check mode where supported, and lint-clean roles/playbooks.
- [ ] **Ansible Idempotency:** Run the playbook twice in a disposable environment and confirm the second run reports no unexpected changes.
- [ ] **Shell Safety:** Use `#!/usr/bin/env bash`, `set -Eeuo pipefail`, quoted expansions, validated inputs, explicit prerequisites, safe temporary directories, cleanup traps, and actionable errors.
- [ ] **No Unsafe Fetch-and-Execute:** Do not pipe remote content directly into a privileged shell. Download from an allowlisted source, pin a version, verify checksum/signature, then execute.
- [ ] **Parity:** When both Ansible and shell are supported, ensure they consume shared defaults, implement equivalent security controls, and pass the same post-deployment assertions.
- [ ] **Static Checks:** Run `ansible-lint`, `ansible-playbook --syntax-check`, inventory validation, `shellcheck`, and `shfmt` where applicable.

### Phase 5: Core Linux Host Hardening

- [ ] **Supported Baseline:** Use a supported OS/kernel and apply tested security updates. Reboot safely when required and verify services afterward.
- [ ] **Accounts & Privilege:** Disable direct root SSH login and password authentication where feasible; require key-based access; restrict sudo; remove stale users, groups, keys, and passwordless privilege.
- [ ] **SSH:** Enforce modern cryptography, low authentication retries, sensible idle/session limits, logging, and rate limiting. Keep a tested recovery path before reloading SSH.
- [ ] **Firewall:** Use a default-deny host firewall and allow only documented management, ingress, control-plane, and node-to-node traffic from required source ranges.
- [ ] **Kernel & Network:** Apply reviewed `sysctl` hardening for forwarding, redirects, source routing, reverse-path filtering, unprivileged features, and kernel information exposure without breaking the CNI/runtime.
- [ ] **Filesystem:** Use secure mount options where compatible; protect bootloader and sensitive paths; enforce correct ownership/permissions; prevent world-writable sensitive files.
- [ ] **Services:** Disable or remove unused services, listeners, packages, kernel modules, and legacy protocols. Confirm required ports with `ss`/equivalent.
- [ ] **Runtime:** Harden containerd/Docker/K3s/Kubernetes configuration, protect sockets and kubeconfigs, restrict registry mirrors, enable audit logging where supported, and avoid the Docker socket in workloads.
- [ ] **MAC & Auditing:** Keep AppArmor or SELinux enforcing where supported; configure auditd/journald retention, time synchronization, and protected remote log forwarding.
- [ ] **Brute-Force Protection:** Configure and test Fail2ban/CrowdSec or the approved equivalent without creating conflicting firewall managers.
- [ ] **Compliance:** Run the applicable CIS distribution/Kubernetes benchmark and Lynis or the repository-approved host scanner; document justified exceptions.

### Phase 6: Secrets, Identity & Supply-Chain Security

- [ ] **Secret Scan:** Scan the full diff and relevant history with the repository-approved tool; revoke and rotate any exposed credential immediately.
- [ ] **Secret Lifecycle:** Source secrets from Vault, External Secrets, SOPS, or the approved manager; use least-privilege authentication, scoped paths, short lifetimes, rotation, and audit logging.
- [ ] **Encryption:** Enforce TLS in transit, validate certificates, encrypt sensitive data at rest, and protect encryption keys separately from encrypted data.
- [ ] **Authorization:** Review cloud IAM, Kubernetes RBAC, sudoers, CI identities, OIDC claims, and service-to-service permissions for least privilege and separation of duties.
- [ ] **Artifact Integrity:** Verify checksums/signatures for downloaded binaries, images, charts, packages, modules, and release assets.
- [ ] **Dependency Scans:** Scan IaC, images, OS packages, language dependencies, and SBOMs. Resolve critical/high findings or record an approved exception with an expiry date.
- [ ] **CI/CD Security:** Pin third-party CI actions, restrict token permissions, protect environments, isolate runners, prevent secret exposure in logs, and require review for production paths.

### Phase 7: Static Analysis, Tests & Policy Gates

- [ ] **Formatting & Linting:** Run all repository formatters and linters on changed IaC, YAML, Ansible, shell, Dockerfiles, and pipeline definitions.
- [ ] **Unit/Module Tests:** Run module, role, chart, policy, and script tests relevant to the change.
- [ ] **Ephemeral Integration:** Provision or use a disposable environment and exercise install, upgrade, repeat run, failure, and teardown paths.
- [ ] **Security Tests:** Run IaC and image scanners such as Checkov/tfsec/Trivy, secret scanning, policy-as-code tests, and repository-standard security suites.
- [ ] **Plan/Diff Gate:** Review Terraform/OpenTofu plan, Helm diff, Argo CD diff, or equivalent. Explicitly approve replacements, deletions, RBAC expansion, firewall changes, public endpoints, and state migrations.
- [ ] **Negative Tests:** Confirm unauthorized access, disallowed network paths, invalid inputs, and missing secrets fail safely.
- [ ] **No False Passes:** Tests must fail on errors; do not mask exit codes, use unconditional `|| true`, or treat warnings as success without an approved exception.

### Phase 8: Deployment, Rollout & Runtime Verification

- [ ] **Preflight:** Confirm context/account/cluster/namespace, Git revision, change ticket, backup, capacity, maintenance window, and operator access before applying.
- [ ] **Progressive Rollout:** Use a canary, small batch, or non-production environment first. Define pause and abort thresholds.
- [ ] **Readiness:** Wait on explicit resource conditions with bounded timeouts; inspect Kubernetes events and rollout status.
- [ ] **Functional Smoke Test:** Verify critical endpoints, DNS, TLS, authentication, authorization, persistence, queues, jobs, ingress/egress, and service dependencies.
- [ ] **Observability:** Confirm dashboards, alerts, log ingestion, metrics, traces, audit events, and notification routes cover the new behavior.
- [ ] **SLO Guard:** Compare error rate, latency, saturation, restarts, pending pods, node pressure, and business/availability signals against the pre-change baseline.
- [ ] **Failure Drill:** For high-risk changes, exercise rollback or restore in non-production and capture timings and gaps.
- [ ] **Post-Deploy Observation:** Observe for the planned period before declaring success; do not equate an accepted API request with a healthy deployment.

### Phase 9: Drift, Resilience & Disaster Recovery

- [ ] **Drift Detection:** Run the approved drift/diff mechanism and ensure live state matches Git after deployment.
- [ ] **GitOps Health:** Verify reconciliation, sync, health, retry behavior, and that manual changes are reverted or deliberately adopted into Git.
- [ ] **Resilience:** Validate behavior under node/service loss, rescheduling, dependency timeout, network interruption, and storage degradation as appropriate.
- [ ] **Recovery Evidence:** Record the last successful backup, restore test, recovery point objective, and recovery time objective for affected stateful systems.
- [ ] **Cleanup:** Remove temporary permissions, debug endpoints, test resources, orphaned volumes, stale feature flags, obsolete manifests, and expired exceptions.

### Phase 10: Documentation & Handoff

- [ ] **Runbooks:** Update install, upgrade, rollback, restore, troubleshooting, certificate/secret rotation, and incident procedures.
- [ ] **Architecture:** Update diagrams, ports, DNS, namespaces, data flows, trust boundaries, dependencies, and ownership where changed.
- [ ] **Configuration Reference:** Document variables, defaults, allowed values, required secrets by name/path only, and environment overrides.
- [ ] **Operational Handoff:** Document dashboards, alerts, log queries, SLOs, known limitations, maintenance tasks, and escalation ownership.
- [ ] **Evidence:** Attach or reference validation results, plans/diffs, scanner summaries, deployment revision, and rollback verification without exposing secrets.
- [ ] **TODO Sync:** Mark only fully satisfied work complete and create explicit follow-up tasks for deferred risks or approved exceptions.

---

## Mandatory Stop Conditions

Stop the loop and request human review when any of the following occurs:

- the target account, cluster, namespace, host, or environment is ambiguous;
- a plan includes unexpected resource deletion/replacement, state loss, or irreversible migration;
- backup or rollback cannot be verified for a stateful or high-risk change;
- a secret is found in Git, logs, rendered manifests, plans, or command output;
- the change expands public exposure, cluster-admin/root access, wildcard IAM/RBAC, privileged containers, host mounts, or firewall access without explicit approval;
- validation, security scanning, policy checks, health checks, or SLO thresholds fail;
- live drift or concurrent changes make the reviewed plan stale;
- production access, approval, or maintenance-window requirements are missing.

---

## Definition of Done

An infrastructure task is done only when:

- desired state is committed as minimal, declarative, idempotent automation;
- Kubernetes, Ansible, and shell responsibilities are clear and supported paths remain consistent;
- relevant lint, schema, policy, security, integration, idempotency, and dry-run/plan checks pass;
- secrets, identity, network, host, container, and supply-chain controls are reviewed;
- rollout succeeds and runtime health is observed against defined thresholds;
- backup, rollback, and recovery procedures are documented and tested proportionate to risk;
- documentation and `TODO.md` are updated; and
- no unresolved critical/high finding or unexplained drift remains.

If production deployment is intentionally outside the task, completion must explicitly stop at a reviewed, reproducible deployment artifact and state who or what performs the remaining rollout.
