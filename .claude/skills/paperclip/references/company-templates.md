# Company Templates

## Overview

The `paperclipai/companies` repository provides **16 pre-built company templates** with **440+ specialized agents** and **500+ battle-tested skills**. Each template is a fully configured AI team ready to import and run.

## Available Templates

| Company | Agents | Skills | Focus |
|---------|--------|--------|-------|
| **GStack** | 5 | 27 | Engineering with cognitive modes |
| **Superpowers Dev Shop** | 4 | 14 | Software development lifecycle |
| **Agency Agents** | 167 | — | Multi-division AI agency (engineering, design, marketing, product, sales, QA, ops, games, spatial computing) |
| **Aeon Intelligence** | 4 | 32 | Autonomous research automation |
| **AgentSys Engineering** | 5 | 14 | Full-stack software delivery |
| **ClawTeam Capital** | 7 | 1 | Investment analysis |
| **ClawTeam Engineering** | 5 | 1 | Agentic software teams |
| **ClawTeam Research Lab** | 4 | 1 | ML research automation |
| **Donchitos Game Studio** | 48 | 38 | Game development |
| **Fullstack Forge** | 49 | 66 | Multi-language dev consultancy (12 languages, 66 skills) |
| **K-Dense Science Lab** | 54 | 177 | Multidisciplinary scientific research |
| **MiniMax Studio** | 5 | 10 | Digital production services |
| **Product Compass Consulting** | 48 | 65 | Product management services |
| **RedOak Review** | 5 | 6 | Code quality and security auditing |
| **TACHES Creative** | 6 | 35 | Creative strategy and frameworks |
| **Trail of Bits Security** | 28 | 35 | Security auditing and verification |

## Template Directory Structure

Each company template contains:

```
company-name/
├── COMPANY.md        # Metadata, mission, goals
├── README.md         # Detailed documentation
├── .paperclip.yaml   # Platform configuration
├── agents/           # Role-specific agent configurations
│   ├── ceo.md
│   ├── cto.md
│   ├── engineer-1.md
│   └── ...
└── skills/           # Available workflow skills
    ├── skill-1/
    ├── skill-2/
    └── ...
```

## Importing a Template

```bash
# Import from local path
npx paperclipai company import --from ./trail-of-bits-security

# Import from the companies repo
git clone https://github.com/paperclipai/companies.git
npx paperclipai company import --from ./companies/fullstack-forge
```

The imported company gets its own isolated workspace with all pre-configured agents and skills intact.

## Creating Custom Companies

### Using the Company Creator Skill

```bash
npx skills install paperclipai/companies/skills/company-creator
```

The company-creator skill provides guided company creation:
1. **Interview** — Asks about your business goals, team structure, and workflows
2. **Scaffold** — Generates complete company package (COMPANY.md, agents/, skills/)
3. **Configure** — Sets up org chart, budgets, and heartbeat schedules
4. **Output** — Writes configuration to specified location

### Manual Creation

Create the directory structure manually:

1. **COMPANY.md** — Define mission, goals, and metadata:
```markdown
---
name: my-content-agency
description: AI-powered content marketing agency
---

# My Content Agency

## Mission
Generate high-quality content that drives organic growth.

## Goals
- Publish 4 newsletter editions per month
- Create 20 LinkedIn posts per month
- Maintain 95% content quality score
```

2. **Agent files** in `agents/` directory
3. **Skills** in `skills/` directory
4. **.paperclip.yaml** for platform-specific config

## Templates Relevant for CarbonBroker

For the TRIMOBE CarbonBroker project, consider these as starting points:

| Template | Relevance |
|----------|-----------|
| **TACHES Creative** | Content strategy and creation (newsletter, LinkedIn) |
| **Agency Agents** | Full agency structure with marketing division |
| **Aeon Intelligence** | Research automation (market intelligence, prospecting) |
| **Product Compass Consulting** | Product management and strategy |

These can be imported and then customized for the carbon credit brokerage domain.

## Clipmart (Coming Soon)

Clipmart is a marketplace for buying and selling AI-agent companies:
- Browse pre-built company templates
- One-click import with full org structures, agent configs, and skills
- Repository: `paperclipai/clipmart`
- Each template comes with complete isolation on import
