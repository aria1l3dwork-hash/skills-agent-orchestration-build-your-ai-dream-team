    # Agent team

For Mona's Project Pulse dashboard, I am using a small custom multi-agent team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 5.5 (copilot). Responsibility: researches the repo, checks dependencies and edge cases, and produces the implementation plan and execution phases. Definition: `.github/agents/planner.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implements the assigned code changes, keeps the work within the scoped files, and validates behavior before reporting back. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibility: focuses on UI/UX, accessibility, information hierarchy, and polished dashboard styling for the Project Pulse experience. Definition: `.github/agents/designer.agent.md`.
- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibility: coordinates the Planner, Coder, and Designer, breaks the work into phases, delegates scoped tasks, and verifies the integrated result. Definition: `.github/agents/orchestrator.agent.md`.

This setup gives Mona's dashboard work a clear split between planning, implementation, design, and coordination while using GitHub Copilot CLI in the Codespace as the control layer for orchestrating the entire build.
