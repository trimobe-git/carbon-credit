---
name: paperclip
description: >
  Use this skill whenever the user mentions Paperclip, zero-human companies, AI agent orchestration,
  multi-agent companies, autonomous businesses, agent hiring, org charts for AI agents, heartbeat systems,
  company templates, Clipmart, agent budgets, or wants to plan/implement/discuss using Paperclip
  (paperclip.ing) for business automation. Also trigger when the user discusses mapping business processes
  to AI agent teams, creating autonomous workflows, or integrating Paperclip with the TRIMOBE CarbonBroker project.
  TRIGGER on: "paperclip", "zero-human", "agent company", "AI employees", "heartbeat", "org chart agents",
  "company template", "clipmart", "npx paperclipai", "autonomous company", "agent orchestration platform".
---

# Paperclip — Open-Source Orchestration for Zero-Human Companies

Paperclip is a Node.js server + React UI that orchestrates teams of AI agents as structured companies.
It provides org charts, budgets, heartbeats, task management, and governance — treating agents as employees,
not isolated tools.

- **Website**: https://paperclip.ing
- **GitHub**: https://github.com/paperclipai/paperclip (52k+ stars, MIT license)
- **Docs**: https://docs.paperclip.ing
- **Created by**: @dotta, launched March 4, 2026

## When to Use This Skill

- Planning or implementing Paperclip-based automation
- Designing agent org charts and company structures
- Configuring agents, heartbeats, budgets, and skills
- Choosing or customizing company templates
- Integrating Paperclip with TRIMOBE CarbonBroker's "3 machines"
- Comparing Paperclip with other orchestration approaches (n8n, LangGraph, etc.)

## Core Concepts (Quick Reference)

| Concept | Description |
|---------|-------------|
| **Company** | Isolated organizational unit with its own agents, goals, budgets, and data |
| **Agent** | An AI "employee" with a role, adapter, budget, and place in the org chart |
| **Adapter** | Runtime connector (Claude Local, Codex, Gemini, HTTP, Process/Bash) |
| **Heartbeat** | Scheduled wake cycle where an agent checks work, executes tasks, and reports |
| **Goal** | North star objective that all tasks trace back to (company mission) |
| **Issue/Task** | Unit of work — ticketed, threaded, with immutable audit logs |
| **Skill** | Runtime-injected capability an agent can use without retraining |
| **Org Chart** | Hierarchical structure with reporting lines (CEO → Directors → Specialists) |
| **Budget** | Monthly spending cap per agent; soft alert at 80%, hard stop at 100% |
| **Board** | Human oversight layer — approves hires, reviews strategy, can pause/terminate agents |
| **Clipmart** | Marketplace for pre-built company templates (coming soon) |

## Standard Workflow: Creating a Paperclip Company

1. **Install & Onboard**: `npx paperclipai onboard --yes`
2. **Create Company**: Name, description, mission statement
3. **Define North Star Goals**: Specific, measurable objectives (e.g., "$50k MRR", "100 leads/month")
4. **Hire CEO Agent**: First agent — reviews company health, sets strategy, delegates work
5. **Build Org Chart**: Add department heads (CTO, CMO, COO) reporting to CEO
6. **Hire Specialist Agents**: Engineers, marketers, researchers under department heads
7. **Configure Budgets**: Monthly caps per agent and company-wide
8. **Set Up Heartbeats**: Schedule wake cycles (e.g., every 30min, hourly, daily)
9. **Activate & Monitor**: Enable heartbeats, watch dashboard, iterate

## Agent Configuration Pattern

Each agent needs:

```
Agent Definition:
├── Name & Role (e.g., "content-lead", role: "cmo")
├── Adapter (e.g., claude-local with model: claude-sonnet-4-6)
├── Prompt Template (instructions with {{variables}})
├── Monthly Budget (in cents)
├── Reports To (parent agent ID)
└── Configuration Files:
    ├── ROLE.md    — Deterministic boundaries (tech stack, APIs, schemas)
    ├── SOUL.md    — Heuristic behavior (tone, constraints, philosophy)
    ├── HEARTBEAT.md — Execution checklist for every wake cycle
    └── TOOLS.md   — Available tools and capabilities
```

### Prompt Template Variables
- `{{agentId}}`, `{{companyId}}`, `{{runId}}`
- `{{agent.name}}`, `{{company.name}}`

## The 9-Step Heartbeat Cycle

Every heartbeat, an agent executes this sequence:

