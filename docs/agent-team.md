# Agent team

Mona's Project Pulse dashboard will be built by a four-agent team with clear ownership and file boundaries:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (Copilot) | Coordinates the team from GitHub Copilot CLI, turns the plan into dependency-aware phases, assigns non-overlapping file scopes, and checks that the integrated dashboard works as a whole. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (Copilot) | Researches the repository and relevant documentation, identifies requirements and edge cases, and produces an ordered implementation plan with file assignments, dependencies, and validation expectations. It plans but does not write code. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (Copilot) | Implements the dashboard logic and runnable application support within its assigned files, using explicit errors and deterministic, testable behavior. For Project Pulse, this can include the Codespace launch configuration. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (Copilot) | Owns the dashboard's UI/UX, accessibility, information hierarchy, responsive layout, and visual polish, including project cards, status badges, priority treatments, and consistent styling. | `.github/agents/designer.agent.md` |

I am using **GitHub Copilot CLI in a Codespace** to direct the Orchestrator, delegate work to the specialist agents, and coordinate the plan, implementation, and design of Project Pulse.
