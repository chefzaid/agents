# TODO

Sample backlog file to be copied into projects and adapted for the project-specific features. Then used with the current repo's agentic workflows.

## Bugs to fix

- [ ] Describe a bug found during testing, with steps to reproduce.
- [ ] Flaky checkout test: `checkout.spec.ts` fails about 1 in 5 runs on the payment step timeout. *(found by: Coding Agent)*

## Needs Input

Parts of tasks that cannot advance without the owner's decision or information. Only the part needing input is set aside here; the rest of the task keeps moving. Answer under the item, and the next agent run applies the answer and removes the entry.

- [ ] Which staging environment and test account should the end-to-end tests use? *(task: Big Feature › Deploy to staging; asked by: Infra Agent)*
  - Answer:

## Big Feature

- [x] Implement the new big feature with all its subcomponents and integrations.
- [ ] Write comprehensive unit and integration tests for the big feature. *(blocked: see Bugs to fix › flaky checkout test)*
- [ ] Deploy the big feature to staging and perform end-to-end testing.

## Another Feature

- [x] Implement another feature with its own set of requirements and dependencies.

## ...
