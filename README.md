# Copilot Agent Team

A set of custom GitHub Copilot agents covering a full product team, from idea to release, operations, and incident analysis. Each agent has a focused role, its own tools, and a clear handoff.

## Agents
| Agent | Role | Tools | Use when |
|---|---|---|---|
| `product-owner` | PO | read, search, edit (docs) | Turn an idea into PRD and user stories |
| `system-analyst` | SA | read, search, edit (docs) | Write functional spec, API contract, data model |
| `ui-ux-designer` | UI/UX | read, search, edit (docs) | Flows, screens, states, accessibility |
| `frontend-engineer` | FE | read, search, edit, execute | Implement UI from spec and design |
| `backend-engineer` | BE | read, search, edit, execute | Implement APIs, logic, DB from spec |
| `qa-engineer` | QA | read, search, edit (tests), execute | Test plan, automated tests, bug reports |
| `expert-engineer` | XE | read, search, edit, execute | Architecture, hard problems, trade-offs, ADRs |
| `tech-ops` | Tech Ops | read, search, edit, execute | CI/CD, deployment, infra, monitoring, runbooks |
| `code-reviewer` | Reviewer | read, search (read-only) | Review a diff before merge |
| `rca-analyst` | RCA | read, search, execute | Find the root cause of a bug or incident |

Skill: `rca-method` (`.github/skills/rca-method/SKILL.md`) gives `rca-analyst` its method and report template.

## Workflow
```
product-owner -> system-analyst + ui-ux-designer -> frontend/backend-engineer
              -> code-reviewer -> qa-engineer -> tech-ops (release)
incident: tech-ops (mitigate) -> rca-analyst (root cause)
expert-engineer supports every step on architecture and hard problems.
```

## Usage
CLI (inside your project):
```
copilot --agent product-owner --prompt "Write a PRD for <feature>"
copilot --agent rca-analyst --prompt "Investigate why @tests/login.test.ts fails intermittently"
```
Or use `/agent` in the CLI, or the agent selector in VS Code Chat.

Prompt template: Context (`@files`) + Goal + Constraints + Acceptance.

## Install
Copy `.github/agents/` and `.github/skills/` into your repo's `.github/` folder, or into `~/.copilot/agents` and `~/.copilot/skills` for personal use. Pick only the agents you need.

## More
How agents, skills, and instructions work in Copilot: [docs/SPIKE.md](docs/SPIKE.md).
