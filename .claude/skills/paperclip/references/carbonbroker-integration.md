# TRIMOBE CarbonBroker × Paperclip Integration Strategy

## Context

TRIMOBE CarbonBroker is an AI-powered carbon credit brokerage connecting Brazilian agricultural cooperatives (sellers) with corporate buyers (ESG goals). It's operated as a one-person business (8-12h/week) relying heavily on AI automation. The project defines 3 automation "machines" that map naturally to Paperclip companies.

## Architecture: 3 Machines → 3 Companies

Paperclip's multi-company support allows running all 3 machines from a single deployment with complete data isolation.

### Company 1: Curadoria & Conteúdo (Curation & Content)

**Mission**: Generate consistent, high-quality content about carbon credits in agribusiness.

**Flow**: Google Alerts → Classify/Score → Notion Curation DB → Generate Newsletter + LinkedIn → Resend/Buffer

**Proposed Org Chart**:
```
CEO (Content Director) — Claude Opus
├── Curator Agent — Claude Sonnet
│   ├── Monitors Google Alerts and news feeds
│   ├── Classifies relevance (0-100 score)
│   └── Writes to Notion Curation database
├── Newsletter Writer — Claude Sonnet
│   ├── Reads curated content from Notion
│   ├── Drafts "Carbono no Agro" newsletter
│   └── Outputs React Email template for review
├── LinkedIn Creator — Claude Sonnet
│   ├── Creates LinkedIn posts from curated content
│   ├── Follows content calendar
│   └── Schedules via Buffer API
└── Editor Agent — Claude Sonnet
    ├── Reviews all content for quality and tone
    ├── Ensures pt-BR language standards
    └── Flags for human approval before publishing
```

**Budget Estimate**: ~$30-50/month (Sonnet-heavy, low Opus usage)

**Heartbeat Schedule**:
- Curator: Every 4 hours (monitor new alerts)
- Newsletter Writer: Weekly (draft newsletter)
- LinkedIn Creator: Daily (create/schedule posts)
- Editor: Triggered after content creation tasks

### Company 2: Prospecção Contínua (Continuous Prospecting)

**Mission**: Continuously discover and qualify cooperatives and corporate buyers for carbon credit deals.

**Flow**: Scheduled search → Research → Deduplicate vs Notion → Generate outreach → Insert into CRM

**Proposed Org Chart**:
```
CEO (Prospecting Director) — Claude Opus
├── Research Agent — Claude Sonnet + Gemini (web search)
│   ├── Searches for cooperatives by sector (coffee, soy, livestock)
│   ├── Identifies corporations with ESG commitments
│   └── Extracts contact information and key data
├── Dedup Agent — Claude Haiku
│   ├── Checks new leads against Notion CRM
│   ├── Merges duplicates
│   └── Flags truly new prospects
├── Outreach Writer — Claude Sonnet
│   ├── Generates personalized outreach messages
│   ├── Adapts tone for cooperatives vs corporates
│   └── Drafts email sequences for human review
└── CRM Updater — Process/Bash adapter
    ├── Writes qualified leads to Notion databases
    ├── Updates pipeline stages
    └── Maintains data consistency
```

**Budget Estimate**: ~$40-60/month (Gemini for web search is separate cost)

**Heartbeat Schedule**:
- Research Agent: Daily (new prospect searches)
- Dedup Agent: After each research run (triggered)
- Outreach Writer: Daily (draft outreach for new leads)
- CRM Updater: After outreach drafts (triggered)

### Company 3: Inteligência Pós-Chamada (Post-Call Intelligence)

**Mission**: Extract maximum value from every sales call through automated analysis and follow-up.

**Flow**: Recording → Whisper transcription → Structured extraction → CRM update → Follow-up draft

