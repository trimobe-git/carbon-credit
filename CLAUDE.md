# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TRIMOBE CarbonBroker is an AI-powered carbon credit brokerage connecting Brazilian agricultural cooperatives (sellers) with corporate buyers (ESG goals). Operated as a one-person business (8-12h/week), it relies heavily on AI automation for prospecting, content generation, and pipeline management.

This repository currently contains planning/context documents in `_contexto/` and will progressively hold all code artifacts for the project.

## Context Documents

The `_contexto/` folder contains the full business and technical specification. Read these before making architectural decisions:

- `01_Plano_de_Negocios_CarbonBroker.md` — Business plan, revenue model (commission-based), target metrics
- `02_PRD_CarbonBroker.md` — Product requirements: AI agents, sales materials, content engine, CRM, market intelligence
- `03_Estrategia_Marketing_Conteudo.md` — Marketing strategy, newsletter "Carbono no Agro", LinkedIn content plan
- `04_Mapeamento_Nichos_Mercado.md` — Market mapping: cooperatives by sector (coffee, soy, livestock), buyer segments
- `05_Estrategia_Tecnologia_IA.md` — Tech stack, AI strategy, automation architecture ("3 machines")
- `06_Guia_Tarefas_Implementacao.md` — Implementation roadmap with weekly tasks

## Planned Tech Stack

| Layer | Technology |
|---|---|
| Landing page | Next.js + Tailwind CSS, deployed on Vercel |
| Email/Newsletter | Resend + React Email templates |
| AI (primary) | Claude API (Sonnet 4) — prospecting agents, material generation, post-call analysis, matching, newsletter drafts |
| AI (secondary) | Gemini API — web search, PDF processing, cross-validation |
| Transcription | Whisper API (OpenAI) |
| Automation/Orchestration | n8n (self-hosted on Railway/Render) |
| CRM/Hub | Notion (databases for Cooperatives, Buyers, Deals, Curation, Content) |
| Social scheduling | Buffer |
| Repository | GitHub |

## Automation Architecture ("3 Machines")

1. **Curation & Content** — Google Alerts → n8n → Claude API (classify/score) → Notion → Claude API (generate newsletter + LinkedIn posts) → Resend/Buffer
2. **Continuous Prospecting** — n8n cron → Claude API + web search → deduplicate against Notion → generate outreach → insert into Notion CRM
3. **Post-Call Intelligence** — Call recording → Whisper API (transcription) → Claude API (structured JSON extraction) → Notion CRM update + follow-up draft

## Language

All business content, outreach, newsletters, and user-facing materials must be written in **Brazilian Portuguese (pt-BR)**. Code comments and technical documentation can be in English or Portuguese — follow the convention of the file being edited.

## Key Constraints

- **Zero upfront investment** — use only free tiers and low-cost APIs
- **Commission-only model** — no SaaS, no subscription fees to clients
- **Human-in-the-loop** — AI prepares, human decides. No critical output (proposals, contracts, emails to clients) goes out without human review
- **Target audience is dual-sided**: cooperatives (sellers) and corporations with ESG goals (buyers) — tone and materials differ for each
