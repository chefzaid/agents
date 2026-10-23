# Agents

Reusable Markdown instructions for AI agents.

This repository keeps agent prompts and operating loops in plain text so they
can be applied to projects, versioned, reviewed, and improved over time.

## Available Agents

| File                                 | Purpose                                                    |
| ------------------------------------ | ---------------------------------------------------------- |
| [CODING_AGENT.md](CODING_AGENT.md)   | Continuous loop for coding tasks and quality gates.        |
| [TESTING_AGENT.md](TESTING_AGENT.md) | Continuous loop for UI testing and regression checks.      |
| [RELEASE_AGENT.md](RELEASE_AGENT.md) | Continuous loop for releases and deployment checks.        |
| [INFRA_AGENT.md](INFRA_AGENT.md)     | Continuous loop for infrastructure and deployment changes. |

## How To Use

1. Checkout this repo to the workspace where you have a project you want to implement.
2. Copy TODO.md to your project and adjust it with the features and requirements you want.
   Keep its **Bugs to fix** section, where agents log every bug or issue they find, and its **Needs Input** section, where they park only the parts of a task that wait on your decision. Answer under a **Needs Input** entry and the next agent run picks it up.
3. Instruct your AI agent to apply the workflow you are interested in against another project in your workspace, for example: *"Follow `agents/CODING_AGENT.md` on `my-app` until no eligible task remains."*
4. Review the agent's commits and the **Bugs to fix** and **Needs Input** sections between runs.

## Authoring Guidelines

When adding or updating an agent file:

- Start with a clear title that describes the agent's loop or responsibility.
- Define the agent's goal before listing detailed checks.
- Keep steps sequential and unambiguous.
- Separate the main loop from recurring validation checklists.
- Prefer checklists for repeatable quality gates.
- Avoid project-specific tool names unless the agent is meant for one stack.
- Include completion criteria so the agent knows when to stop or repeat.

## License

This repository is released under the [MIT License](LICENSE).
