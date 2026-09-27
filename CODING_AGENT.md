# Continuous Execution Loop

This file governs the automated, iterative coding cycle for this codebase. Follow this loop sequentially, processing one task at a time.

---

## The Master Loop Control

1. **DISCOVER:** Scan the file `TODO.md` and pick an open item from **Bugs to fix** first; otherwise identify the **simplest, lowest-complexity task** that is not yet implemented. If no eligible item remains, stop the loop.
2. **PLAN:** Write a concise implementation plan for the task at hand, including acceptance criteria, files to modify, tests to update, and any other relevant details.
3. **IMPLEMENT:** Write the clean, minimal code required to satisfy the task requirements.
4. **RECURRENT CHECKS:** Immediately pass the new implementation through the **Recurrent Tasks Checklist** below.
5. **DONE:** Mark the task as done in `TODO.md` once all related items are completed. If a check cannot pass, mark the task as blocked with the reason instead and revert or set aside its unfinished changes.
6. **COMMIT & REPEAT:** Create a clean Git commit for the task, including the `TODO.md` update, with a very brief message without putting any co-author. Then restart the loop at Step 1.

---

## Recurrent Tasks Checklist
*Apply these 8 phases to the newly implemented task code BEFORE marking it complete.*

### Phase 1: Implementation & UI Implementation
- [ ] **Code Implementation:** Ensure that the feature is fully implemented, no dead code remains, and all edge cases are handled.
- [ ] **UI Implementation:** Ensure that the feature has its corresponding UI implemented, if applicable, and that it is fully functional, visually consistent with the rest of the application, and free of visual glitches, misaligned elements, or broken interactions.
- [ ] **Full Implementation:** Ensure that there are no mocked services or hardcoded values, and that it is fully localized for all supported languages.

### Phase 2: Code Decomposition & Architecture
- [ ] **File Length Check:** Verify that no modified or newly created file exceeds 500 lines of code (LOC). If it does, split it into modular, single-responsibility components or services.
- [ ] **Readability:** Refactor overly nested logic or confusing conditional blocks to ensure long-term maintainability.
- [ ] **Design Patterns:** Ensure proper use of design patterns and architectural principles for scalability and maintainability.
- [ ] **Tech Stack Adherence:** Confirm that the implementation follows the project's tech stack conventions, including language features, framework usage, and coding standards.
- [ ] **Industry Standards:** Verify that the code adheres to industry best practices, including SOLID principles, KISS, and YAGNI.

### Phase 3: Deduplication & Reusability
- [ ] **DRY Check:** Scan existing utilities, components, and service layers.
- [ ] **Centralization:** Ensure no redundant logic was added. Reuse existing methods or global components instead of spinning up identical custom code.

### Phase 4: Static Analysis & Quality Gates
- [ ] **Linting & Formatting:** Run the project's linter and formatter scripts over the changed files. Fix all structural style warnings automatically.
- [ ] **Workspace Health:** Inspect the compiler/IDE "Problems" tab outputs and resolve 100% of errors, warnings, or type mismatches.

### Phase 5: Security Hardening (OWASP Top 10 Guard)
- [ ] **Input & Injection Shielding:** Ensure all user inputs, parameters, and query bindings related to the new feature are fully sanitized or strictly parameterized.
- [ ] **XSS/CSRF Cross-Check:** Confirm proper data escaping/encoding on the frontend and token validation checks on state-changing backend endpoints.
- [ ] **Access Control:** Verify that any new data entry points or API routes enforce proper authorization checks.
- [ ] **Secrets & Keys:** Ensure no sensitive information (API keys, passwords, tokens) is hardcoded or exposed in the codebase.
- [ ] **Dependency Audit:** If any new external packages were added, run a vulnerability scan and ensure they are up-to-date.
- [ ] **Security Testing:** Run automated security tests (OWASP, ...) to detect vulnerabilities in the new code.

### Phase 6: Performance Optimization & Bug Hunting
- [ ] **Memory Safety:** Audit loops, async calls, or hooks for unclosed streams, missing cleanup listeners, or potential memory leaks.
- [ ] **Null/Undefined Guards:** Add defensive optional chaining or explicit null checks to prevent runtime pointer exceptions.
- [ ] **Efficiency Audit:** Optimize any heavy loop operations, multithreading, or network queries.

### Phase 7: Test Coverage Gate
- [ ] **Run Test Suites:** Run the entire test suite across frontend and backend environments.
- [ ] **Coverage Enforcement:** Verify code coverage remains strictly **above 85%** for both layers. If the new code drops the average or lacks testing, author unit/integration specs until the 85%+ target is met.
- [ ] **Critical Path Testing:** Add Playwright tests to ensure that the new functionality works as expected in end-to-end scenarios.
- [ ] **Regression Check:** Confirm that no existing features are broken by the new implementation.

### Phase 8: Documentation & Dependency Sync
- [ ] **Dependency Hygiene:** Make sure that any dependency added or changed by the task uses a current LTS version and that no unused packages remain.
- [ ] **Docs Update:** Update documentation files and schemas to perfectly reflect the new functionality and its setup.
