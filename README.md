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

## How to call an agent
Agents must first be installed (see Install below): in the project's `.github/agents/` or in `~/.copilot/agents/`.

**1. Copilot CLI, one-shot.** Run in your project folder:
```
copilot --agent product-owner --prompt "Write a PRD for <feature>"
copilot --agent rca-analyst --prompt "Investigate why @tests/login.test.ts fails intermittently"
```

**2. Copilot CLI, interactive.** Start `copilot`, then:
```
/agent                 # list agents and pick one (e.g. backend-engineer)
```
After picking, every prompt goes to that agent. Use `/agent` again to switch.
Skills: `/skills reload` after adding files, `/skills info rca-method` to check.

**3. By name in a prompt.** Without switching, ask for it directly:
```
Use the qa-engineer agent to write a test plan for @docs/specs/login.md
```
Copilot can also delegate on its own when your task matches an agent's `description`.

**4. VS Code.** Open Copilot Chat, open the agent dropdown at the bottom of the chat box, pick the agent (e.g. `frontend-engineer`), then type your prompt.

Tip: chain them. Run `product-owner`, then `system-analyst` on the PRD it saved in `docs/product/`, and so on (see Workflow).

Prompt template: Context (`@files`) + Goal + Constraints + Acceptance, e.g.
```
@docs/specs/login.md
CONTEXT: login API spec is approved.
GOAL: implement the login endpoint.
CONSTRAINTS: follow the spec exactly, add tests.
ACCEPTANCE: build and tests pass.
```

## Install
Pick one option.

**A. Into a project (shared with the team via git).** From your project root:
```
git clone https://github.com/fikar46/copilot-agent-team.git /tmp/copilot-agent-team
mkdir -p .github/agents .github/skills
cp /tmp/copilot-agent-team/.github/agents/*.agent.md .github/agents/
cp -r /tmp/copilot-agent-team/.github/skills/* .github/skills/
git add .github && git commit -m "Add Copilot agents"
```
Copy only the agents you need (e.g. `cp .../agents/qa-engineer.agent.md .github/agents/`). Keep `rca-method` if you use `rca-analyst`.

**B. Personal (all your projects, not in git).**
```
mkdir -p ~/.copilot/agents ~/.copilot/skills
cp /tmp/copilot-agent-team/.github/agents/*.agent.md ~/.copilot/agents/
cp -r /tmp/copilot-agent-team/.github/skills/* ~/.copilot/skills/
```

**Verify.** Open `copilot` in the project, run `/agent` to see the list, and `/skills reload` then `/skills info rca-method`. In VS Code, reopen Chat and check the agent dropdown.

## More
How agents, skills, and instructions work in Copilot: [docs/SPIKE.md](docs/SPIKE.md).
