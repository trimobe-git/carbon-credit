# Paperclip Adapters

## Overview

Adapters are the bridge between Paperclip's control plane and the actual AI agents. They handle runtime-specific concerns like authentication, session management, and tool injection. The rule: **"If it can receive a heartbeat, it's hired."**

## Available Adapters

| Adapter | Type | Best For |
|---------|------|----------|
| **Claude Local** | Built-in | Primary agent runtime using Claude Code CLI |
| **Codex Local** | Built-in | OpenAI Codex-based agents |
| **Gemini Local** | Built-in | Google Gemini-based agents |
| **HTTP** | Built-in | External services via webhooks |
| **Process** | Built-in | Shell/Bash command execution |

## Claude Local Adapter (Primary)

The most commonly used adapter, connecting to Claude Code CLI.

### Configuration Fields

| Field | Type | Description |
|-------|------|-------------|
| `cwd` | string | Working directory for the agent (absolute path; auto-created if missing) |
| `model` | string | Claude model variant (e.g., `claude-sonnet-4-6`, `claude-opus-4-6`) |
| `promptTemplate` | string | Prompt used for all runs (supports variable substitution) |
| `env` | object | Environment variables (supports secret references) |
| `timeoutSec` | number | Maximum execution time per heartbeat |
| `graceSec` | number | Grace period before force termination |
| `maxTurnsPerRun` | number | Agentic turn limit per heartbeat |
| `dangerouslySkipPermissions` | boolean | Dev-only: bypass permission checks |

### Session Management
- **Persists session IDs** between heartbeats
- On next wake, **resumes existing conversation** with full context
- Session resume is **cwd-aware**: directory changes trigger fresh sessions
- Unknown session errors → automatic retry with new session

### Skill Injection
- Creates a **temporary directory with symlinks** to Paperclip skills
- Passes via `--add-dir` flag to Claude Code CLI
- Skills become discoverable without polluting agent's working directory

### Authentication
Requires `ANTHROPIC_API_KEY` set either:
- In the environment directly
- In the agent's `env` configuration (with optional secret references)

### Environment Validation
Test command: `claude --print - --output-format stream-json --verbose` with a "hello" prompt.

### Prompt Template Variables
- `{{agentId}}` — Agent's unique ID
- `{{companyId}}` — Company's unique ID
- `{{runId}}` — Current heartbeat run ID
- `{{agent.name}}` — Agent's display name
- `{{company.name}}` — Company's display name

### Example Configuration
```yaml
adapter: claude-local
config:
  cwd: /workspace/content-agent
  model: claude-sonnet-4-6
  promptTemplate: |
    You are {{agent.name}} working for {{company.name}}.
    Your role is to create content for the carbon credit newsletter.
    Follow your HEARTBEAT.md checklist for every execution.
  env:
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_KEY }}
    NOTION_TOKEN: ${{ secrets.NOTION_TOKEN }}
  timeoutSec: 300
  graceSec: 30
  maxTurnsPerRun: 25
```

## HTTP Adapter

For integrating external services and webhooks:
- Send heartbeat payloads to any HTTP endpoint
- Receive structured responses
- Useful for: existing APIs, microservices, custom runtimes

## Process Adapter

For shell/bash-based agents:
- Execute shell commands as heartbeat actions
- Useful for: scripts, cron-like tasks, system operations
- Simpler than full AI agents — good for deterministic workflows

## Codex Local Adapter

For OpenAI Codex-based agents:
- Similar pattern to Claude Local
- Requires `OPENAI_API_KEY`

## Gemini Local Adapter

For Google Gemini-based agents:
- Similar pattern to Claude Local
- Requires Google AI API key

## Multi-Provider Strategy

Paperclip supports mixing adapters within the same company:

```
CEO (Claude Opus) → Strategic reasoning
├── CTO (Claude Sonnet) → Technical decisions
│   ├── Dev Agent (Codex) → Code generation
│   └── QA Agent (Claude Haiku) → Test execution
├── CMO (Claude Sonnet) → Marketing strategy
│   └── Researcher (Gemini) → Web research with grounding
└── Ops Agent (Process/Bash) → System maintenance scripts
```

This enables cost optimization and capability matching per role.
