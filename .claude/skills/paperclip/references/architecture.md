# Paperclip Architecture

## Two-Layer Design

### Control Plane (Paperclip)
The central management system that handles:
- **Agent Registry** — Who is hired, their roles, and configurations
- **Org Charts** — Hierarchical relationships and reporting lines
- **Task Assignment** — Issue creation, checkout, status tracking
- **Budget Tracking** — Token-level cost monitoring per agent/project/goal
- **Goals** — North star objectives that all work traces back to
- **Monitoring** — Real-time dashboard with full observability

### Execution Services (Adapters)
External agents that report back to the control plane:
- Each agent connects via an **adapter** (Claude Local, Codex, HTTP, etc.)
- Adapters handle runtime-specific concerns (session management, auth, tool injection)
- Agents are stateless between heartbeats but can resume sessions (adapter-dependent)
- The control plane doesn't care how agents think — only that they report results

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Backend | Node.js server |
| Frontend | React UI (dashboard) |
| Database | PostgreSQL (embedded locally; external for production) |
| File Storage | Local filesystem (configurable for cloud) |
| Package Manager | pnpm 9.15+ |
| Runtime | Node.js 20+ |

## Core Architectural Patterns

### Atomic Execution
- Task **checkout** prevents two agents from working on the same issue
- Budget enforcement is atomic — no overspend possible
- 409 Conflict response means another agent owns the task

### Persistent State
- Claude Local adapter persists session IDs between heartbeats
- On next wake, resumes existing conversation with full context
- Session resume is cwd-aware: directory changes trigger fresh sessions
- Unknown session errors trigger automatic retry with new session

### Goal Ancestry
- Every task carries the full context chain: Company Mission → Goal → Project → Issue → Subtask
- Agents understand the "why" behind their work, not just the "what"
- Enables autonomous decision-making aligned with company objectives

### Runtime Skill Injection
- Skills are injected at runtime via symlinked directories
- Claude Local adapter creates temp dir with symlinks, passes via `--add-dir`
- Agents learn new capabilities without retraining or redeployment
- Skills are discoverable without polluting the agent's working directory

### Multi-Company Isolation
- Single deployment supports unlimited companies
- Complete data isolation between companies
- Separate agents, goals, budgets, and audit trails per company
- One control plane manages the entire portfolio

## Configuration Versioning
- All config changes are revisioned
- Bad changes can be rolled back safely
- Approval gates enforce governance on critical changes
- Full audit trail of who changed what and when

## Deployment Modes

| Mode | Description |
|------|-------------|
| **Local (default)** | Embedded Postgres, file storage, trusted loopback |
| **LAN** | `--bind lan` — LAN authentication for local network access |
| **Tailnet** | `--bind tailnet` — Tailscale integration for remote access |
| **Cloud/Production** | External PostgreSQL, cloud storage, full auth |
| **Docker** | Containerized deployment |

## Telemetry
- Anonymous usage telemetry enabled by default
- Disable via: `PAPERCLIP_TELEMETRY_DISABLED=1`, `DO_NOT_TRACK=1`, or config `telemetry.enabled: false`
- No personal data, content, or secrets collected