**Proposed Org Chart**:
```
CEO (Intelligence Director) — Claude Opus
├── Transcription Agent — Process adapter (Whisper API)
│   ├── Receives call recordings
│   ├── Runs Whisper API transcription
│   └── Outputs timestamped transcript
├── Call Analyst — Claude Sonnet
│   ├── Extracts structured JSON from transcript
│   ├── Identifies: pain points, budget signals, timeline, decision makers
│   ├── Scores deal probability
│   └── Generates call summary
├── CRM Updater — Process/Bash adapter
│   ├── Updates Notion deal database with extracted data
│   ├── Moves pipeline stage based on analysis
│   └── Links transcript and summary to deal record
└── Follow-up Drafter — Claude Sonnet
    ├── Generates personalized follow-up email
    ├── References specific points from the call
    └── Suggests next steps for human review
```

**Budget Estimate**: ~$20-40/month (event-driven, lower volume)

**Heartbeat Schedule**:
- Transcription Agent: Triggered when new recording uploaded
- Call Analyst: Triggered after transcription completes
- CRM Updater: Triggered after analysis
- Follow-up Drafter: Triggered after CRM update

## Integration Points

### Notion (CRM Hub)
All 3 companies need Notion API access for:
- **Cooperatives DB**: Lead data, contact info, sector, status
- **Buyers DB**: Corporate info, ESG goals, budget, timeline
- **Deals DB**: Pipeline stages, deal value, probability
- **Curation DB**: Curated content, scores, publication status
- **Content DB**: Newsletter drafts, LinkedIn posts, scheduled content

Set `NOTION_TOKEN` as a shared secret across companies.

### External APIs
| Service | Used By | Purpose |
|---------|---------|---------|
| Claude API | All agents | Primary AI runtime |
| Gemini API | Research Agent | Web search with grounding |
| Whisper API | Transcription Agent | Call transcription |
| Resend | Newsletter Writer | Email delivery |
| Buffer | LinkedIn Creator | Social scheduling |
| Google Alerts | Curator Agent | Content monitoring |

### Human-in-the-Loop Checkpoints
Aligned with CarbonBroker's "AI prepares, human decides" principle:
1. **Newsletter publication** — Human reviews before sending via Resend
2. **LinkedIn posts** — Human approves before Buffer schedules
3. **Outreach emails** — Human reviews personalized messages before sending
4. **Follow-up emails** — Human reviews and may edit before sending
5. **Deal pipeline changes** — Human validates significant stage changes

## Implementation Roadmap

### Phase 1: Content Machine (Week 1-2)
- Set up Paperclip locally
- Create Company 1 (Curation & Content)
- Start with Curator + Newsletter Writer only
- Validate Notion integration
- Test heartbeat cycles

### Phase 2: Prospecting Machine (Week 3-4)
- Create Company 2 (Continuous Prospecting)
- Start with Research Agent + CRM Updater
- Add Dedup Agent and Outreach Writer
- Validate deduplication logic

### Phase 3: Post-Call Intelligence (Week 5-6)
- Create Company 3 (Post-Call Intelligence)
- Set up Whisper integration
- Test full pipeline: recording → transcript → analysis → CRM → follow-up

### Phase 4: Optimization (Week 7+)
- Review budgets and adjust
- Fine-tune heartbeat frequencies
- Add/remove agents based on actual workload
- Consider deploying to VPS for 24/7 operation

## Cost Projection

| Company | Monthly Estimate | Primary Driver |
|---------|-----------------|----------------|
| Content | $30-50 | Daily LinkedIn + weekly newsletter |
| Prospecting | $40-60 | Daily research + outreach generation |
| Post-Call | $20-40 | Event-driven, ~10-20 calls/month |
| **Total** | **$90-150/month** | Within zero-investment constraint if using API credits wisely |

## Key Considerations

1. **Language**: All agent prompts, SOUL.md, and ROLE.md files must enforce pt-BR for content generation
2. **Tone**: Different tone for cooperatives (informal, agricultural) vs corporates (formal, ESG/sustainability)
3. **Budget**: Start with minimal budgets and increase based on demonstrated ROI
4. **Monitoring**: Use Paperclip's dashboard to track cost per lead, cost per content piece
5. **Scaling**: Begin with Company 1 (lowest risk, highest visibility) before expanding
