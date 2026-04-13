# Getting Started with Paperclip

## Requirements
- **Node.js** 20+
- **pnpm** 9.15+
- An API key for your chosen AI provider (e.g., `ANTHROPIC_API_KEY` for Claude)

## Quick Start (Recommended)

```bash
npx paperclipai onboard --yes
```

This single command:
1. Launches an interactive setup wizard in your browser
2. Creates an embedded PostgreSQL database automatically
3. Configures authentication
4. Walks you through creating your first company and CEO agent
5. Starts the API server at `http://localhost:3100`

### Bind Modes

```bash
npx paperclipai onboard --yes              # Local only (trusted loopback)
npx paperclipai onboard --yes --bind lan   # LAN access with authentication
npx paperclipai onboard --yes --bind tailnet  # Remote via Tailscale
```

## Manual Setup (From Source)

```bash
git clone https://github.com/paperclipai/paperclip.git
cd paperclip
pnpm install
pnpm dev
```

The server starts at `http://localhost:3100` with an auto-created embedded PostgreSQL.

## Development Commands

```bash
pnpm dev              # Full dev with watch mode (server + UI)
pnpm dev:once         # Single run without file watching
pnpm dev:server       # Server-only mode (no UI)
pnpm build            # Build all packages
pnpm typecheck        # TypeScript type checking
pnpm test:run         # Execute test suite
pnpm db:generate      # Create new DB migration
pnpm db:migrate       # Apply pending migrations
```

## Six-Step Company Setup

### Step 1: Create Company
Provide a name and optional description through the web UI at `http://localhost:3100`.

### Step 2: Define North Star Goals
Set specific, measurable objectives:
- "Achieve $1M MRR by Q4"
- "Generate 100 qualified leads per month"
- "Ship bug-free code within 1 hour of issue creation"

### Step 3: Hire CEO Agent
Configure the first agent:
- **Name**: e.g., "atlas" or "chief"
- **Role**: "ceo"
- **Adapter**: Claude Local (recommended default)
- **Prompt**: "Review company health, set strategy, delegate work to reports"
- **Monthly Budget**: Set in cents (e.g., 5000 = $50/month)
- **Model**: Claude Opus for strategic thinking

### Step 4: Build Org Chart
Add department heads reporting to the CEO:
- **CTO** — Engineering oversight, technical decisions
- **CMO** — Marketing, content, outreach
- **COO** — Operations, processes, logistics

Each agent reports to exactly one manager (strict hierarchy).

### Step 5: Configure Budgets
Set at both company and individual agent levels:
- **Soft alert**: 80% utilization — agent focuses on critical tasks only
- **Hard stop**: 100% — agent pauses automatically
- Budget is tracked per agent, per task, per project, and per goal

### Step 6: Activate
Enable heartbeats to start agent activity. Monitor progress through the dashboard.

## Importing a Company Template

Instead of building from scratch, import a pre-built template:

```bash
# Import a specific template
npx paperclipai company import --from ./path-to-template

# Install the company-creator skill for guided setup
npx skills install paperclipai/companies/skills/company-creator
```

## Environment Variables

| Variable | Purpose |
|----------|---------|
| `ANTHROPIC_API_KEY` | Claude API authentication |
| `OPENAI_API_KEY` | OpenAI/Codex authentication |
| `PAPERCLIP_TELEMETRY_DISABLED=1` | Disable telemetry |
| `DO_NOT_TRACK=1` | Standard telemetry opt-out |

## Production Deployment Considerations

- **Database**: Point to external PostgreSQL instance (not embedded)
- **Storage**: Configure cloud storage (not local filesystem)
- **Auth**: Use proper authentication (not trusted loopback)
- **Monitoring**: Set up alerting for agent failures and budget thresholds
- **Backup**: Regular database backups for audit trail preservation
