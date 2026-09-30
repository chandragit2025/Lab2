# Agent team

The Mona's Project Pulse dashboard will be built by a coordinated team of four custom agents, orchestrated through GitHub Copilot CLI in a Codespace:

- **Orchestrator** — Uses **Claude Opus 4.7 (copilot)** to break the work into phases, delegate tasks, manage file ownership and dependencies, and verify the integrated result. Definition: `.github/agents/orchestrator.agent.md`.
- **Planner** — Uses **Claude Opus 4.7 (copilot)** to research the repository, dependencies, documentation, risks, edge cases, and validation needs, then produce an implementation plan. Definition: `.github/agents/planner.agent.md`.
- **Designer** — Uses **Gemini 3.1 Pro (copilot)** to shape the dashboard's UI/UX, information hierarchy, accessibility, interaction flow, responsive behavior, and polished Project Pulse visual design. Definition: `.github/agents/designer.agent.md`.
- **Coder** — Uses **GPT-5.5 (copilot)** to implement the assigned application code and runnable-app support, following repository patterns with explicit, deterministic, testable behavior and validating the changes. Definition: `.github/agents/coder.agent.md`.

The Orchestrator runs the Planner first, then coordinates the Designer and Coder in parallel where their file scopes are independent, or sequentially when work has dependencies or overlapping ownership. Each specialist reports its decisions, changes, and validation results back to the Orchestrator; no agent stages, commits, or pushes changes.
