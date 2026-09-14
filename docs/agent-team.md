# Agent team for Mona's Project Pulse dashboard

The dashboard work will be coordinated through GitHub Copilot CLI in a GitHub Codespace, using a small custom team of specialist agents defined under `.github/agents/`.

## Custom agent team

- Orchestrator — Model: Claude Opus 4.7 (copilot)
  - Responsibility: breaks the project into phases, delegates work to specialists, and keeps the overall build aligned with the project requirements.
  - Definition: `.github/agents/orchestrator.agent.md`

- Planner — Model: Claude Opus 4.7 (copilot)
  - Responsibility: researches the repo, inspects relevant files and constraints, and produces a concrete implementation plan with dependencies, validation steps, and file assignments.
  - Definition: `.github/agents/planner.agent.md`

- Coder — Model: GPT-5.5 (copilot)
  - Responsibility: implements the application logic and code changes in the assigned file scopes, including runnable app support such as launch configuration when needed.
  - Definition: `.github/agents/coder.agent.md`

- Designer — Model: Gemini 3.1 Pro (copilot)
  - Responsibility: shapes the dashboard UX and visual design, with focus on usability, accessibility, information hierarchy, responsive layout, and polished project-card styling for Project Pulse.
  - Definition: `.github/agents/designer.agent.md`

## How the team works together

The Orchestrator acts as the lead coordinator, using the Planner to map the work, the Coder to implement the code, and the Designer to refine the visual experience. The full custom team lives in the repository's agent folder and is invoked through GitHub Copilot CLI in the Codespace environment to keep the build structured and collaborative.