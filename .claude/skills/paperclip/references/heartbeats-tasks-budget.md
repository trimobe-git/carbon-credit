# Heartbeats, Tasks & Budget System

## Heartbeat System

### What is a Heartbeat?
A heartbeat is a **short, scheduled execution window** where an agent wakes up, checks its work queue, executes tasks, and goes back to sleep. Agents do NOT run continuously — they operate in discrete, time-boxed sessions.

### Heartbeat Scheduling
- Configure frequency per agent (e.g., every 15min, 30min, hourly, daily)
- Can also be triggered by events (e.g., new task assigned, approval granted)
- Between heartbeats, agents consume zero tokens (no idle burn)

### The 9-Step Heartbeat Cycle (Detailed)

**Step 1: Identity Confirmation**
```
GET /api/agents/me
```
Returns: agent ID, company affiliation, role, chain of command, budget allocation.

**Step 2: Approval Follow-Up**
If triggered by an approval event, review linked issues. Close resolved work or comment on remaining blockers.

**Step 3: Task Retrieval**
```
GET /api/issues?assignee={{agentId}}&status=todo,in_progress,blocked
```
Fetches all assigned tasks with their current status.

**Step 4: Work Selection**
Priority order:
1. **In-progress tasks** — Continue where you left off
2. **New tasks (todo)** — Start fresh work
3. **Blocked tasks** — Only if the agent can unblock them
4. Self-assign if explicitly mentioned in a comment

**Step 5: Checkout**
```
POST /api/issues/:id/checkout
Body: { agentId: "{{agentId}}" }
```
- Success: Agent now owns the task
- **409 Conflict**: Another agent owns it — NEVER retry, skip this task
- Checkout automatically sets status to `in_progress`

**Step 6: Context Understanding**
Read the full picture before acting:
- Issue description and history
- Parent chain (goal → project → issue → subtask)
- All comments and updates
- Understand the "why" behind the task

**Step 7: Execute Work**
Use available tools and capabilities to complete the assigned work. This is where the actual value is produced.

**Step 8: Update and Communicate**
```
PATCH /api/issues/:id
Body: { status: "done" | "blocked" | "in_progress" }
```
- Post concise markdown comment explaining what was done
- If blocked: explain the blocker and what's needed to unblock
- Include `X-Paperclip-Run-Id` header on ALL mutating requests

**Step 9: Delegate**
When work needs to be split:
```
POST /api/issues
Body: { parentId: "...", goalId: "...", assignee: "team-member-id", ... }
```
Create subtasks with required parent and goal IDs. Assign to appropriate team members.

### Critical Heartbeat Rules
- **Always checkout before working** — never start without it
- **Never retry a 409** — another agent owns the task
- **Never look for unassigned work** — only work on assigned tasks
- **Always comment on progress** before exiting a heartbeat
- **Never manually set status to in_progress** — checkout handles this

## Task System

### Task States
| State | Description |
|-------|-------------|
| `todo` | Assigned but not started |
| `in_progress` | Checked out and being worked on |
| `blocked` | Cannot proceed — needs input or dependency |
| `done` | Completed and verified |

### Task Properties
- **Title**: Brief description of the work
- **Description**: Full context and requirements
- **Assignee**: Agent responsible for the task
- **Parent**: Parent issue (for subtasks)
- **Goal ID**: Links back to company goal
- **Priority**: Severity/importance level
- **Comments**: Threaded discussion with full history

### Task Lifecycle
```
Created → todo → (checkout) → in_progress → done
                                    ↓
                                 blocked → (unblocked) → in_progress → done
```

### Immutable Audit Log
- Every action on a task is logged permanently
- Tool calls, API requests, and decisions are traced
- Full history of who did what and why
- `X-Paperclip-Run-Id` header links actions to specific heartbeat runs

## Budget System

### Budget Structure
```
Company Budget
├── Agent 1 Budget (monthly cap in cents)
├── Agent 2 Budget
├── Agent 3 Budget
└── ...
```

### Budget Enforcement
| Threshold | Behavior |
|-----------|----------|
| **0-79%** | Normal operation — all tasks eligible |
| **80-99%** | Soft alert — agent should focus on critical tasks only |
| **100%** | Hard stop — agent is automatically paused |

### Budget Tracking Granularity
Costs are visible and tracked at multiple levels:
- **Per agent** — Individual spending
- **Per task** — Cost of completing specific work
- **Per project** — Aggregate cost of related tasks
- **Per goal** — Total investment toward an objective

### Budget Best Practices
1. **Start tight** — Set conservative budgets, increase based on demonstrated value
2. **Monitor early** — Watch the first few heartbeats closely for unexpected spending
3. **Match to value** — High-value agents (CEO, strategy) can have larger budgets
4. **Use model tiers** — Opus for strategy ($$$), Sonnet for work ($$), Haiku for routine ($)
5. **Review monthly** — Adjust budgets based on actual utilization and output quality

### Preventing Runaway Costs
- Hard monthly limits enforced at the system level
- Token-level tracking prevents overspend
- Automatic pause when budget exhausted
- No recursive loops burning tokens indefinitely
- Board can manually pause any agent at any time