1. **Identity** — `GET /api/agents/me` (role, chain of command, budget)
2. **Approvals** — Check for pending approval events
3. **Fetch Tasks** — Get assigned tasks (todo, in_progress, blocked)
4. **Select Work** — Priority: in-progress first, then new tasks
5. **Checkout** — `POST` checkout with agent ID (409 = another agent owns it)
6. **Context** — Read full issue history, parent chain, all comments
7. **Execute** — Do the work using available tools
8. **Update** — PATCH status + post markdown comment with progress
9. **Delegate** — Create subtasks if needed, assign to team members

## Essential CLI Commands

```bash
# Onboarding & Setup
npx paperclipai onboard --yes              # Interactive setup
npx paperclipai onboard --yes --bind lan   # LAN mode
npx paperclipai onboard --yes --bind tailnet  # Tailscale mode

# Development (from cloned repo)
pnpm install && pnpm dev                   # Full dev with watch
pnpm dev:server                            # Server only
pnpm build                                 # Build all packages
pnpm typecheck                             # Type checking
pnpm test:run                              # Run tests
pnpm db:generate                           # Create DB migration
pnpm db:migrate                            # Apply migrations

# Company Templates
npx paperclipai company import --from ./path-to-template
npx skills install paperclipai/companies/skills/company-creator
```

## Key Design Principles

1. **Agent Agnostic** — Works with Claude, Codex, Gemini, Bash, HTTP, or custom agents
2. **Human-in-the-Loop** — Board approves hires, reviews strategy, can override/pause
3. **Atomic Execution** — Task checkout + budget enforcement prevent double-work
4. **Persistent State** — Agents resume context across heartbeats (no cold starts)
5. **Goal Ancestry** — Every task carries full context chain back to company mission
6. **Runtime Skill Injection** — Agents learn new workflows without retraining
7. **Full Observability** — Every tool call, API request, decision logged with tracing

## Reference Files Index

Read these files as needed for deeper context:

| File | When to Read |
|------|-------------|
| `references/architecture.md` | When discussing system design, control plane vs execution, or technical architecture |
| `references/getting-started.md` | When helping with installation, setup, configuration, or first deployment |
| `references/agents-and-org.md` | When designing agent roles, org charts, ROLE.md/SOUL.md files, or delegation patterns |
| `references/heartbeats-tasks-budget.md` | When configuring heartbeats, managing budgets, understanding task lifecycle |
| `references/company-templates.md` | When choosing or creating company templates, importing pre-built companies |
| `references/adapters.md` | When configuring specific adapters (Claude Local, Codex, HTTP, etc.) |
| `references/api-reference.md` | When working with the Paperclip API, building integrations, or debugging |
| `references/carbonbroker-integration.md` | When planning how to use Paperclip for TRIMOBE CarbonBroker automation |

## TRIMOBE CarbonBroker Integration Overview

The CarbonBroker project has 3 automation "machines" that map naturally to Paperclip companies:

| CarbonBroker Machine | Paperclip Company | Key Agents |
|----------------------|-------------------|------------|
| **Curation & Content** | content-company | Curator, Newsletter Writer, LinkedIn Creator, Editor |
| **Continuous Prospecting** | prospecting-company | Research Agent, Outreach Writer, CRM Updater, Dedup Agent |
| **Post-Call Intelligence** | intelligence-company | Transcriber, Analyst, CRM Updater, Follow-up Drafter |

> For full integration strategy, read `references/carbonbroker-integration.md`

## What Paperclip Is NOT

- **Not a chatbot** — No conversation interface; agents work through tickets
- **Not an agent framework** — Doesn't dictate how agents are built
- **Not a workflow builder** — No drag-and-drop pipelines
- **Not a prompt manager** — Agents bring their own prompts/models
- **Not single-agent** — Designed for teams with hierarchies and delegation

## Ecosystem Repositories

| Repo | Purpose |
|------|---------|
| `paperclipai/paperclip` | Main platform (Node.js + React) |
| `paperclipai/companies` | 16 pre-built company templates (440+ agents, 500+ skills) |
| `paperclipai/clipmart` | Marketplace for buying/selling AI-agent companies |
| `paperclipai/companies-tool` | CLI tool for company management |
| `paperclipai/docs` | Documentation source |
| `paperclipai/pr-reviewer` | PR triage UI |

## Best Practices

1. **Start small** — Begin with 2-3 agents, add more as workflows stabilize
2. **Budget conservatively** — Set tight budgets initially, increase based on value delivered
3. **Use templates** — Import existing company templates before building from scratch
4. **Monitor heartbeats** — Watch the dashboard for stuck/failing agents early
5. **Define clear goals** — Vague goals produce vague work; be specific and measurable
6. **Separate concerns** — Each agent should have one clear domain of responsibility
7. **Model matching** — Use Opus for CEO/strategy, Sonnet for specialist work (cost-effective)
8. **Test incrementally** — Activate one department at a time, validate before expanding
