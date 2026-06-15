# Agents

Reusable Markdown instructions for AI agents.

This repository keeps agent prompts and operating loops in plain text so they
can be applied to projects, versioned, reviewed, and improved over time.

## Available Agents

| File                                   | Purpose                                             |
| -------------------------------------- | --------------------------------------------------- |
| [CODING_AGENT.md](CODING_AGENT.md)     | Continuous loop for coding tasks and quality gates. |
| [RELEASE_AGENT.md](RELEASE_AGENT.md)   | Continuous loop for releases and deployment checks. |
| [TESTING_AGENT.md](TESTING_AGENT.md)   | Continuous loop for UI testing and regression checks. |

## How To Use

1. Choose the agent file that matches the work you want automated.
2. Copy the file contents into the AI tool, project instructions, or agent
   configuration where the instructions should run.
3. Adjust project-specific references such as `TODO.md`, test commands,
   documentation paths, or coverage requirements.
4. Commit local customizations in the target project if they are intended to
   become part of that project's workflow.

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
