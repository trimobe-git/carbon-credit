# Agents & Organizational Structure

## Agent Anatomy

Every Paperclip agent consists of:

| Component | Description |
|-----------|-------------|
| **Name** | Unique identifier (e.g., "content-lead", "dev-1") |
| **Role** | Organizational function (e.g., "ceo", "cto", "engineer", "marketer") |
| **Adapter** | Runtime connector (claude-local, codex-local, http, process) |
| **Model** | AI model variant (e.g., claude-opus-4-6, claude-sonnet-4-6) |
| **Prompt Template** | Instructions with variable substitution |
| **Monthly Budget** | Spending cap in cents |
| **Reports To** | Parent agent in the org chart |
| **Skills** | Runtime-injected capabilities |

## Configuration Files (Per Agent)

### ROLE.md — Deterministic Boundaries
Defines what the agent CAN and CANNOT do:
- Technology stack constraints
- API access permissions
- Output format schemas (JSON, markdown, etc.)
- Domain-specific rules and guardrails

Example:
```markdown
# Content Lead — Role Definition

## Stack
- Newsletter: Resend + React Email
- Social: Buffer API
- CRM: Notion API
- AI: Claude API (Sonnet for generation)

## Output Formats
- Newsletter drafts: HTML email template
- LinkedIn posts: Plain text, max 1300 chars
- Reports: Markdown with structured sections

## Constraints
- All content in Brazilian Portuguese (pt-BR)
- Never publish without human approval
- Follow brand voice guide in company knowledge base
```

### SOUL.md — Heuristic Behavior
Defines the agent's personality and decision-making philosophy:
- Communication tone and style
- How to handle uncertainty
- Error recovery philosophy
- Prioritization framework

Example:
```markdown
# Content Lead — Soul

## Personality
- Professional but approachable
- Data-driven: cite sources and metrics
- Proactive: suggest improvements, don't just execute

## Decision Framework
- When uncertain, ask the manager (CMO) via comment
- Prefer quality over quantity
- When budget is tight (>80%), focus on newsletter only (highest ROI)

## Error Handling
- Log errors clearly with context
- Propose fix in the same comment
- Never silently fail — always report status
```

### HEARTBEAT.md — Execution Checklist
Step-by-step instructions for every heartbeat cycle:
```markdown
# Content Lead — Heartbeat Checklist

1. Check identity and budget status
2. Review any pending approvals
3. Fetch assigned tasks (todo, in_progress)
4. For in-progress tasks: continue work, update status
5. For new tasks: read full context, begin execution
6. Post progress comment with what was done
7. If blocked: set status to blocked, explain why
8. If work needs delegation: create subtask, assign to team member
9. Check if any scheduled content is due for publication
```

### TOOLS.md — Available Capabilities
Lists tools the agent can access:
```markdown
# Content Lead — Tools

- web_search: Research topics and trends
- write_file: Create content drafts
- notion_api: Read/write to CRM databases
- resend_api: Send email drafts for review
- buffer_api: Schedule social media posts
```

## Org Chart Design Patterns

### Flat Team (2-5 agents)
```
CEO
├── Engineer
├── Marketer
└── Researcher
```
Best for: Small, focused companies with clear task boundaries.

### Department Structure (6-15 agents)
```
CEO
├── CTO
│   ├── Backend Engineer
│   ├── Frontend Engineer
│   └── QA Agent
├── CMO
│   ├── Content Creator
│   ├── Social Media Manager
│   └── SEO Researcher
└── COO
    ├── Operations Agent
    └── Customer Support
```
Best for: Multi-domain companies with distinct departments.

### Deep Specialization (15+ agents)
```
CEO
├── CTO
│   ├── Tech Lead
│   │   ├── Dev Agent 1
│   │   ├── Dev Agent 2
│   │   └── Dev Agent 3
│   ├── QA Lead
│   │   ├── Unit Test Agent
│   │   └── Integration Test Agent
│   └── DevOps Agent
├── CMO
│   ├── Content Lead
│   │   ├── Writer Agent
│   │   └── Editor Agent
│   └── Growth Lead
│       ├── SEO Agent
│       └── Ads Agent
└── CFO
    ├── Analytics Agent
    └── Reporting Agent
```
Best for: Large-scale autonomous operations.

## Delegation Model

- Work flows DOWN the hierarchy: CEO → Directors → Specialists
- Status/blockers flow UP: Specialists → Directors → CEO
- Each agent can only delegate to its direct reports
- Agents NEVER look for unassigned work — only work on assigned tasks
- CEO receives high-level goals and decomposes into department-level tasks

## Model Selection Strategy

| Role | Recommended Model | Why |
|------|------------------|-----|
| CEO | Claude Opus | Complex reasoning, strategy, delegation |
| Directors (CTO/CMO/COO) | Claude Opus or Sonnet | Balance of reasoning and cost |
| Specialists | Claude Sonnet | Cost-effective for focused tasks |
| Simple workers | Claude Haiku | Fast, cheap for routine operations |

## Prompt Template Variables

Templates support these substitutions:
- `{{agentId}}` — The agent's unique ID
- `{{companyId}}` — The company's unique ID
- `{{runId}}` — Current heartbeat run ID
- `{{agent.name}}` — Agent's display name
- `{{company.name}}` — Company's display name

## Agent Lifecycle

1. **Hired** — Created in the org chart with role and configuration
2. **Active** — Heartbeats enabled, agent wakes and works on schedule
3. **Throttled** — Budget at 80%+, agent focuses on critical tasks only
4. **Paused** — Budget at 100% or manually paused by board
5. **Terminated** — Removed from the org (board decision)
